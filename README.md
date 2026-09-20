# grAIph

**A graph-first visual software environment for bounded agentic development.**

grAIph explores a practical systems question:

> What changes when software architecture is explicit, shared state instead of context that every model, tool, and interface must reconstruct independently?

The development implementation represents generation units, dependencies, interfaces, constraints, evidence, and revision history as a graph. That graph drives visualization, context compilation, generation order, scoped tool access, validation, and reconciliation. Model output is treated as an unverified candidate; only an authoritative host can commit graph and file state.

The same project representation can be attached to by the native VS Code surface, an authenticated local browser surface, and MCP-aware operators. Local viewer state remains local, while committed graph changes converge through one revisioned runtime.

**Current status:** the architecture is implemented and actively evolving in a private development workspace. Host-independent visualization, embodied MCP, provider-neutral observation, topology reconciliation, and bounded candidate tooling have local or bounded functional evidence. Release qualification remains incomplete, the current visual responsiveness target is not met, and the provider-backed relation-surface A/B is still pending. grAIph is not making a benchmark-superiority claim.

---

## Why graph-first?

Raw model capability does not answer the systems questions that appear in larger generation tasks:

- Which dependencies belong in context for this generation unit?
- Which parts of those dependencies are requirements rather than suggestions?
- In what order should interconnected units be generated?
- Which tools should be visible for this task and phase?
- How should plausible output be distinguished from accepted project state?
- What happens when emitted code disagrees with the graph that requested it?
- How can several interfaces operate on one project without creating shadow state?
- How can an experiment distinguish generation from source leakage or recovery behavior?

grAIph treats these as architecture, orchestration, authority, validation, and evidence problems.

---

## The system in one view

~~~text
                    shared revisioned graph runtime
                   /              |                \
          VS Code surface   browser/Codex UI      MCP operators
                   \              |                /
                    graph-scoped execution request
                               ↓
             topology + contracts + capability plan
                               ↓
            synthesis → QA/repair → conformance → gate
                               ↓
                 unverified candidate + evidence
                               ↓
             graph reconciliation + host finalization
                               ↓
                committed graph/file state + receipt
~~~

The graph is both a visual architecture and an intermediate representation. Interfaces may observe or propose changes, but they do not acquire commit authority merely by being connected.

---

## Implemented architecture

### Shared visual runtime

One host-independent UI can attach through VS Code or an authenticated local browser, including the Codex browser surface. A versioned visual protocol separates durable graph commands from viewer-local state such as selection, hover, viewport, and open panels.

MCP, VS Code, and browser-originated graph edits use the same authoritative runtime, persistence, revision, receipt, and replay path. See [Host-independent visual runtime](./docs/visual-runtime.md).

### Topological Context Compilation

Nodes represent generation units. Edges represent dependencies or structural relationships. Node and edge surfaces can carry interfaces, contracts, usage expectations, and provenance.

Generation order is derived from topology. Context is compiled from graph relationships rather than assembled as an undifferentiated prompt. Direct neighbors may receive richer representations while distant nodes receive surfaces, symbols, identity, or no context. The policy is configurable and remains a research variable.

See [Topological Context Compilation](./docs/tcc-primer.md).

### Candidate-first pipeline

The default PATH B route is implemented as typed phases:

1. analysis and topology planning;
2. task planning;
3. generation-contract construction;
4. capability planning;
5. code synthesis and deterministic normalization;
6. QA and bounded repair;
7. conformance and bounded repair;
8. independent quality gating;
9. graph-facing evidence production;
10. graph reconciliation;
11. candidate assembly;
12. host persistence, receipt verification, and committed state.

The Orchestrator remains the public sequencing facade. A model response, tool result, validation pass, or graph proposal is not by itself a committed generation. See [Pipeline overview](./docs/pipeline-overview.md).

### Bounded candidate tools

Tool execution uses a common policy boundary across supported routes. Semantic capability requirements are resolved against a catalog and materialized as exact, phase-bound leases. Read-only evidence tools arrive before candidate-only mutation tools; capability expansion is broker-controlled and cannot grant itself host authority.

Quality Gate, graph commit, settings, package installation, and host finalization remain outside the candidate lease system. Confined general-purpose workbench execution is not part of the current program. See [Bounded tool runtime](./docs/tool-runtime.md).

### Observation is not authority

Provider streaming and runtime events are observation planes. They support token progress, bounded replay, MCP subscriptions, diagnostics, and future process-memory consumers. The final provider response remains authoritative for pipeline interpretation, and committed state still requires host persistence and a receipt.

See [Runtime observability](./docs/runtime-observability.md).

### Topology plan and reconciliation

Topology Scrutineer produces an immutable, graph-fingerprint-bound plan. A single reconciliation executor applies candidate add/remove mutations to detached state. The host verifies that persisted graph and file state cover the accepted candidate before the result becomes committed.

This graph transaction is separate from visual node placement. See [Evidence reconciliation](./docs/evidence-reconciliation.md) and [Topology-derived layout](./TOPOLOGY_LAYOUT.md).

---

## Current evidence boundary

The latest reviewed visual-host run performed 35 actual Codex-browser-origin Add Node edits on a graph growing from 500 to 535 nodes, with native VS Code and standalone Chrome attached concurrently. Every observation matched the durable revision, document hash, receipt, and all three viewer layouts.

That run does **not** establish release readiness:

- it covered one edit origin and three viewers, not the complete all-origin matrix;
- it did not exercise live provider generation;
- p95 receipt-to-layout observations were 1266 ms in VS Code, 1414 ms in Chrome, and 1506 ms in the Codex browser;
- all three exceeded the unchanged 250 ms target.

