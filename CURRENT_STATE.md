# grAIph — Current State

**Last reviewed:** 2026-08-13

This document is the public claim boundary for grAIph. It distinguishes implementation properties from experimental mechanisms, open empirical questions, and claims the project explicitly does not make.

When another public document is less precise, use this document as the more conservative description.

---

## Verified implementation properties

### Generation lifecycle

The generation architecture is staged. A generation first produces an **unverified candidate**.

A model response, parsing step, validation pass, or graph proposal is not by itself a committed generation. The host must persist the candidate, provide a canonical receipt, and verify that the committed graph/file state covers the candidate targets.

### Default pipeline

The normal decompressed candidate chain is:

1. TopologyScrutineer
2. PatternSpecialist
3. TaskDecomposer
4. Coder
5. QASquire
6. Conformance
7. QualityGate
8. Grapher

Host persistence and RecordKeeper complete the durable boundary afterward. Alternate profiles are explicit modes; they are not assumed to run every stage in every path.

### Modes and provider paths

The normal node-generation mode is the decompressed sequential profile. A lightweight whisper re-generation path and compressed profiles are explicit alternatives.

Native provider orchestration is conditional. It requires an explicit opt-in, provider support, and a provider orchestration implementation.

The runtime distinguishes mock, API, CLI/SDK, and VS Code Language Model backends. Similar model labels do not make those backends interchangeable.

### Language capabilities

Language support is explicit and provider-backed. Unknown languages resolve as unsupported instead of silently inheriting TypeScript behavior.

Capability is graded per language and per operation:

| Language | File naming | Context reduction | Pattern resolution | Usage evidence | Compiler QA | Repair integration |
|---|---|---|---|---|---|---|
| TypeScript | exact | best-effort | exact | best-effort | best-effort | unsupported |
| JavaScript | exact | best-effort | unsupported | best-effort | best-effort | unsupported |
| Python | exact | best-effort | best-effort | best-effort | best-effort | unsupported |
| C# | exact | best-effort | best-effort | unsupported | best-effort | unsupported |
| Go | exact | best-effort | unsupported | unsupported | best-effort | unsupported |
| Java | exact | best-effort | exact | unsupported | best-effort | unsupported |

“Best-effort” means the operation has a defined implementation path but is not being presented as equivalent to exact language-native analysis.

### Benchmark provenance

Benchmark infrastructure can record immutable provenance for the graph, source revision, runtime revision, provider, model, mode, flags, and run time.

Benchmark eligibility is separate from ordinary generation success. A committed generation can still be ineligible for a graph-only benchmark.

### Topology-derived layout

A topology-derived layout implementation exists behind explicit feature controls. It deterministically derives a layout from graph waves and cross-wave relationships. Its existence does not establish that it improves generation quality.

---

## Implemented but experimental

These mechanisms exist in the development implementation but have not been validated as improvements by the current clean provider-backed A/B.

### Unified relation surfaces

Provided and required program surfaces can be represented through a common structural model. This supports more precise descriptions than a generic node-to-node edge.

### Binding contracts

Contracts can describe how a consumer uses a neighboring dependency, including:

- call;
- construct;
- inject;
- reference;
- imported-value read.

### Structural contracts

Source-derived structural expectations can be represented separately from raw source bodies. A structural contract can preserve class shape, public members, and inheritance or implementation expectations while source bodies are suppressed.

### Evidence rewiring

Generated code can be scanned for observed use of another node's provided symbols. Those observations can create or strengthen evidence-backed graph relations.

### Contract retraction and provenance

Evidence that is no longer present can be withdrawn from evidence-derived contracts. Declared contracts remain architectural requests; if emitted code does not realize one, the gap is reported rather than silently deleted.

### AST-assisted usage evidence

Where an appropriate language strategy exists, usage evidence can be derived from syntax-aware analysis. Otherwise the implementation falls back to conservative scanning. The presence of an AST flag is not proof that an AST result was used.

### Context representation and pruning

Direct dependencies can be represented as bodies, symbol surfaces, relation information, or adaptive combinations. When contract context exceeds a budget, structured pruning can reduce it in stages instead of arbitrary tail truncation.

---

## Empirical validation pending

### H1 — Relation surfaces and explicit bindings

Do explicit relation surfaces and usage-kind bindings reduce run-to-run variance or architectural drift under otherwise equivalent generation conditions?

### H2 — Structural contracts

Do explicit structural surfaces improve architectural fidelity when source bodies are suppressed?

### H3 — Context allocation

Which graph-informed context representation performs best under controlled token budgets? A fixed exponential policy is one candidate, not a settled result.

### H4 — Evidence reconciliation

Does reconciling observed implementation evidence back into the graph improve subsequent generation quality or graph accuracy?

### H5 — Topology-derived layout

When does topology-derived placement outperform simpler deterministic layout policies, and does layout affect generation outcomes independently of presentation?

---

## Release gate

**Status: BLOCKED**

A fresh provider-backed controlled A/B for the relation-surface and binding-contract experiment has not yet been completed and reviewed.

Historical runs may document failure modes and motivate hypotheses, but they are not evidence for a current release decision unless they satisfy the active protocol.

No experimental default should be promoted and no benchmark-superiority statement should be published on the basis of historical headline scores.

---

## Explicitly not claimed

grAIph does not currently claim:

- parity with frontier coding systems;
- that previous byte-identical results were valid;
- that the historical 2.8-to-7.7 score change is a current result;
- that exponential attenuation is the optimal context-allocation rule;
- that every language has identical analysis capabilities;
- that every experimental feature should become a default;
- that implementation novelty establishes research novelty;
- that a model response is equivalent to committed project state.

---

## Historical integrity correction

In July 2026, an audit found that a fallback path contaminated previously reported byte-identical benchmark results with imported source content.

Those claims were retracted. See [the correction record](./archive/2026-07-17-benchmark-correction.md).

The active benchmark protocol is [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md), and the preserved v1.1 document is [archive/BENCHMARK_SPEC-v1.1.md](./archive/BENCHMARK_SPEC-v1.1.md).
