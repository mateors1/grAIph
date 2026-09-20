# grAIph — Current State

**Last reviewed:** 2026-09-20

This document is the canonical public claim boundary for grAIph. It distinguishes implementation properties, bounded evidence, experimental mechanisms, current blockers, planned work, and claims the project explicitly does not make.

When another public document is less precise, use this document as the more conservative description.

---

## Status vocabulary

| Status | Meaning |
|---|---|
| Verified implementation | The mechanism exists and has deterministic or integration coverage in the private development workspace. |
| Verified locally with limitations | The mechanism has host or live local evidence, but the evidence does not establish production qualification or general performance. |
| Implemented but experimental | The mechanism exists, but its proposed benefit has not been established by the governing experiment. |
| Planned or deferred | The design direction exists, but the capability is not active product behavior. |
| Blocked | A named gate remains unsatisfied; adjacent evidence does not unlock it. |

---

## Verified implementation properties

### Shared graph runtime and visual protocol

The development implementation has a versioned visual protocol and an authoritative graph/session runtime. Durable graph commands advance a revision and document hash through one persistence and publication path.

The same host-independent UI can attach through:

- the native VS Code webview;
- an authenticated standalone local browser;
- the Codex browser surface.

Viewer-local state—selection, hover, viewport, open panels, and draft text—does not become shared graph authority. MCP and UI mutations converge through the same graph runtime rather than maintaining independent project copies.

**Qualification:** verified locally and through bounded host evidence. The complete final-build all-origin performance matrix, steady-state memory baseline, and long soak remain open.

### Headless and embodied MCP

The implementation provides:

- a headless stdio MCP route with its own workspace context;
- an extension-hosted, authenticated loopback HTTP route bound to the live graph runtime;
- observe access by default and explicit drive authorization for mutations;
- stale graph-version rejection for mutation requests;
- bounded graph-read projection for responses that exceed the shared output budget.

The embodied route does not create a shadow graph. It operates on the same runtime that publishes to the visible UI.

### Provider-neutral observation

Provider streaming is normalized into an observation plane. Token and execution events may be projected to the UI, diagnostics, runtime-event subscribers, or MCP resources. The final `LLMResponse` remains authoritative for pipeline interpretation.

Runtime events are bounded, sanitized, ordered, and read-only. Observation cannot authorize a mutation, accept a candidate, or establish committed state.

### Typed candidate pipeline

The normal PATH B route is decomposed into explicit phases:

1. analysis and topology planning;
2. task planning;
3. generation-contract construction;
4. capability planning;
5. code synthesis and deterministic normalization;
6. QA repair;
7. conformance repair;
8. Quality Gate;
9. Grapher evidence production;
10. graph reconciliation;
11. candidate assembly.

The Orchestrator remains the public sequencing facade. Host persistence and receipt verification complete the durable boundary after candidate processing.

### Candidate versus committed state

A model response, tool result, validation pass, graph proposal, or assembled candidate is not by itself a committed generation.

The host must:

- persist the accepted files and graph mutation;
- return a canonical receipt;
- prove that committed targets cover the candidate;
- expose the resulting graph revision and file state.

Compatibility success signals are derived from that final outcome rather than from the existence of a model response.

### Topology plan and graph reconciliation

Topology Scrutineer owns an immutable plan bound to the input graph fingerprint. Planning and mutation are separate responsibilities:

- planners analyze generation order, edge additions/removals, and cycle repair;
- one reconciliation executor applies the plan to detached graph state;
- the host verifies the committed graph against the accepted candidate and receipt.

Candidate, accepted, persisted, and committed are distinct states. The topology transaction is also separate from visual layout.

### Unified tool execution and durable invocation

Supported tool routes enter a shared executor boundary for schema validation, authorization, confirmation, cancellation, timeout, hooks, bounded output, and terminal-state settlement.

Caller-owned inputs are snapshotted before asynchronous work. Untrusted transport metadata does not grant authority. Full outputs remain local artifacts while models, logs, and remote interfaces receive bounded policy-safe projections.

Invocation lifecycle can be persisted and recovered without replaying an uncertain mutation. Tool-invocation evidence does not replace a graph/file commit receipt.

### Minimum capability surfaces and candidate tools

Task planning can express semantic capability requirements without naming concrete tools. A deterministic capability planner applies hard policy before ranking and budgets, then materializes an exact effective surface for the current model turn.

The active candidate-tool sequence is bounded:

- read-only contract, candidate-symbol, repository-symbol, and diagnostic evidence;
- before-hash-guarded candidate patching and finding comparison;
- broker-issued capability expansion with exact leases, budgets, expiry, and denial alternatives.

The Coder can mutate only candidate state. Quality Gate, settings, dependency installation, graph commit, and host finalization do not receive adaptive general-purpose tools.

### Language capabilities

Language support is explicit and capability-graded. Unknown languages resolve as unsupported rather than silently inheriting TypeScript behavior.

“Best effort” means that a defined implementation path exists; it does not mean exact language-native analysis or equivalent behavior across languages.

### Benchmark provenance

Benchmark infrastructure records graph, source revision, runtime revision, provider, model, mode, flags, run identity, leak audit, candidate evidence, and commit eligibility.

Benchmark eligibility remains separate from ordinary generation success. A committed generation can still be ineligible for a graph-only benchmark.