The completed minimum-tool-surface program also has bounded functional evidence, including deterministic contract reduction, candidate-repair progress, and uncommon brokered expansion. Those results are not a provider-quality or production-frequency claim.

See [September 2026 progress and limitations](./docs/progress-2026-09.md) and the maintained [current claim boundary](./CURRENT_STATE.md).

---

## Active research program

The current controlled generation experiment asks whether explicit relation surfaces, usage bindings, and structural contracts change behavior under source suppression.

Eligible arms require:

- the same pinned source revision;
- a fresh graph for each arm;
- the same real provider, model, sampling settings, workspace, and run count;
- explicit manifests, provider identity, receipts, and prompt-leak audits;
- at least five repetitions per arm.

That provider-backed A/B has **not** been completed and reviewed. Visual-runtime and tooling evidence do not substitute for it. Until an eligible decision record exists, related mechanisms remain experimental and no benchmark-superiority conclusion is published.

See [Benchmark specification v2](./BENCHMARK_SPEC.md), [Research hypotheses](./RESEARCH_HYPOTHESES.md), and [Experiment protocol](./docs/experiment-protocol.md).

---

## Public claim boundary

### Verified implementation properties

- Graph-structured generation and topology-derived ordering
- Typed candidate pipeline and explicit candidate/committed boundary
- Host-independent visual protocol and shared revisioned runtime
- Headless and authenticated embodied MCP paths
- Provider-neutral streaming with an authoritative final response
- Immutable topology plans and transactional graph reconciliation
- Unified tool-execution policy and durable invocation lifecycle
- Semantic capability planning, exact leases, candidate-only repair, and brokered expansion
- Benchmark manifests, provenance controls, and prompt-leak auditing machinery
- Deterministic topology-derived layout implementation

Some properties are verified locally or through bounded functional evidence rather than production deployment. The exact qualification for each claim is in [CURRENT_STATE.md](./CURRENT_STATE.md).

### Implemented but experimentally unvalidated as improvements

- Unified relation surfaces and usage bindings
- Structural contracts under source suppression
- Evidence rewiring and contract retraction
- Alternative graph-informed context representations
- Active minimum-capability surfaces as a provider-quality intervention
- Candidate repair and capability expansion as quality improvements
- Topology-derived visual placement as a generation intervention

### Planned or deliberately deferred

- Durable Record Keeper consumption as full process memory
- Adaptive orchestration driven by accumulated evidence
- External AVO-style observe → attribute → replay → evaluate loops
- Confined multi-tool program execution
- Automatic topology revision without explicit evidence and host authority

### Not currently claimed

- Benchmark superiority or parity with frontier coding systems
- Production readiness or release-complete visual hosting
- That the 250 ms visual responsiveness target is met
- That deterministic functional results prove provider-quality improvement
- That a fixed exponential context rule is optimal
- That historical retracted byte-identical scores are evidence
- That a model response or tool result is equivalent to committed project state

---

## Benchmark integrity

On July 17, 2026, the project publicly recorded a benchmark correction. An audit found that byte-identical files in the auditable run history had been written by a fallback path that flushed import-seeded source content when generation produced no file. Those files were not model generations.

The affected claims were retracted. Earlier runs without retained generation evidence were treated as unverifiable. The correction remains part of the project record:

- [Benchmark correction](./archive/2026-07-17-benchmark-correction.md)
- [Archived benchmark specification v1.1](./archive/BENCHMARK_SPEC-v1.1.md)

The active protocol requires run provenance, explicit eligibility, source-leak auditing, generation evidence, and a declared integrity arm before a result can support a conclusion.

---

## Documentation

- [CURRENT_STATE.md](./CURRENT_STATE.md) — canonical public claim boundary
- [docs/visual-runtime.md](./docs/visual-runtime.md) — shared runtime and host-independent UI
- [docs/pipeline-overview.md](./docs/pipeline-overview.md) — candidate pipeline and authority boundaries
- [docs/tool-runtime.md](./docs/tool-runtime.md) — capability planning, leases, and candidate tools
- [docs/runtime-observability.md](./docs/runtime-observability.md) — streaming and bounded runtime events
- [docs/evidence-reconciliation.md](./docs/evidence-reconciliation.md) — declared architecture and implementation evidence
- [docs/progress-2026-09.md](./docs/progress-2026-09.md) — bounded evidence and limitations
- [docs/tcc-primer.md](./docs/tcc-primer.md) — Topological Context Compilation primer
- [TOPOLOGY_LAYOUT.md](./TOPOLOGY_LAYOUT.md) — deterministic visual placement
- [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md) — active benchmark protocol
- [RESEARCH_HYPOTHESES.md](./RESEARCH_HYPOTHESES.md) — hypotheses and falsifiers
- [CITATION.cff](./CITATION.cff) — citation metadata

---

## Scope of this repository

The full development workspace remains private while the system is hardened. This public repository contains architecture documentation, research hypotheses, benchmark methodology, bounded public evidence summaries, historical corrections, and the current public claim boundary.

It does not contain private source code, raw internal planning material, development logs, credentials, provider configuration, private benchmark outputs, or unpublished implementation artifacts.

---

## Contact

**Mateo Rendon Suarez**

Bogotá, Colombia

hello@graiph.dev

[graiph.dev](https://graiph.dev)

---

## License

Apache 2.0 — see [LICENSE](./LICENSE).
