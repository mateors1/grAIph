# TOPOLOGY_LAYOUT.md
# grAIph Topology-Driven Output Layout
# Feature branch: feature/topology-layout

---

## Status

Specification only. Not yet implemented.
Implementation begins after this document is reviewed and the branch is created.

Flag: `enableTopologyLayout: false` (opt-in, default false until benchmark validated)

---

## The Problem This Solves

grAIph currently delegates two structural decisions to the Coder agent:

1. Where does this file live in the output directory tree?
2. What import paths does it use to reference other files?

Both decisions are made by the model at generation time, from imperfect context.
The result is PHANTOM-IMPORT (model invents paths that don't exist) and inconsistent
directory placement across nodes generated in different pipeline runs.

The graph already encodes the information needed to make these decisions correctly —
dependency relationships, cluster structure, and bridge nodes are all computable from
the topology before any LLM runs. The compiler should make these decisions, not the model.

---

## Core Insight

Kahn's algorithm, already used to determine generation order, computes a natural
coordinate system for the graph. The wave layer is the X axis. Cross-wave edge
gravitational attraction determines the Y axis. Together they produce a deterministic
2D layout that encodes module boundaries and import semantics without any separate
clustering algorithm.

The same physics that governs generation order governs output structure.

---

## Existing Infrastructure

The wave computation is already implemented and can be reused directly:

- `BatchSkill.topoSortNodes()` — canonical Kahn implementation
- `MessageManager.computeTopologicalLayers()` — layer-based Kahn
- `TopologyScrutineerAgent` — stores `generationOrder: string[][]`
- `graiph_getGenerationOrder` tool — exposes wave layers via MCP

Phase 1 (X assignment) requires no new code. It reads the existing wave structure.

---

## The Algorithm

### Phase 1 — X Assignment

```
X(node) = wave_index(node)
```

Wave 0 = nodes with no upstream dependencies (sources, exporters with no imports).
Wave N = nodes whose earliest dependency resolves to wave N-1.
Nodes in the same wave are independent by definition — no mutual dependencies,
parallelizable, naturally co-located.

Source: existing `generationOrder` from TopologyScrutineer. No new computation.

---

### Phase 2 — Initial Y Assignment

Within each wave, sort nodes by node ID (lexicographic) for determinism.
Assign Y = 0, 1, 2, ... in sorted order.

This is the cold-start position. Relaxation will move nodes to equilibrium.
Sorting by ID guarantees identical starting state for the same graph.

---

### Phase 3 — Y Relaxation via Cross-Wave Gravitational Attraction

Cross-wave edges (edges connecting nodes in different waves) act as gravitational
fields. Each edge pulls its endpoints toward each other on the Y axis. Pull strength
grows with the number of edges between the same pair — like gravity, not like a
simple average.

**The gravity formula:**

```
cross_wave_neighbors(v) = { u : edge(v,u) exists AND wave(u) ≠ wave(v) }

weight(v, u) = edge_count(v, u) ^ gravity_exponent
             = edge_count(v, u) ^ 2   (default: square law)

Y_new(v) = Σ[ Y(u) * weight(v,u) ] / Σ[ weight(v,u) ]
           for all u in cross_wave_neighbors(v)
```

The square law means a node with 3 edges to cluster A and 1 edge to cluster B
is pulled toward A with 9× the force, not 3×. This produces tighter clusters
with cleaner band boundaries.

The formula is still a weighted average — so a node pulled in two directions
settles at the weighted midpoint rather than being arbitrarily forced into one
cluster. Ambiguous nodes settle in the overlap zone, which becomes its own band.
This is the compromise for folder structure: physics determines ambiguity,
not arbitrary assignment.

**Iteration:**

```
repeat up to MAX_ITERATIONS (default: 50):
  Y_prev = copy of all Y values
  for each node v sorted by node ID:          ← fixed order for determinism
    if cross_wave_neighbors(v) is not empty:
      Y(v) = gravity_formula(v)
  
  delta = max( |Y(v) - Y_prev(v)| ) for all nodes
  if delta < CONVERGENCE_THRESHOLD (default: 0.01):
    break
```

Fixed iteration order (by node ID) guarantees determinism.
Same graph + same edges = same relaxation trajectory = same final positions.
Every time.

**Isolated nodes:**

If `cross_wave_neighbors(v)` is empty, Y(v) is unchanged throughout all iterations.
Isolated nodes keep their initial position and live in their own band.

This is intentional. Isolated nodes are usually feature drift, incomplete features,
or vaguely related scripts. Their topological isolation is semantically meaningful.
They go to `/orphans/` — visible, not scattered.

---

### Phase 4 — Variance Calculation

Track each node's Y position across all iterations:

```
Y_trajectory(v) = [ Y(v) at iteration 0, 1, 2, ..., final ]

variance(v) = mean( (Y(v,i) - mean(Y_trajectory(v)))^2 )
              across all iterations i
```

Nodes that settle quickly → low variance.
Nodes pulled in multiple directions simultaneously → high variance.
They never fully converge because competing attractors keep pulling them.

High-variance nodes are structurally ambiguous: they bridge multiple clusters.
This is the betweenness centrality signal, computed organically from the
relaxation dynamics. No separate BC algorithm needed.

---

### Phase 5 — Core Node Identification

**Threshold: relative (top N% of variance across all nodes)**

```
DEFAULT_CORE_PERCENTILE = 85

variance_scores = [ variance(v) for all nodes ]
threshold = percentile(variance_scores, DEFAULT_CORE_PERCENTILE)

isCoreCandidate(v) = variance(v) >= threshold AND NOT isIsolated(v)
```

The top 15% highest-variance nodes are core candidates.
Isolated nodes (variance = 0) are explicitly excluded.

Core candidates are hoisted to `/core/` regardless of their wave or band.
Their wave-band assignment is overridden.

Configurable: `topologyLayout.coreThresholdPercentile` (default: 85).

---

### Phase 6 — Discretization and Band Assignment

After relaxation, Y values are continuous floats. Map to discrete bands:

```
BAND_WIDTH = 3.0  (configurable)

band(v) = floor( Y(v) / BAND_WIDTH )
```

Nodes in the same band belong to the same module directory.
Two nodes in the same wave and same band are co-located — correct, expected.

**Band naming:**

```
1. Collect all node labels in the band across all waves
2. Find the most common label suffix (Action, Service, Handler, View, Repository, etc.)
3. Lowercase, pluralize: "Action" → "actions", "Service" → "services"
4. Strip special characters, replace spaces with hyphens
5. If no common suffix: use "module-w{wave}-b{band}"
6. If name collision: prefix with wave index "w{wave}-{name}"
```

User override: any node can carry `moduleAlias: "custom-name"` which propagates
to all co-band nodes. User intent overrides inference.

---

### Phase 7 — Directory Assignment

```typescript
function assignDirectory(v: Node, coordinates: TopologicalCoordinates): string {
  if (coordinates.isCoreCandidate) {
    return `/${config.coreDirectory}/`;           // default: "/core/"
  }
  if (coordinates.isIsolated) {
    return `/${config.orphansDirectory}/`;         // default: "/orphans/"
  }
  const bandName = deriveBandName(coordinates.wave, coordinates.yBand, graph);
  return `/${config.modulesDirectory}/${bandName}/`; // default: "/modules/{name}/"
}
```

---

### Phase 8 — Import Path Resolution

Once every node has a directory, all import paths are pre-computed:

```typescript
function resolveImport(source: Node, target: Node): ResolvedImport {
  if (target.coordinates.isCoreCandidate) {
    return {
      resolvedPath: `@/${config.coreDirectory}/${target.filename}`,
      importType: "core"
    };
  }
  if (sameDirectory(source, target)) {
    return {
      resolvedPath: `./${target.filename}`,
      importType: "relative"
    };
  }
  return {
    resolvedPath: `@/${target.coordinates.outputDirectory}${target.filename}`,
    importType: "absolute"
  };
}
```

These resolved paths are stored in `ResolvedImport[]` on `GraphGenerationContract`.
The Coder receives them as a pre-resolved import map and cannot hallucinate alternatives.

The "REUSABLE EXPORTS (DO NOT REDECLARE)" prompt section (added in W1-T2) gains
a `resolvedPath` field alongside the symbol name. The Coder is told not just
what symbol exists but exactly where to import it from.

---

## Incremental Recomputation (Graph Expansion)

When new nodes or edges are added, layout needs to update without full restart.

### The Anchor Weight System

Nodes that have been generated carry a stability weight:

```
anchorWeight(v) = min(1.0, settledIterations(v) / MAX_ITERATIONS)

Y_anchored(v) = (gravity_pull(v) * (1 - anchorWeight(v))) +
               (Y_current(v)    *  anchorWeight(v))
```

A new node has `anchorWeight = 0` — relaxes freely.
A long-settled generated node has `anchorWeight → 1` — barely moves.

Adding one node to a large stable graph reorganizes only nodes directly connected
to the new node. Existing module assignments are preserved unless a new edge
creates very strong pull.

### Dirty Layout Detection

```
layoutHash(v) = hash(wave(v), yBand(v), isCoreCandidate(v), outputDirectory(v))
```

If `layoutHash` changes after recomputation: the node is layout-dirty.
Layout-dirty propagates through the import graph — same mechanism as code drift detection.

When a node's `outputDirectory` changes:
- All nodes that import FROM this node need their import paths updated
- All nodes that this node imports FROM need to be notified their path is referenced differently
- These nodes are marked dirty for regeneration

### Existing Codebase Compatibility

For imported nodes (`sourceOrigin: "imported"`):

```
Y_anchor(v) = Y position corresponding to the node's current directory path
anchorWeight(v) = 1.0  (fully anchored — will not move)
```

Imported nodes don't participate in Y relaxation unless the user explicitly enables
`allowLayoutMigration: true`. This allows adoption of topology layout incrementally:
new nodes use it, existing nodes keep their current structure.

When `allowLayoutMigration: true`, imported nodes participate with reduced anchor
weight (`anchorWeight = 0.7`) — they can move but with resistance.

---

## Data Structures

```typescript
interface TopologicalCoordinates {
  // X axis
  wave: number;

  // Y axis
  yPosition: number;           // continuous, post-relaxation
  yBand: number;               // discretized band index

  // Variance
  yVariance: number;           // trajectory variance
  yTrajectory?: number[];      // per-iteration Y values (debug mode only)

  // Classification
  isCoreCandidate: boolean;    // top N% variance
  isIsolated: boolean;         // no cross-wave edges

  // Output
  outputDirectory: string;     // "/core/" | "/modules/{name}/" | "/orphans/"
  moduleAlias?: string;        // user override

  // Pre-resolved imports
  resolvedImports: ResolvedImport[];
}

interface ResolvedImport {
  targetNodeId: string;
  symbol: string;
  resolvedPath: string;
  importType: "relative" | "absolute" | "core";
}

interface TopologyLayoutConfig {
  enabled: boolean;                      // default: false
  gravityExponent: number;               // default: 2
  bandWidth: number;                     // default: 3.0
  coreThresholdPercentile: number;       // default: 85
  maxIterations: number;                 // default: 50
  convergenceThreshold: number;          // default: 0.01
  allowLayoutMigration: boolean;         // default: false
  coreDirectory: string;                 // default: "core"
  modulesDirectory: string;              // default: "modules"
  orphansDirectory: string;              // default: "orphans"
  debugTrajectory: boolean;              // default: false (logs yTrajectory)
}
```

---

## Implementation Stages

### Stage 0 — Branch and spec ← YOU ARE HERE
- [ ] Create `feature/topology-layout` off main
- [ ] Commit TOPOLOGY_LAYOUT.md to branch root
- [ ] No implementation code

### Stage 1 — Standalone coordinate calculator
File: `packages/core/src/layout/TopologyLayoutEngine.ts`

- [ ] `computeWaves(graph)` — wraps existing Kahn, returns `Map<nodeId, wave>`
- [ ] `computeInitialY(waves)` — Phase 2, deterministic by node ID sort
- [ ] `relaxY(waves, edges, config)` — Phase 3-4, returns trajectories
- [ ] `computeVariance(trajectories)` — Phase 4
- [ ] `identifyCoreNodes(variance, config)` — Phase 5
- [ ] `assignBands(yPositions, config)` — Phase 6
- [ ] `assignDirectories(bands, coreNodes, config)` — Phase 7
- [ ] `resolveImports(directories, edges)` — Phase 8
- [ ] `computeGraphDiameter(nodes, edges)` → `number` — BFS from each node, max shortest path
- [ ] `computeLayout(graph, config)` — full pipeline (calls diameter check before Phase 6)

No pipeline integration yet. Standalone module only.

Tests: `packages/core/src/__tests__/topology-layout.test.ts`
- Verify VirtualView.ts identified as core candidate on TEM graph
- Verify plugin.ts identified as core candidate on TEM graph
- Verify bookmark manager demo produces 3-4 distinct module bands
- Verify determinism: same graph, 10 runs, identical coordinates

### Stage 2 — Validation via coordinate logging
- [ ] Add `enableTopologyLogging: true` flag (compute but don't use)
- [ ] Log coordinates to benchmark session output
- [ ] Run against TEM graph. Manually verify geometry matches semantic expectations.
- [ ] Adjust gravityExponent and bandWidth until geometry is correct.
- [ ] Document calibrated values in this spec.
- [ ] Verify TEM graph diameter value. If diameter < 4 → single module band (correct).
  Log should show: "graphDiameter=N, clusteringEnabled=false"
- [ ] Verify a synthetic 200-node graph with diameter ≥ 4 produces multiple bands.

### Stage 3 — Wire into GraphGenerationContract
- [ ] Add `topologyCoordinates?: TopologicalCoordinates` to contract type
- [ ] Populate when `enableTopologyLayout: true`
- [ ] Extend Coder prompt section at order 22: pre-resolved import map
- [ ] Extend output path instruction: use `outputDirectory` from coordinates
- [ ] Run benchmark flag-on vs flag-off. Measure PHANTOM-IMPORT delta.

### Stage 4 — Incremental recomputation
- [ ] Add `anchorWeight` to layout engine
- [ ] Add `layoutHash` to node state
- [ ] Wire into existing hash-based drift detection

### Stage 5 — Merge with flag off
- [ ] Merge into main, `enableTopologyLayout: false`
- [ ] No production behavior change

### Stage 6 — Flip default after validation
- [ ] `enableTopologyLayout: true` default
- [ ] V15+ benchmark story

---

## Open Questions

**Cycle members:**
Tarjan already detects cycles. Current behavior: cycle members appended at end
of Kahn sort. For layout: treat cycle members as a unit at the wave of the latest
member. Intra-cycle edges are same-wave (no Y pull). Cross-cycle edges treated normally.

**Multi-edge graphs:**
grAIph currently enforces one edge per source-target pair. Assume `edge_count = 1`
for all pairs until this changes.

**Subgraph nodes (FractalLevel.File):**
Layout engine operates on root-level nodes only (`!node.data.parentNodeId`).
File-level children inherit parent's coordinates. Intra-parent imports are
relative by definition.

**Band width calibration:**
`bandWidth = 3.0` is an estimate. Stage 2 logging will produce data to calibrate.
Expect to adjust after first TEM graph coordinate run with `clusteringEnabled=true`.

**Graph diameter threshold calibration:**
`MIN_CLUSTERING_DIAMETER = 4` is theoretically motivated and empirically supported by
the V17 benchmark (TEM graph produced semantically wrong clusters at diameter ≈ 3-4).
Stage 2 logging will output the actual TEM diameter. If the TEM graph has diameter 4
and produces correct single-module behavior, the default is validated. If diameter 3
and clustering still fires, lower the threshold to 3. If diameter 5 and correct, raise
the threshold or leave it — conservative thresholds are preferable to aggressive ones.

The threshold is the most empirically important calibration parameter in the algorithm.
Document the measured TEM diameter in this spec after Stage 2 validation.

**Filesystem constraints:**
Band names must be valid on Windows, macOS, Linux. The derivation function must
produce only `[a-z0-9-]` characters. Strip diacritics, special characters, spaces.

---

## Novelty Assessment

Components individually have precedent:
- Kahn's algorithm: Kahn (1962)
- Force-directed layout: Fruchterman-Reingold (1991)
- Betweenness centrality: Freeman (1977)
- Bazel/Buck: dependency-graph build ordering (not layout)

**The combination applied to LLM compiler output is novel:**
- Kahn waves as spatial X coordinate (not just ordering) — new
- Cross-wave edge gravity as Y relaxation field (not rendering physics) — new
- Betweenness centrality derived from relaxation variance (not explicit calculation) — new
- Pre-resolving import paths from graph coordinates before LLM generation — new
- Anchor weight system for incremental recomputation in a live graph editor — new
- Graph diameter threshold as a clustering activation gate — new: using graph topology
  to decide whether clustering is meaningful at all, rather than blindly applying it

Publishable: systems contribution (MSR/ICSE workshop tier).
Potentially patentable: the specific method claim of Kahn layer indices as spatial
coordinates with cross-layer edge attraction for compiler output structure.

The core idea was visualized in approximately 30 seconds when Louvain's
non-determinism was identified as a blocker — a direct response to a specific
constraint, not a pre-planned solution.

---

## Expected Benchmark Impact

**PHANTOM-IMPORT → near zero**
Import paths pre-computed from coordinates. Coder cannot invent a path outside
the coordinate system. Directly addresses V14 failure: DynamicSlotAction and
NumericEncoderAction importing from non-existent modules.

**INTERFACE-GUT → reduced**
Core candidates automatically identified before generation.
VirtualView.ts (gutted in V14) should be identified as core candidate.
Richer contracts can be applied to core candidates pre-generation.

**Directory consistency → deterministic**
Same graph always produces same directory structure.
Currently model-dependent and between-run variable.

**File stability → improved**
Pre-resolved imports eliminate one source of between-run output variance.
V14 showed 70% file stability. Import path determinism should push this higher.

**Graph diameter threshold → correct self-disabling at small scale**
V17 (first topology run, 37 files, TEM graph) produced semantically wrong clusters
because the graph was too dense for Y-relaxation to produce meaningful separation.
The diameter guard ensures the algorithm correctly falls back to single-module layout
for graphs that don't benefit from clustering — producing flat-by-role output that
matches what expert developers would choose.

Empirical finding from V17: topology-derived module layout requires graph diameter ≥ N
to produce semantically correct clusters. Below this threshold, the algorithm correctly
self-disables. This is a publishable empirical threshold finding — the first quantitative
characterization of when graph-first layout adds value over role-based layout.

---

*Authored: 2026-05-17*
*Visualized by Mateo Rendon Suarez in ~30 seconds*
*Status: Specification. Branch created 2026-05-17. V17 empirical findings incorporated 2026-05-18.*
*Next action: Implement W0-T5 (computeGraphDiameter + diameter guard in Phase 6), then W0-T3 (file duplication fix), then clean V18 benchmark.*