---

## Bounded public evidence

### Host-independent visual correctness

The latest reviewed exact-build run performed 35 actual Codex-browser-origin Add Node edits on a graph growing from 500 to 535 nodes while VS Code and standalone Chrome were attached.

For that run:

- all 35 observations matched the authoritative revision and document hash;
- all 35 matched durable receipts;
- all three viewer layout markers converged;
- the native viewport remained unchanged.

This is a bounded correctness result for one origin and three viewers. It is not the complete all-origin matrix and did not exercise provider generation.

### Visual responsiveness

Receipt-to-layout measurements from the same run were:

| Viewer | Median | p95 |
|---|---:|---:|
| Native VS Code | 672 ms | 1266 ms |
| Standalone Chrome | 776 ms | 1414 ms |
| Codex browser | 820 ms | 1506 ms |

All three p95 values fail the unchanged 250 ms target. The target requires evidence-based calibration; it has not been silently replaced with a number that the run passes.

### Candidate-tool program

The minimum-tool-surface program completed under amended functional criteria. Public-safe bounded results include:

- 54/54 deterministic semantic observations passed for the read-only evidence surface;
- selected contract bytes were 77.26% lower at equal deterministic semantic accuracy;
- all 30 attempted candidate repairs showed finding progress, with zero critical findings introduced;
- expansion occurred on 3/24 representative normal tasks;
- 15/18 distinct grants were useful;
- the final expansion schedule used zero provider calls.

These results establish bounded functional behavior. They do not establish provider-quality improvement, token savings, production frequency, latency improvement, or causal superiority.

See [September 2026 progress and limitations](./docs/progress-2026-09.md).

---

## Implemented but experimentally unvalidated as improvements

### Relation surfaces and bindings

Graph relations can represent provided and required program surfaces and usage kinds such as call, construct, inject, reference, and imported-value read. Their effect on generation variance or architectural fidelity remains unvalidated by the governing provider-backed A/B.

### Structural contracts

Source-derived structural expectations can preserve class shape, public members, inheritance, and implementation requirements while source bodies are suppressed. The mechanism exists but is not promoted as a proven improvement.

### Evidence rewiring and retraction

Generated code can be inspected for actual imported-symbol use. Evidence can create or strengthen relations and retract unsupported evidence-derived contracts while preserving declared architectural intent.

### Graph-informed context allocation

Dependencies can be represented as bodies, symbol surfaces, relation information, structural contracts, or adaptive combinations. No fixed attenuation rule is treated as optimal.

### Active capability surfaces and candidate repair

The runtime mechanisms and bounded safety properties exist. Whether active minimum surfaces, candidate repair, or brokered expansion improve provider-backed generation quality remains an empirical question.

### Topology-derived layout

A deterministic topology-derived visual placement strategy exists behind explicit controls. Its existence does not establish a generation-quality or human-efficiency benefit.

---

## Current blockers

### B1 — Relation-surface provider experiment

**Status: BLOCKED**

The fresh provider-backed A/B for relation surfaces, binding contracts, and separately attributed structural contracts has not been completed and reviewed.

Tooling-program results, host synchronization evidence, historical scores, and deterministic fixtures do not satisfy this gate.

### B2 — Visual release qualification

**Status: BLOCKED**

The latest exact-build run establishes bounded convergence but not release-complete visual hosting. Open work includes:

- evidence-based responsiveness budgets;
- the final-build all-origin/viewer matrix;
- fixed-size comparable performance measurements;
- steady-state memory baselines;
- a long soak;
- a current clean release receipt.

### B3 — Production qualification

**Status: BLOCKED**

Local and bounded evidence is not a production deployment record. Public package availability, production operations, and broad external compatibility are not claimed by this repository.

---

## Planned or deliberately deferred

- Durable Record Keeper consumption of runtime events, accepted/rejected attempts, evidence, recurrence, and provenance as full process memory
- Adaptive orchestration driven by reliable state, evaluation, and memory
- An external AVO-style loop based on observe → attribute → replay → evaluate
- Confined multi-tool program execution such as `workbench_execute`
- Automatic topology revision without explicit evidence, bounded planning, and host authorization

The current system provides substrates for some of this work. A substrate is not the completed capability.

---

## Explicitly not claimed

grAIph does not currently claim:

- benchmark superiority or parity with frontier coding systems;
- production readiness or release-complete visual hosting;
- that the 250 ms visual responsiveness target is met;
- that deterministic functional evidence proves provider-quality improvement;
- that every host/origin/viewer combination has final-build performance evidence;
- that every language has identical analysis capabilities;
- that exponential attenuation is the optimal context-allocation rule;
- that Record Keeper already supplies complete process memory;
- that adaptive orchestration or AVO is active;
- that confined general-purpose workbench execution is available;
- that historical byte-identical results are valid;
- that implementation novelty establishes research novelty;
- that a model response or tool result is equivalent to committed project state.

---

## Historical integrity correction

In July 2026, an audit found that a fallback path contaminated previously reported byte-identical benchmark results with imported source content.

Those claims were retracted. See [the correction record](./archive/2026-07-17-benchmark-correction.md).

The active benchmark protocol is [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md), and the preserved v1.1 document is [archive/BENCHMARK_SPEC-v1.1.md](./archive/BENCHMARK_SPEC-v1.1.md).
