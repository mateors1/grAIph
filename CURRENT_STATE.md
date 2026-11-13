# grAIph — Current State

**Last reviewed:** 2026-11-13

This document is the canonical public claim boundary for grAIph. It distinguishes implementation properties, bounded evidence, experimental mechanisms, current blockers, planned work, and claims the project explicitly does not make.

When another public document is less precise, use this document as the more conservative description.

This review covers the local November 13 implementation checkpoint and separates it from active uncommitted remediation. Scoped verification passed, but broad-suite and recursive-test checks remain unresolved. Historical visual measurements retain their original scope. See [November progress and limitations](./docs/progress-2026-11.md).

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

### Controlled adaptive execution

A controller is implemented for explicitly enabled native runs with a trusted independent evaluator. It can select bounded generation, retry, evaluation, and replanning actions; host policy still owns admission and acceptance. Run storage retains action reservations, usage, candidate/evaluation identity, and interruption state. Unknown usage retains its reservation, and uncertain effects require reconciliation.

The opt-in defaults off. Ordinary Generate remains deterministic even when controlled commands are available. Neither autonomous harness evolution nor a general quality/cost benefit is established. See [Controlled adaptation](./docs/controlled-adaptation.md).

### Recoverable attempt history

Rejected candidates, raw evaluation reports, and accepted-to-rejected lineage can be persisted and retrieved in a fresh process. Repairs require a new candidate identity and fresh evaluation against the pinned obligations. Advisory diagnoses do not replace raw findings or host authority.

This is a bounded memory implementation, not the full Record Keeper process-memory role. Local repair fixtures do not establish general model repair quality.

### Grouped generation and invocation controls

Batch scheduling groups cyclic dependencies for joint validation and host acceptance. Dependent groups wait for complete dependency-group acceptance. Failures, stale state, and uncertain writes retain distinct terminal/recovery outcomes. Logical group transactions do not establish crash-atomicity across files, graph storage, and SQLite.

One invocation ledger accounts for calls and tokens across planning, coding, repair, and recovery. Timing is separate: Michelangelo is the current default without an ordinary generation deadline; bounded timing and explicit duration contracts enforce deadlines. Controlled adaptive runs retain their own limits. Optional graph freeze preserves captured declarations during generation; it is not a manual editing lock.

Deterministic structural/API checks apply at generation admission. Behavioral graph review is a separate opt-in; a claimed conflict can receive a bounded grounded challenge. Model review must cite existing requirements and can return unverified; attributed review is not a semantic proof.

Final native/headless group compiler errors can return to retained member candidates through bounded repair without resetting invocation accounting. Repaired candidates revisit Conformance and the complete group must compile before host acceptance. Missing environment evidence, unowned diagnostics, and non-compiler gates remain rejections.

### Saved-graph validation and dependency environments

Source-bound import/API evidence supports deterministic dependency reconciliation and declaration, member, call-shape, and port checks for supported JavaScript/TypeScript paths. An explicit bounded graph-repair flow revalidates detached changes before host commit and invalidates affected generation state. Saved receipts support later admission without requiring another import; source-suppression rules still apply to synthesis.

Portable dependency metadata is separate from machine-local readiness receipts. The Node path can reconstruct an isolated validation environment from captured manifests/locks even with project-file scaffolding off. Versions, declarations, consumers, and runtime constraints are verified; incomplete or stale environments remain unavailable/partial. Local/workspace dependencies and non-Node/native toolchains have incomplete coverage.

These are committed development mechanisms with scoped checks, not release qualification. See [Saved-graph regeneration](./docs/saved-graph-regeneration.md).

### Native project QA

A host-registered profile defines independent readiness checks and graph-owned paths. The workflow can assess findings, route unambiguous owner repairs through the controlled native path, validate detached candidates, and reassess the project. Qualified status additionally requires integration, a complete tested-artifact inventory, and verification in a different reopened native host instance. Changed source, profile, runtime, or artifacts invalidate the projected verdict.

Ambiguous ownership and unsupported multi-owner/SCC expansion require a host-approved plan. Infrastructure failures remain unverified. MCP QA mutations use a separate explicit allow-list, empty by default; graph-edit access cannot register an evaluator.

### Project recreation and persistent settings

Imports support faithful recreation and an explicit source-defect-correction policy. Correction requires a working model/summarizer and remains a diagnosis to validate, not proof of correct behavior. Corrected requirements are identified separately from exact-recreation benchmarks.

Scaffold snapshots retain recognized configuration, lockfiles, passive assets, and discovered entry points while application implementations remain generation targets. Project-file setup and independent QA-environment preparation have separate outcomes. Destination files are preserved; native toolchain and local dependency preparation remain conditional.

Supported static emitted-HTML checks parse inline JavaScript without executing the application. Audited default paths use project evidence for framework/SDK choices. Relationship wire colors/styles and collapsed-edge classification are viewer presentation controls; they do not change graph authority.

Settings persist generation policy and feature controls through the trusted host, with safe browser projections. Installed-host evidence covers settings and command gates on one isolated build/profile, not general generation qualification.

### Language capabilities

