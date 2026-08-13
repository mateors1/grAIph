# grAIph Pipeline Overview

This overview describes the public architecture boundary. The implementation remains private; names and stage descriptions are included to explain the design, not to expose source code.

---

## From graph to committed state

~~~text
graph and edition face
        ↓
topology and mode selection
        ↓
context and contract compilation
        ↓
candidate pipeline
        ↓
validation and conformance
        ↓
evidence reconciliation
        ↓
host persistence and receipt
        ↓
committed graph/file state
~~~

The pipeline is deliberately longer than a single prompt-to-completion call because generation, validation, evidence, and persistence answer different questions.

---

## Default candidate chain

The normal decompressed sequential profile is:

| Stage | Responsibility |
|---|---|
| TopologyScrutineer | Check graph coherence, cycles, and generation order |
| PatternSpecialist | Resolve declared architectural patterns and their contracts |
| TaskDecomposer | Turn the node request into bounded generation tasks |
| Coder | Produce a candidate implementation |
| QASquire | Run language-appropriate syntax, compile, or quality checks |
| Conformance | Check the candidate against graph and structural contracts |
| QualityGate | Decide whether blocking issues remain and whether repair is allowed |
| Grapher | Update graph-facing representations and relation evidence |

Host persistence and RecordKeeper follow candidate processing. The model response is not treated as durable success before this boundary.

Alternate profiles exist, including lightweight whisper regeneration, compressed opt-in paths, and conditional native orchestration. They are explicit modes rather than assumptions about every run.

---

## Context compilation

The context compiler combines multiple kinds of information:

- target description and edition-face metadata;
- graph neighbors and dependency direction;
- provided and required relation surfaces;
- binding and structural contracts when enabled;
- generated files and safe symbol surfaces;
- language capability information;
- token and model-budget constraints;
- source-suppression and benchmark controls.

Direct dependencies can receive richer context than distant nodes. Context can be reduced to bodies, surfaces, symbols, relation/edge information, or identity depending on the selected policy.

A distance-aware attenuation rule is one available policy. It is treated as a configurable hypothesis, not as an empirically settled law.

---

## Candidate validation

The candidate passes through checks appropriate to the selected language and profile:

- structural and syntax checks;
- compiler or best-effort language QA;
- graph and contract conformance;
- quality-gate policy;
- emission/path integrity checks.

The language boundary is explicit. Unknown languages do not silently inherit TypeScript behavior, and capability is reported per operation.

---

## Evidence and graph reconciliation

After candidate emission, the pipeline can inspect actual use of neighboring symbols. Evidence may describe calls, construction, injection, references, or imported values consumed as data.

Evidence-derived relations are kept distinct from declared architecture. Import-derived contracts are protected from evidence passes that would overwrite them. Unsupported evidence can be retracted, while a declared contract remains as an architectural request and can be reported as unfulfilled.

See [evidence-reconciliation.md](./evidence-reconciliation.md).

---

## Candidate versus committed outcome

A candidate has:

- a candidate identifier;
- generated files;
- proposed graph mutations;
- warnings and errors;
- benchmark eligibility information.

An outcome becomes committed only when validation passes and host persistence reports committed targets covered by a receipt. Compatibility success signals are derived from that final outcome rather than from the existence of a model response.

This distinction is also the basis of the benchmark eligibility gate.

---

## Provider and host boundary

The runtime distinguishes provider identities and host responsibilities. API, CLI/SDK, mock, and VS Code Language Model paths may have different capabilities and must be recorded as such.

The host is responsible for durable persistence, receipts, and committed graph/file verification. The core pipeline may return a validated candidate without claiming that the host committed it.

---

## What this overview does not establish

This architecture overview does not establish that:

- graph structure always improves generation;
- the default pipeline outperforms a raw model;
- any provider is interchangeable with another;
- every language has the same analysis quality;
- experimental contracts should be enabled by default.

Those are empirical questions tracked in [CURRENT_STATE.md](../CURRENT_STATE.md), [RESEARCH_HYPOTHESES.md](../RESEARCH_HYPOTHESES.md), and [docs/experiment-protocol.md](./experiment-protocol.md).
