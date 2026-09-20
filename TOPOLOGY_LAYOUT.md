# grAIph Topology-Derived Visual Layout

**Status:** Implemented behind explicit experimental controls

**Reviewed:** 2026-09-20

This document describes deterministic visual placement derived from graph topology. It does not describe graph mutation, generation ordering authority, or host persistence, and it does not claim that visual layout improves model quality.

---

## Three related but separate concerns

| Concern | Output | Authority |
|---|---|---|
| Topology analysis | Components, waves, cycle analysis, immutable topology plan | Planning only |
| Graph reconciliation | Candidate edge additions/removals applied to detached state | Candidate mutation; host commits |
| Visual layout | Deterministic node coordinates and convergence metadata | Presentation only |

The three can consume related graph information, but success in one does not prove success in another.

See [Evidence and topology reconciliation](./docs/evidence-reconciliation.md) for the graph transaction boundary.

---

## Why deterministic visual layout matters

A visual software environment benefits from stable placement:

- equivalent graph inputs should not jump unpredictably between sessions;
- related units should remain inspectable across topology changes;
- viewer-local viewport state should remain distinct from durable graph structure;
- layout diagnostics should be reproducible without asking a model to choose coordinates.

These are presentation and inspection goals. File placement, import paths, and generation context are governed by separate contracts.

---

## Current controls

The implementation provides explicit controls for:

- enabling the topology-derived strategy;
- logging decisions without applying them;
- selecting the layout strategy;
- using precomputed layout information where applicable.

Topology-derived placement is not presented as a universally optimal default.

---

## Algorithm

### Phase 1 — Wave assignment

Use the analyzed graph's topological waves. The wave index supplies the primary horizontal coordinate:

~~~text
x(node) = wave index
~~~

Strongly connected components and cycle decisions belong to topology analysis before ordinary wave placement.

### Phase 2 — Deterministic initial ordering

Within each wave, sort stable node identities and assign deterministic initial vertical positions. Isolated nodes remain distinct from nodes connected across waves.

### Phase 3 — Cross-wave relaxation

Cross-wave edges attract related nodes toward compatible vertical positions. Connection counts are aggregated by wave before the configured relaxation function is applied.

Iteration order is stable and bounded by an explicit convergence threshold and maximum iteration count. “Gravity” is an implementation metaphor, not a claim about software or model physics.

### Phase 4 — Stability and core detection

The engine records movement from initial positions and can identify high-connectivity core nodes while retaining isolated-node treatment. Optional trajectories and convergence metadata support diagnosis.

### Phase 5 — Placement

The selected strategy converts settled coordinates into rendered positions while preserving deterministic tie ordering.

---

## Inputs and outputs

Inputs include:

- graph nodes and edges;
- analyzed wave assignment;
- stable node identity and labels;
- edge multiplicity and cross-wave relationships;
- layout configuration.

Outputs include:

- deterministic node positions;
- wave and convergence metadata;
- optional debug trajectories;
- an apply or log-only layout decision.

The layout engine does not:

- generate source code;
- authorize graph mutations;
- validate model output;
- persist project state;
- decide benchmark eligibility.

---

## Host-independent viewer behavior

Committed graph state is shared, but viewport position and zoom are viewer-local. A layout command may update durable node positions through the graph command path; ordinary panning or zooming does not.

The latest bounded visual-host run preserved the native VS Code viewport while 35 Codex-browser-origin graph edits converged across three viewers. That result is synchronization evidence, not proof that the topology-derived layout strategy itself is superior.

---

## What remains open

- whether topology-derived placement reduces human inspection cost;
- which policies are most stable across graph shapes and sizes;
- whether layout commands remain responsive at larger fixed sizes;
- whether any generation effect exists after controlling for context and path policy;
- whether the strategy should become a default.

See [Research hypotheses](./RESEARCH_HYPOTHESES.md), [Host-independent visual runtime](./docs/visual-runtime.md), and [Current state](./CURRENT_STATE.md).
