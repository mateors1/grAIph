# grAIph

**Graph-structured infrastructure for LLM software generation.**

grAIph explores a practical question:

> What changes when software architecture is represented explicitly as a graph instead of being reconstructed implicitly inside a prompt?

The system models generation units, dependencies, interfaces, constraints, and usage relationships as graph state. It compiles that state into generation context, treats model output as an unverified candidate, evaluates the candidate with deterministic checks, and reconciles implementation evidence back against the graph before a host can record committed state.

**Current research status:** the architecture is implemented and actively evolving in a private development workspace. The current relation-surface, binding-contract, structural-contract, and evidence-reconciliation mechanisms are experimental. A fresh provider-backed controlled A/B is still pending, so grAIph is not currently making a benchmark-superiority claim.

---

## The problem

Raw model capability does not by itself answer the systems questions that appear in larger generation tasks:

- Which dependencies belong in context for this generation unit?
- Which parts of those dependencies are required?
- In what order should interconnected units be generated?
- Which interfaces are requirements rather than suggestions?
- How should plausible output be distinguished from accepted project state?
- What should happen when emitted code disagrees with the graph that requested it?
- How can a benchmark distinguish generation from source leakage or recovery behavior?

grAIph treats these as architecture, orchestration, validation, and evidence problems.

---

## The architecture

### 1. A graph as an intermediate representation

Nodes represent generation units. Edges represent dependencies or structural relationships. Node and edge surfaces can carry interfaces, contracts, usage expectations, and provenance.

The graph is not presented as a complete semantic model of a codebase. It is an explicit representation of the architecture that the generation pipeline is allowed to use and update.

### 2. Topological ordering and context compilation

Generation order is derived from graph topology. Strongly connected components can be handled structurally, then dependency waves provide a deterministic order for ordinary generation.

Context is allocated from graph relationships rather than by indiscriminately including every available file. Direct neighbors can receive richer representations; farther nodes can be reduced to surfaces, symbols, or identity. The particular allocation policy is configurable and remains a research variable.

An exponential attenuation rule is one policy under investigation. It is not claimed to be a universal law, an optimal setting, or a description of how LLMs literally behave.

### 3. A staged candidate pipeline

The normal decompressed profile uses this candidate chain:

~~~text
TopologyScrutineer
        ↓
PatternSpecialist
        ↓
TaskDecomposer
        ↓
Coder
        ↓
QASquire
        ↓
Conformance
        ↓
QualityGate
        ↓
Grapher
        ↓
host persistence and RecordKeeper
~~~

The model is one part of the pipeline. A model response is not, by itself, a successful generation.

### 4. Candidate versus committed state

grAIph separates what the pipeline produced from what the host durably accepted:

~~~text
model response
    ↓
unverified candidate
    ↓
validation and conformance
    ↓
graph reconciliation
    ↓
host persistence
    ↓
receipt and committed graph/file state
~~~

The committed status is derived only when validation passes and persistence reports committed state that covers the candidate targets. A response, validation pass, or graph proposal alone does not cross that boundary.

### 5. Declared architecture and observed evidence

The graph begins with declared architecture, but emitted code is evidence too. Experimental reconciliation can inspect actual usage—such as calls, construction, injection, references, and imported-value reads—and create or strengthen graph relations.

This is intended to make disagreement explicit. Evidence is not allowed to silently erase user-declared architectural intent, and an observed use is not treated as proof that the broader research hypothesis has been validated.

---

## Current research program

The current controlled experiment focuses on whether explicit relation surfaces, usage bindings, and structural contracts change generation behavior under source suppression.

The fresh A/B is designed to use:

- the same pinned source revision;
- a fresh graph for each arm;
- the same real provider, model, sampling settings, workspace, and run count;
- self-generated near-neighbor source;
- explicit manifests, provenance, and prompt-leak audits;
- a minimum of five repetitions per arm.

That provider-backed A/B has **not yet been completed and reviewed**. Until it is, the related features remain experimental defaults and the release decision remains blocked.

See:

- [Current state](./CURRENT_STATE.md)
- [Research hypotheses](./RESEARCH_HYPOTHESES.md)
- [Experiment protocol](./docs/experiment-protocol.md)

---

## Benchmark integrity

On July 17, 2026, the project publicly recorded a benchmark correction. An audit found that byte-identical files in the auditable run history had been written by a fallback path that flushed import-seeded source content when generation produced no file. Those files were not model generations.

The affected byte-identical claims were retracted. Earlier runs without retained generation evidence were treated as unverifiable. The correction is preserved as part of the project record:

- [Benchmark correction](./archive/2026-07-17-benchmark-correction.md)
- [Archived benchmark specification v1.1](./archive/BENCHMARK_SPEC-v1.1.md)

The active protocol requires run provenance, explicit eligibility, source-leak auditing, generation evidence, and a declared integrity arm before a result can support a conclusion. See [Benchmark specification v2](./BENCHMARK_SPEC.md).

---

## Public claim boundary

### Verified implementation properties

- Graph-structured generation pipeline
- Topology-derived generation ordering
- Explicit candidate versus committed-generation boundary
- Validation, conformance, and quality-gate stages
- Provider abstraction with explicit backend identity
- Per-language capability reporting
- Benchmark manifests and provenance machinery
- Prompt-leak auditing machinery
- Topology-derived layout implementation

### Implemented but experimental

- Unified relation surfaces
- Binding contracts
- Structural contracts
- Evidence rewiring and contract retraction
- AST-assisted usage evidence
- Alternative near-neighbor context representations
- Adaptive contract pruning

### Not currently claimed

- Benchmark superiority over frontier coding systems
- That a fixed exponential decay rule is optimal
- That the retracted byte-identical scores are evidence
- That the historical 2.8-to-7.7 comparison is a current result
- That experimental mechanisms should become defaults
- That implementation novelty alone establishes research novelty
- That a model response is equivalent to committed project state

The maintained boundary is [CURRENT_STATE.md](./CURRENT_STATE.md).

---

## Scope of this repository

The full development workspace remains private while the system is being hardened. This public repository contains:

- architecture documentation;
- research hypotheses;
- benchmark methodology;
- experimental protocols;
- topology and evidence-reconciliation descriptions;
- historical corrections;
- public project state.

It does not contain private source code, raw internal planning material, development logs, credentials, benchmark outputs, or unpublished implementation artifacts.

---

## Documentation

- [CURRENT_STATE.md](./CURRENT_STATE.md) — current public claim boundary
- [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md) — active v2 benchmark protocol
- [RESEARCH_HYPOTHESES.md](./RESEARCH_HYPOTHESES.md) — hypotheses and falsifiers
- [docs/pipeline-overview.md](./docs/pipeline-overview.md) — generation architecture
- [docs/tcc-primer.md](./docs/tcc-primer.md) — Topological Context Compilation primer
- [docs/experiment-protocol.md](./docs/experiment-protocol.md) — current controlled A/B
- [docs/evidence-reconciliation.md](./docs/evidence-reconciliation.md) — graph and implementation feedback
- [TOPOLOGY_LAYOUT.md](./TOPOLOGY_LAYOUT.md) — deterministic topology-derived layout
- [CITATION.cff](./CITATION.cff) — citation metadata

---

## Contact

**Mateo Rendon Suarez**  
Bogotá, Colombia

hello@graiph.dev  
[graiph.dev](https://graiph.dev)

---

## License

Apache 2.0 — see [LICENSE](./LICENSE).