Language support is explicit and capability-graded. Unknown languages resolve as unsupported rather than silently inheriting TypeScript behavior.

“Best effort” means that a defined implementation path exists; it does not mean exact language-native analysis or equivalent behavior across languages.

### Benchmark provenance

Benchmark infrastructure records graph, source revision, runtime revision, provider, model, mode, flags, run identity, leak audit, candidate evidence, and commit eligibility.

Benchmark eligibility remains separate from ordinary generation success. A committed generation can still be ineligible for a graph-only benchmark.

---

## Bounded public evidence

### Host-independent visual correctness

The retained September 2026 exact-build run performed 35 actual Codex-browser-origin Add Node edits on a graph growing from 500 to 535 nodes while VS Code and standalone Chrome were attached.

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

### Diagnostic-repair comparisons

In a later six-case Codex development comparison, direct repair, original guidance, and clarified guidance each passed 6/6 cases. The campaign recorded 30 calls with no retries, 12 valid diagnoses, and 18 passing repairs. Clarified guidance used 213,834 reported input/output tokens versus 101,851 for direct repair: 2.099 times as many, with six paired passing ties and no wins.

An earlier separate campaign produced 6/6 direct, 5/6 deterministic-guidance, and 3/6 scheduled reasoned-guidance passes. Unavailable diagnoses/repairs and all diagnosis costs remained in the denominator. Neither campaign supports default promotion. They are exposed development samples under different protocols, not pooled replications, a holdout study, or the relation-surface A/B.

### Installed settings and regression scope

A November 1–2 isolated installed-extension check exercised save/cancel, native adaptive command gates, full host exit, and retained settings after cold reopen. It added no model calls. A separate retained regression summary reports 1,022 passing tests across 70 files, using the latest complete passing result for each file and preserving earlier failures. This was selected regression coverage, not a new full-suite or cross-platform release receipt.

### November 13 checkpoint

The retained checkpoint records passing builds, production typechecks, and selected regressions, alongside failing broad-suite and recursive-test checks. Only a subset of failures was reproduced on an earlier baseline; unresolved failures are not all classified as pre-existing. Active remediation has not established a clean release result.

A saved-graph compiler-feedback replay verified no commit with repair disabled and one revision after bounded repair and revalidation. Its repair response and model Conformance were scripted. A later prompt-delivery check is retained as uncommitted follow-up evidence. Neither establishes fresh-model repair quality or installed-host readiness.

See [November progress and limitations](./docs/progress-2026-11.md).

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

### Controlled selection and memory-guided repair

The controller, attempt lineage, and advisory diagnosis interfaces are implemented. General improvement in quality or total work over deterministic generation remains unproven. The small development comparisons above do not justify automatic promotion or default changes.

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

The retained September exact-build run establishes bounded convergence but not release-complete visual hosting. Open work includes:

- evidence-based responsiveness budgets;
- the final-build all-origin/viewer matrix;
- fixed-size comparable performance measurements;
- steady-state memory baselines;
- a long soak;
- a current clean release receipt.

### B3 — Broad validation and production qualification

**Status: BLOCKED**

The November 13 checkpoint recorded broad-suite failures and recursive-test typechecking errors. Active follow-up has cleared some scoped checks, but the broader rerun and skip review remain incomplete. Passing scoped tests and local commits do not close this gate.

Local and bounded evidence is not a production deployment record. The isolated installed-settings check does not close the all-origin visual matrix, fresh-model compiler-repair, soak, or cross-platform gates. Public registry availability, production operations, and broad external compatibility are not established by this repository.

---

## Planned or deliberately deferred

- Full Record Keeper process memory beyond recoverable attempt/evaluation lineage
- Autonomous harness evolution and automatic promotion of policies or diagnostic guidance
- Broad evidence-informed strategy selection with a demonstrated advantage over deterministic control
- Confined multi-tool program execution such as `workbench_execute`
- Automatic topology revision without explicit evidence, bounded planning, and host authorization

The bounded controller and attempt memory are implemented subsets. Their existence does not establish the broader capabilities above.

## Work in progress

The graph validation, dependency preparation, compiler-feedback, emitted-script QA, and edge presentation work previously listed here is now committed locally. Follow-up changes to validation runners, settings/protocol bindings, platform-sensitive fixtures, tests, and compiler-feedback verification remain uncommitted under active remediation. They are outside the retained checkpoint and installed-build evidence; no broad all-clear or release promotion is implied.

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
- that adaptation is the ordinary generation default or autonomous harness evolution is active;
- that confined general-purpose workbench execution is available;
- that historical byte-identical results are valid;
- that implementation novelty establishes research novelty;
- that a model response or tool result is equivalent to committed project state.

---

## Historical integrity correction

In July 2026, an audit found that a fallback path contaminated previously reported byte-identical benchmark results with imported source content.

Those claims were retracted. See [the correction record](./archive/2026-07-17-benchmark-correction.md).

The active benchmark protocol is [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md), and the preserved v1.1 document is [archive/BENCHMARK_SPEC-v1.1.md](./archive/BENCHMARK_SPEC-v1.1.md).
