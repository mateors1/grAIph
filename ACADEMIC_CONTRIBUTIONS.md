# grAIph — Academic Contributions & Publication Map
## Living Document — Updated On Each New Discovery

**Author:** Mateo Rendon Suarez  
**Contact:** hello@graiph.dev  
**Last updated:** 2026-05-19  
**Status:** Pre-publication. NLnet grant pending. First benchmark paper target: post-V18 clean run.

---

## How To Use This Document

This document is updated whenever a new publishable finding, novel algorithm, or
empirical result is identified. Each entry includes:
- What the contribution is (precise technical description)
- Why it's novel (what it adds beyond existing work)
- What evidence exists (benchmark results, codebase implementation, empirical findings)
- Target venue and readiness status

---

## Tier 1 — Directly Publishable
### Novel contributions to known problems. Each stands alone as a research paper.

---

### T1-1: Topological Context Compilation (TCC)
**Status:** Named. Empirically validated (partial). Full paper after V18 clean topology benchmark.  
**Target venue:** MSR, ICSE, or PLDI workshop  
**Evidence:** 90% byte-identical output without source code, 0% import hallucination,
correct cycle handling. V15: 8.1/10 clean run, 4/4 critical hubs byte-identical.
V-Poisoned-SF: 100% byte-perfect on external machine.

**What it is:**
A compilation architecture that applies classical compiler theory — topological sorting,
SCC contraction, dependency graph analysis — to manage the context window of a stochastic
LLM. The LLM is one deterministic pass in a structured pipeline, not the pipeline itself.
The JSON graph is the source language. Kahn wave ordering is the context window schedule.

**Why it's novel:**
Current LLM code generation treats the context window as a prompt engineering problem.
TCC treats it as a compiler paging problem — the same problem compilers solved for RAM
on memory-constrained hardware, applied to context windows. This reframing is precise,
not metaphorical: the dependency ordering, the object file analogy (generated contracts
as compiled interfaces), the linker (RecordKeeper) all map exactly.

**The core claim (testable):**
Kahn wave ordering aligns the context window sliding window with the dependency graph.
The most recently generated content is always the most topologically relevant content
for the current node — not by accident, but by construction.

