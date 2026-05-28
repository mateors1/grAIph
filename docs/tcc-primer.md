# Topological Context Compilation — A Primer

## The Problem

LLM code generation has a context window problem. When generating a large codebase,
the model needs information about dozens of files simultaneously. But context windows
are finite. Put too much in and attention dilutes. Put too little in and the model
hallucinates imports, wrong method signatures, incorrect architecture.

Current approaches: bigger context windows, RAG retrieval, summarization heuristics.

None of them ask the right question: **what is the correct information for this specific
file, at this specific moment in the generation sequence?**

---

## The Insight

A codebase is a dependency graph. Files import from other files. Some files depend
on many others. Some are depended on by everything.

In a dependency graph, **graph distance is a proxy for semantic relevance.**

A file that imports directly from `ViewManager` needs to know ViewManager's full
interface. A file three hops away needs to know ViewManager exists. A file on a
completely separate branch doesn't need to know about ViewManager at all.

This is the same insight compilers have used for sixty years — dependency-ordered
compilation passes ensure each compilation unit's working context contains exactly
what it needs. Compilers solved this for RAM on memory-constrained hardware.
grAIph solves it for LLM context windows.

---

## The Three Mechanisms

### 1. Context Decay Formula

```
C(v) = α · e^{-β · d(v₀, v)}

where:
  v₀ = target node (file being generated)
  v  = neighbor node
  d  = graph distance in hops
  α  = 1.0 (baseline weight)
  β  = 0.7 (empirically calibrated decay rate)
```

Information relevance decreases exponentially with graph distance.
Empirically calibrated at β=0.7 across a 37-file TypeScript benchmark.

**Three-tier degradation** operationalizes the continuous decay curve:

| Distance | Content provided | Rationale |
|----------|-----------------|-----------|
| d=0 | Full content | This IS the target |
| d=1 | Interface contracts + generated files | Direct imports |
| d=2 | Symbol + return type only | Transitive dependencies |
| d=3+ | Registry identity only | Distant context |

### 2. Kahn Wave Ordering

Kahn's algorithm produces a topological sort of the dependency graph.
Files with no dependencies generate first (Wave 0). Files that depend only
on Wave 0 generate next (Wave 1). And so on.

**Why this matters for LLMs:**

When a Wave 3 file generates, Wave 0-2 outputs are already in the context window
as recently generated content. The sliding window naturally contains the most
relevant information — not because of retrieval, but because generation order
IS relevance order.

The topology ordering makes the sliding window semantically aligned with the
dependency graph by construction.

### 3. SCC Contraction for Cycles

Real codebases have circular dependencies. Kahn's algorithm requires a DAG.

Tarjan's Strongly Connected Components algorithm detects cycles before Kahn's runs.
Each cycle is contracted to a virtual node. Kahn's runs on the resulting DAG.
Cycle members receive stable wave assignments and generate as co-located peers.

The pipeline never deadlocks on cycles. It handles them structurally.

---

## The Result

```
0%    import hallucination    (structural guarantee from wave ordering)
90%+  byte-identical output   (without source code — IR density validation)
100%  byte-identical          (with source — pipeline determinism ceiling)
```

A 2.3 billion parameter model governed by TCC produces output competitive
with frontier models on structured code generation tasks.

---

## The Compiler Analogy

This is not a metaphor. It is structural correspondence:

| Traditional Compiler | grAIph (TCC) |
|---------------------|--------------|
| Source language | JSON graph (editionFace + relationFace) |
| Type system | Graph relation contracts |
| Type checker | QualityGateAgent |
| Compilation pipeline | 7-agent sequential pipeline |
| Context window schedule | Kahn wave ordering |
| Linker | RecordKeeper + registry artifacts |
| Header files | Precomputed stubs from declared outputs |
| Output | Verified, compilable code in any target language |

The LLM is the assembly-line worker. The graph is the foreman.

---

## Why It's Closer To Nature Than It Should Be

TCC arrived through benchmark iteration and engineering intuition. Only in
retrospect did the underlying physics become visible.

The context decay formula is the mathematical form of **physical signal attenuation**
— the same equation governing how sound, light, and gravitational influence diminish
with distance. It was not derived from physics. It was calibrated empirically at β=0.7
and happened to match a law nature already knew.

Kahn wave ordering is **synaptic propagation** — downstream nodes cannot fire until
upstream signals arrive. Generation order enforces the same causality constraint
neurons operate under.

The topology layout algorithm uses gravitational relaxation — nodes cluster by
semantic weight, edges exert force proportional to relationship strength.

**LLM code generation obeys spatial physics.** The pipeline is designed accordingly —
not because physics was the starting point, but because the structure of the problem
and the structure of physical information propagation are the same structure.

This convergence is not coincidence. It is the consequence of applying rigorous
constraint to a stochastic system. Nature solved information routing under resource
constraints long before computers existed. TCC is one rediscovery of those solutions
in a new substrate.

---

## Why Nobody Else Is Doing This

Current LLM code generation tools treat the context window as a prompt engineering
problem — what text should I put in the prompt?

TCC treats it as a compiler paging problem — in what order should I generate, and
what information does each generation unit need?

These are the same problem. Compilers solved it for RAM. grAIph solves it for LLMs.

---

## Further Reading

- [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md) — Full benchmark methodology
- [TOPOLOGY_LAYOUT.md](../TOPOLOGY_LAYOUT.md) — Topology layout algorithm
- [ACADEMIC_CONTRIBUTIONS.md](../ACADEMIC_CONTRIBUTIONS.md) — Publication map

*Mateo Rendon Suarez — hello@graiph.dev*