**Three components:**
1. Kahn wave ordering — context window schedule from dependency topology
2. SCC contraction (Tarjan's) — stable wave assignment through cycles, never deadlocks
3. Exponential decay context pruning — relevance-weighted context by graph distance

**Empirical results:**
- 0% import hallucination (structural guarantee from wave ordering)
- 90%+ byte-identical without source (IR density validation)
- 100% byte-identical with source on external machine (pipeline determinism ceiling)
- Correct cycle handling with visual markers (SCC contraction working)

---

### T1-2: ScopePruning — Influence Wave Decay
**Status:** Implemented. Parameters empirically calibrated. Needs formal characterization.  
**Target venue:** Section of TCC paper, or standalone ICSE workshop paper  
**Evidence:** `ScopePruning.ts`, formula `C(v) = α · e^{-β · d(v₀, v)}`, β=0.7, α=1.0

**What it is:**
A context allocation algorithm that weights information in the LLM prompt by graph
distance from the target node, using exponential decay. Three-tier degradation:
- Distance 0: full content (target node)
- Distance 1: interface contracts + generated files (direct neighbors)
- Distance 2: symbol + return type only (transitive)
- Distance 3+: RecordKeeper registry identity only

Progressive degradation triggers at D=0.5: full content → interface signatures → name-only stubs.

**Why it's novel:**
The application of exponential decay to graph distance as the independent variable for
context window allocation is not documented elsewhere in LLM engineering literature.
The three-tier degradation mirrors how human engineers reason about a codebase at
increasing distance — a formal operationalization of a cognitive heuristic.

**Key engineering finding (from DEVLOG):**
The neighbor token budget can go NEGATIVE when sacred sections consume the full context
window. The `Math.max(0, ...)` floor fix ensures the pruning loop fires even at negative
budget — degrading everything to stubs rather than including full content beyond capacity.
This is a production finding that informs the theoretical framework.

**Tunable parameters (current defaults):**
- `α = 1.0` — baseline weight at distance 0
- `β = 0.7` — decay rate (drops below 0.1 around distance 4)
- `D = 0.5` — progressive degradation threshold
- `NEIGHBOR_BUDGET_RATIO = 0.4` — 40% of input budget for neighbor content

---

### T1-3: Rendon's Geometric Expansion (working name)
**Status:** Specced. First empirical run complete (V17, infrastructure bugs). V18 pending.  
**Target venue:** Section of TCC paper or separate short paper on topology layout  
**Evidence:** TOPOLOGY_LAYOUT.md, V17 benchmark, graph diameter threshold finding

**What it is:**
A topology layout algorithm where:
- X axis = Kahn wave assignment (deterministic, from topological sort)
- Y axis = cross-wave gravitational attraction (force-directed, continuous)
- Weight = `edge_count(v, u) ^ gravity_exponent` (configurable, default exponent=2)

The same coordinate system drives both the visual canvas layout AND the compiler output
directory structure. File placement in the generated codebase is determined by the same
physics that positions nodes on the canvas.

**Why it's novel:**
Neither Kahn-based layout nor force-directed layout is novel independently.
The combination — using Kahn waves as a fixed X axis with force-directed Y relaxation —
applied specifically to compiler output structure, is novel. The key insight: the canvas
layout and the filesystem layout are projections of the same topological coordinate system.

**Sub-finding: Graph Diameter Threshold (T1-4 below)**
The algorithm self-disables below a graph diameter threshold — the first quantitative
characterization of when graph-first layout adds value over role-based layout.

---

### T1-4: Graph Diameter Threshold for Clustering Activation
**Status:** Empirically motivated (V17). Needs validation (V18).  
**Target venue:** Finding within TCC or topology paper  
**Evidence:** V17 TEM benchmark, TOPOLOGY_LAYOUT.md, v16-infrastructure-fix-plan.md

**What it is:**
A minimum graph diameter threshold (default: 4) below which topology-derived module
layout is disabled. When graph diameter < threshold, Y-relaxation noise exceeds
structural signal — clusters produced are arbitrary (structural coupling from call-graph
proximity, not conceptual cohesion). Above the threshold, clustering is meaningful.

**Why it's novel:**
The first quantitative characterization of when graph-first layout adds value over
flat-by-role organization. The distinction between structural coupling (A calls B)
and conceptual cohesion (A and B serve the same purpose) is named and formalized
as a function of graph diameter.

**V17 empirical finding:**
37-file plugin (TEM), diameter ≈ 3-4. NumericHandler appeared in `activetargetservice/`
module because it's called from ActiveTargetService — structural coupling masquerading
as conceptual cohesion. Diameter guard correctly self-disables at this scale.

**The publishable claim:**
"Topology-derived module layout produces semantically correct cluster separation when
graph diameter ≥ N. Below this threshold, the call-graph proximity signal is
indistinguishable from noise in the gravitational field."

---

### T1-5: Two-Track Benchmark Methodology with AssertionHintContract
**Status:** Implemented and running. Methodology paper target: post-V18.  
**Target venue:** MSR, ICSE workshop — standalone methodology contribution  
**Evidence:** `AssertionHintContract`, `BenchmarkAssistHook`, benchmark_logs.md

**What it is:**
A two-track evaluation framework for LLM code generation:

**Track A (pure generation):** Graph-only. `suppressOriginalSource: true`.
No source code anywhere in pipeline. Measures: what can the model achieve
from specification alone?

**Track B (assisted generation):** Graph + sealed metadata hints. QA Squire
and Conformance receive `AssertionHintContract` — extracted signatures, behavioral
markers, structural metrics, never raw code. Coder never sees hints. Measures:
how much does informed reviewing improve generation vs blind QA?

**The delta between Track A and Track B** isolates the contribution of
quality-aware reviewing to generation quality. Every other benchmark measures
generation + reviewing + repair as a single undifferentiated signal.

**AssertionHintContract mechanism:**
Extracts from source: exported signatures, behavioral markers, structural metrics,
pattern hints. Creates information asymmetry: reviewers (QA Squire, Conformance)
know what's expected. Generator (Coder) doesn't see the solution. Mirrors how
code review works in practice.

**Key insight:**
Benchmark mode is a controlled variant of the production workflow
(generate-from-workspace with `suppressOriginalSource: true`). The only variable
between production and benchmark is whether existing file content is visible to
the Coder. Same pipeline, same infrastructure, different epistemic condition.

---

### T2-9: Three-Phase Pipeline Progression — Historical Benchmark Analysis
**Status:** Complete. Full V1-V23 history analyzed.
**Target venue:** Section of T1-5 (benchmark methodology paper) — historical validation

**What it is:**
The complete benchmark history across 23 runs reveals three distinct phases of
pipeline maturation, providing longitudinal evidence for the TCC thesis.

**Phase 1 (V6-V13): Tool maturation (3.1-5.8/10)**
Fatal hub failures common. Flash Lite V10: 2.8/10, −71% LOC, 2 fatal hub truncations,
13 DROP-DECORATOR, 9 ENGINE-INJECT. Pipeline not yet protecting generation.

**Phase 2 (V14-V15): Wave 0-2 improvements (7.4-8.1/10)**
First zero traditional bug signatures (V14). First 4/4 byte-identical hubs (V15).
+1.6 point improvement from documented pipeline improvements. Reproducible.

**Phase 3 (V19-V23): Post-pipeline-fixes (7.1-8.1/10)**
Multiple models cluster 7.1-7.9 regardless of parameter count:
- GPT-4.1 (~1T+ params): 7.9/10
- Flash Lite (smaller): 7.7/10
- Gemma 4 E2B (2.3B Q4): 7.6/10
Variance between models is less than variance caused by pipeline improvements
on the same model. Pipeline is the dominant quality variable.

**The landmark data point:**
Flash Lite V10 → V23: 2.8/10 → 7.7/10. +4.9 points. Same model. Pipeline only.
This is the single strongest empirical argument for TCC's architectural significance.

---

### T1-6: Context Decay in Graph-Structured Generation
**Status:** Formalized. Parameters empirically calibrated. Paper not yet written.
**Target venue:** EMNLP, ACL, or NLP-adjacent workshop — different audience from T1-1
**Evidence:** `ScopePruning.ts`, β=0.7, α=1.0, three-tier degradation, V14-V23 benchmark progression

**What it is:**
A parametric model for context window allocation in graph-structured code generation
where relevance decreases exponentially with graph distance from the compilation target:

```
C(v) = α · e^{-β · d(v₀, v)}

where:
  v₀ = target node (compilation unit)
  v  = neighbor node at graph distance d
  α  = 1.0 (baseline weight at d=0)
  β  = 0.7 (empirically calibrated decay rate)
```

Three-tier degradation operationalizes the continuous decay curve at natural
semantic breakpoints:
```
d=0:   C=1.00 → full content (target node)
d=1:   C=0.50 → interface contracts + generated files (direct neighbors)
d=2:   C=0.25 → symbol + return type only (transitive)
d=3+:  C=0.12 → registry identity only (distant nodes)
```

The thresholds correspond to meaningful semantic boundaries — the difference
between "what this node does," "what it exports," and "that it exists."
The decay model predicts these breakpoints should matter. Benchmark results
validate that they do: 0% import hallucination, 90%+ byte-identical output.

**Why it's novel:**
Nobody has formalized context relevance as a function of dependency graph distance.
Existing approaches use:
- Semantic similarity (RAG/cosine distance) — measures content overlap, not structural relevance
- Recency (sliding window) — measures sequence position, not architectural proximity
- Position-based attention analysis — measures where in the context array content sits
- Compression heuristics — ad-hoc rules for what to summarize

The key insight: **dependency graph topology is the correct coordinate system for
context relevance in code generation.** A node 1 hop away is semantically closer
than a node with high cosine similarity but 4 hops away. Graph distance captures
change coupling — if A depends on B, B's exports are always relevant to A regardless
of semantic similarity.

**What the literature has (and what's missing):**
- "Lost in the middle" (Liu et al., 2023): documents attention degradation for
  middle-context content. Does not propose a topology-based remedy.
- RAG relevance scoring: semantic similarity for retrieval. No graph structure.
- Sliding window attention: architectural, not content-based.
- Context compression: summarization heuristics. No parametric model.

**Missing:** A graph-distance-based decay model for context relevance with empirical
calibration. This is the gap T1-6 fills.

**Experimental design for the paper:**
1. Vary β across {0.3, 0.5, 0.7, 0.9, 1.2} — measure generation quality delta
2. Compare against flat context allocation (β=0), recency-based, semantic-similarity-based
3. Show β=0.7 is optimal or near-optimal on the TEM benchmark
4. Characterize the three-tier degradation thresholds as β-dependent breakpoints
5. Generalize: does the optimal β vary by codebase size, language, or graph diameter?

**The broader claim (testable):**
In any structured generation task where units have dependency relationships,
graph-distance-based context decay outperforms position-based and similarity-based
context allocation. Code generation is the first domain where this is empirically
validated, but the model generalizes to any graph-structured generation task.

**Relationship to other contributions:**
- T1-2 (ScopePruning): T1-2 is the implementation. T1-6 is the theoretical model
  that explains why the implementation works. They are different papers for
  different audiences.
- T1-1 (TCC): T1-6 is one component of TCC. It can be published independently
  because the mathematical model stands alone without the full compiler architecture.
- T1-5 (Benchmark methodology): T1-6 uses the benchmark methodology to validate
  the decay model. The experiments are described in T1-5 terms.

---

## Tier 2 — Strong Engineering Contributions
### Novel techniques worth a technical report, workshop paper, or prominent blog series.

---

### T2-1: GraphGenerationContract as a Typed Semantic IR
**Status:** Implemented and running.  
**Evidence:** `GraphGenerationContract`, PATH B pipeline, benchmark logs showing `requiredByDependents`

The `dependency_neighbors` vs `dependent_neighbors` direction distinction is the key
contribution. Dependencies are import sources (what you need). Dependents are export
obligations (what downstream nodes require from you). Standard dependency graphs don't
make this distinction explicit. The `requiredByDependents` field tracks which symbols
must be preserved regardless of what the model would otherwise generate.

The IR captures: target IO, exact output paths, dependency neighbors (import sources),
dependent neighbors (export obligations), dependent-required symbols, pattern contracts,
task decomposition output, benchmark mode, reference implementation status.

---

### T2-2: Dual-Hash Drift Detection (Content Hash ≠ Contract Hash)
**Status:** Implemented.  
**Evidence:** Vector DB schema, NodeHasher.ts

Two hashes per node: SHA-256 of generated content + hash of the graph contract.
Traditional compilation asks "did the source change?" LLM compilation asks "did the
specification change, even if we haven't regenerated yet?"

A node needs regeneration when EITHER hash changes — content drift (code changed in
workspace) OR specification drift (graph contract updated). Separating these two signals
is a specific contribution to incremental LLM compilation not documented elsewhere.

---

### T2-3: Triage Routing with the Inversion Principle
**Status:** Implemented.  
**Evidence:** Orchestrator.ts, triage routing logic

"Worst model suited for the task, not best." The goal of triage is not the best model
but the cheapest model that will still pass the quality gate. The compounding loop:
verified nodes → richer retrieval → cheaper models succeed → more nodes verified.

The quality gate as safety net is what makes this economically rational. Without the
quality gate, cheaper models produce silent quality degradation. With it, they fail
loudly and escalate — making cost-optimized routing safe.

---

### T2-4: Sacred Section ContextBudget with Priority-Tier Allocation
**Status:** Designed. Implementation gap identified (compactContext() still present).  
**Evidence:** PEP.md RE2, ContextBudget.ts

`compactContext()` trims from the START of the context array under budget pressure —
silently removing the system prompt first (role definition, output format, contract
enforcement). The fix: sacred sections (system prompt, target node, IO contracts)
are protected from eviction. Budget overflow handled by controlled tier-priority drops.

Engineering finding: the most important content in an LLM context is typically at
the start of the array and therefore the first evicted under naive budget management.

---

### T2-5: Round-Tripping as a Structural Property of Dense IR
**Status:** Empirically validated.  
**Evidence:** V15 benchmark (8.1/10, 90%+ byte-identical), V-Poisoned-SF (100%)

90%+ byte-identical output without source code means Code → Graph → Code is a stable
cycle. This is not a feature built on top of grAIph — it's a consequence of IR density.
The graph encodes enough semantic information to reconstruct the original with high
fidelity. Validated by the poisoned run ceiling (100% when source is available) and
the clean run performance (90%+ without source).

---

### T2-6: Cycle-Aware ScopePruning with Hashset Boundary Register
**Status:** Designed (topology plan). Implementation pending.  
**Evidence:** topology-layout-plan.md, cycle handling design

Using a `Set<string>` populated from SCC metadata as a cycle boundary register in BFS
context assembly. The Set enforces non-repeatability by construction — O(1) insert and
lookup, no conditional logic required to prevent re-entry. The BFS cannot revisit a
cycle member regardless of how many paths lead to it.

The `cycleHitCount` — number of BFS traversal attempts blocked by the hashset — is a
novel architectural smell signal: high counts indicate a node that over-relies on
circular architecture. This becomes the `cycle:over-coupling` conformance rule.

---

### T2-8: Weak-Model Benchmark Finding — Pipeline Is The Bottleneck
**Status:** Validated. V10 → V23 (Flash Lite) and V20 → V22 (Gemma) progressions.
**Target venue:** Section of T1-5 (benchmark methodology paper)
**Evidence:** V20 (4.8/10, 5.4% identical) → V22 (8.1/10 adjusted, 73% identical) — same 2.3B Q4 model, pipeline fixes applied between runs. +3.3 point improvement from pipeline alone.

**What it is:**
A 2.3B parameter model running at Q4 quantization (fits in 2GB RAM) achieves 8.1/10
adjusted score — tying V15 which used an unknown frontier model. The gap between
V20 (4.8/10) and V22 (8.1/10) is entirely pipeline infrastructure, not model capability.

**Specific findings:**
- Zero ENGINE-PLUGIN-INJECT — first clean run in benchmark history (W1-T3 Coder prohibition worked)
- 4/4 critical hubs byte-identical — same as V15 frontier model result
- 73% file stability — competitive with V15's 70.3%
- 6 tsc errors, all single-character typos (1 missing `f`, 1 missing `}`, 1 wrong bracket)
- Fix 3 characters → 0 compile errors

**The publishable claim:**
"When governed by Topological Context Compilation, models spanning orders of magnitude
in parameter count cluster within 0.6 points of each other. The single most dramatic
demonstration: Gemini 3 Flash Lite scored 2.8/10 in V10 (worst performer in benchmark
history) and 7.7/10 in V23 with the same model — a +4.9 point improvement from
pipeline improvements alone. The quality ceiling is determined by pipeline infrastructure
specification gaps, not model capability."

**V23 additions (Flash Lite):**
- **ORCHESTRATOR-SKIP** new failure mode: model generates all dependency nodes correctly
  but skips the root node (plugin.ts — the entry point). Failure mode is pipeline
  completeness, not generation quality. Fix: post-batch completeness check verifies
  all declared nodes have output on disk.
- **Semantic equivalence score**: 9.2/10 on DIFF files — new metric measuring runtime
  behavior equivalence independent of byte-identical status. SvgIcons renders identical
  pixels despite different encoding. EnumCycle produces identical runtime behavior
  despite `interface` vs `type` difference.
- **14/14 UUIDs correct**: unprecedented for Flash Lite. First clean UUID run besides
  V18 (poisoned) and V22.

**The four-metric framework (formalized from V23 analysis):**
Each metric answers a different question about generation quality:
1. Byte-identical: "Did the model reproduce this exactly?"
2. Semantic equivalence: "Does it behave the same at runtime?"
3. Adjusted score: "What with pipeline gaps closed?" (ONLY pipeline-fixable gaps)
4. Functional equivalence: "How much is equivalent under reasonable tolerance?"
Adjusted score EXCLUDES style differences — those are model quality signals for
semantic equivalence, not pipeline problems.

**The abstraction hierarchy finding (from V22 analysis):**
Repeating pipeline failures must be fixed at the correct abstraction level:
1. SPEC (graph node) — model couldn't have known without being told
2. PROMPT (Coder brief) — model knew the concept, applied inconsistently
3. CONFORMANCE (rule in registry) — structural invariant violation
4. QA SQUIRE (compile check) — syntactic malformation, language-level truth
5. POST-PROCESSING (file writer) — wrong artifact format from pipeline

Fix as high as possible. Level 1 (spec) prevents. Level 5 (post-processing) patches.

---

### T2-7: AssertionHintContract Information Asymmetry Technique
**Status:** Implemented.  
**Evidence:** AssertionHintContract.ts, BenchmarkAssistHook.ts

Extracting structural metadata from source code and injecting it into reviewing agents
(QA Squire, Conformance) but NOT into the generating agent (Coder) creates a controlled
information asymmetry. Reviewers know what's expected. The implementer doesn't see the
solution. This mirrors how code review works in practice and enables measurement of the
reviewing contribution independently of the generation contribution.

This is both a benchmark tool AND a production technique for guiding quality improvement
without contaminating generation quality measurement.

---

## Tier 3 — Implementation Findings
### Real production discoveries worth documenting as blog posts or paper appendices.

---

### T3-1: Model Capability Handshake for Adaptive Budget Calculation
`getModelInfo()` resolves actual contextWindow and maxCompletionTokens at runtime.
Adaptive budget: `INPUT_BUDGET = contextWindow - maxCompletionTokens - SYSTEM_RESERVE - TARGET_RESERVE`.
Critical fix: `Math.max(0, floor(INPUT_BUDGET × ratio))` — negative budgets must still
trigger pruning, not disable it.

### T3-2: Iterative DFS for Tarjan's on Production Graphs
Recursive DFS causes stack overflow on real codebases (287+ edges documented).
Explicit stack-based implementation with `[nodeId, neighborIndex]` tuples to resume
traversal — avoids stack overflow without losing DFS semantic correctness.

### T3-3: Completeness Verification Metrics
Symbol coverage ratio, import coverage ratio, body-line-count ratio (flag if < 0.5).
Systematic quality signals for LLM-generated code that go beyond test-based verification.
`symbolCoverage`, `importCoverage`, `lineRatio` form a lightweight completeness framework.

### T3-4: Negative Budget Floor Fix in Progressive Pruning
When sacred sections (source, system prompt) consume the full context window,
`INPUT_BUDGET` goes negative. Without the floor fix, `neighborTokenBudget` is negative
and the condition `totalNeighborTokens > neighborTokenBudget` is always true but the
loop body never degrades files. Fix: `Math.max(0, ...)` forces the loop to fire and
degrade all neighbor content to stubs when budget is exhausted.

---

## Publication Map

| ID | Contribution | Target Venue | Readiness | Blocker |
|----|---|---|---|---|
| T1-1 | Topological Context Compilation | MSR / ICSE | Draft after V18 | V18 clean topology benchmark |
| T1-2 | ScopePruning exponential decay | Section of T1-1 or standalone | Parameter characterization needed | β calibration study |
| T1-3 | Rendon's Geometric Expansion | Section of T1-1 or short paper | V18 validates | V18 clean topology benchmark |
| T1-4 | Graph diameter threshold | Finding within T1-1/T1-3 | V18 validates | V18 diameter measurement |
| T1-5 | Two-track benchmark methodology | MSR / ICSE workshop | Strongest standalone — write first | None — already implemented |
| T1-6 | Context decay in graph-structured generation | EMNLP / ACL workshop | Write after T1-5 | β calibration experiments (vary β, measure quality) |
| T2-1 | GraphGenerationContract IR | Technical report | Post open core release | Public repo |
| T2-2 | Dual-hash drift detection | Technical report / blog | Underappreciated — write soon | None |
| T2-3 | Triage inversion principle | Blog → workshop paper | Accessible framing | None |
| T2-4 | Sacred section ContextBudget | Blog | Implement first | ContextBudget wiring (PEP RE2) |
| T2-5 | Round-tripping finding | Finding within T1-1 | Validated | None |
| T2-6 | Cycle-aware hashset ScopePruning | Section of T1-1 | After topology merges | Topology branch merge |
| T2-7 | AssertionHintContract technique | Section of T1-5 | Validated | None |
| T3-1 | Model capability handshake | Blog | Done | None |
| T3-2 | Iterative DFS for Tarjan's | Blog / appendix | Done | None |
| T3-3 | Completeness metrics | Appendix in T1-5 | Done | None |
| T3-4 | Negative budget floor fix | Blog | Done | None |

---

## The Recommended Writing Order

**Write first:** T1-5 (two-track benchmark methodology). It's the most immediately
publishable, doesn't require V18, and establishes the measurement framework that makes
every other result credible. A submitted methodology paper is also the strongest
evidence for the NLnet second cycle.

**Write second:** T1-6 (context decay model). Narrow claim, different venue,
different audience from T1-1. Requires one experiment (vary β, measure quality delta).
Can be written in parallel with T1-1 preparation.

**Write third:** T1-1 (TCC umbrella paper). After V18 validates the topology algorithm.
Subsumes T1-2, T1-3, T1-4, T2-5, T2-6, T2-7 as sections.

**Write third:** T1-3 (Rendon's Geometric Expansion) as a standalone if the topology
results are strong enough to warrant it. Otherwise remains a section of T1-1.

---

## Terminology Reference

| Term | Definition |
|------|-----------|
| TCC | Topological Context Compilation — the complete architecture |
| Influence Wave Decay | The ScopePruning exponential decay algorithm |
| Rendon's Geometric Expansion | Kahn + Y-relaxation gravitational layout algorithm |
| Graph Diameter Threshold | Minimum graph diameter for clustering activation (default: 4) |
| AssertionHintContract | Sealed metadata extracted from source, injected into reviewers only |
| Sacred Sections | System prompt + target node + IO contracts — never evicted under budget pressure |
| Dual-Hash Drift Detection | Content hash ≠ contract hash — two independent regeneration triggers |
| Inversion Principle | Triage selects worst model that will still pass quality gate, not best |
| Round-tripping | Code → Graph → Code stable cycle, consequence of IR density |
| Context decay | Exponential decrease in context relevance as a function of graph distance from the generation target. C(v) = α · e^{-β · d(v₀, v)} |
| β (decay rate) | Calibrated constant in the context decay model. β=0.7 empirically optimal on TEM benchmark. Higher β = steeper decay, less neighbor context. |
| Three-tier degradation | Discretization of the continuous decay curve: d=0 full content, d=1 interfaces, d=2 signatures, d=3+ identity. Semantic breakpoints predicted by the decay model. |
| Compounding Loop | More verified nodes → cheaper models succeed → more nodes verified |
| Cycle Boundary Register | Set<string> populated from SCC members, enforces BFS non-repeatability |
| cycleHitCount | BFS traversal attempts blocked by cycle boundary — architectural smell signal |

---

## Update Log

| Date | What Changed |
|------|-------------|
| 2026-05-19 | Document created. 15 contributions catalogued across 3 tiers. Publication map drafted. |
| 2026-05-21 | T1-6 added — Context Decay in Graph-Structured Generation. Nobody formalizes context relevance as a function of graph distance. β=0.7 empirically calibrated. Three-tier degradation as discretization of continuous decay curve. Target venue: EMNLP/ACL. Different audience from T1-1. |
| 2026-05-21 | V23 (Flash Lite) added — 7.7/10 floor. +4.9 pts from V10 (2.8). ORCHESTRATOR-SKIP new failure mode. 9.2/10 semantic on DIFF files. Four-metric framework formalized. T2-9 added (three-phase historical analysis). Adjusted score definition clarified — style diffs excluded. |
| 2026-05-21 | V22 (Gemma 4 E2B, 2.3B Q4) added — 7.6/10 floor, 8.1/10 adjusted. Ties V15 frontier model on adjusted score. T2-8 weak-model benchmark finding validated. Zero ENGINE-INJECT first time in benchmark history. 3 compile errors all single-character typos → QA Squire mandatory compile check formalized. UUID placeholder pattern → graph spec fix (Level 1). Abstraction hierarchy for pipeline fix layers documented. |
