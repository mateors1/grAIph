# November 2026 Progress and Limitations

**Reviewed:** 2026-11-04

Since the September public preview, grAIph has added controlled adaptive execution, native project QA, grouped generation for cyclic dependencies, persistent settings, and richer project recreation. The deterministic generation route remains the default.

This update summarizes implementation and retained evidence from the private development workspace through November 2. Later working-tree changes are identified separately below. Raw development artifacts remain private; these summaries do not make the experiments independently reproducible from this repository.

---

## Controlled adaptation and recoverable attempt history

An explicitly enabled controller can select bounded generation, retry, evaluation, and replanning actions against current host state. Native execution requires a trusted, registered independent evaluator. A selector proposes actions; the host enforces dependency readiness, budgets, evaluation, and commit authority.

The controller persists run policy, action reservations, usage, candidate/evaluation identities, and terminal state. Missing usage retains its reservation. An interrupted action with uncertain effects requires reconciliation rather than automatic replay. Ordinary Generate does not become adaptive when the opt-in is enabled.

Attempt storage now retains rejected candidates and raw evaluation reports. A bounded repair sequence demonstrated retrieval in a fresh process, repair against the same pinned compiler and repository assertions, host commit, and accepted-to-rejected lineage. A separate reasoning interface can attach advisory diagnoses. This is a narrower implemented memory capability than the full Record Keeper process-memory role, which remains incomplete.

See [Controlled adaptation and project QA](./controlled-adaptation.md).

## Cyclic dependencies and host transactions

Batch scheduling groups strongly connected components so mutually dependent units can be generated and validated together. A complete group must be accepted before its dependent groups proceed. Stale graph state, peer failure, or unavailable required evidence prevents acceptance; uncertain writes stop dispatch for recovery.

Native fixtures exercise real filesystem, graph, and SQLite boundaries with deterministic candidate production. Group acceptance is a logical transaction, not a claim of power-loss atomicity across files, graph storage, and the index. Neither throughput improvement nor live-provider quality follows from these fixtures.

## Generation controls and early conflict review

One invocation ledger accounts for provider calls and tokens across planning, synthesis, repair, and recovery. Current default allowances are 12 calls and 500,000 accounted or reserved tokens per node. Unknown usage does not reopen the retry budget.

Timing is a separate policy. The current default, **Michelangelo**, lets generation finish or be cancelled without the ordinary generation deadline. **Bounded** timing enforces deadlines; the default node duration in that mode is 600,000 ms. Explicit duration contracts and controlled adaptive-run limits remain binding. These are admission/accounting controls, not guarantees about provider billing.

Contract preflight checks typed owner/return shapes and can request an attributed behavioral review before generation. A conflict must cite existing requirements on both sides; missing or invalid review is unverified. Citation validation establishes attribution, not the semantic truth of a model judgment. A confirmed conflict stops code repair rather than spending attempts on incompatible requirements.

## Native project QA

Project readiness has a separate workflow from historical generation acceptance. Host-registered profiles define independent checks and graph-owned files. Findings can be routed to the owning generation unit, evaluated against original requirements and project regressions, and rechecked after accepted repair.

Qualification requires integration evidence, a complete emitted-artifact inventory, and verification after reopening in a different native host instance. Source, runtime, profile, or artifact changes invalidate the verdict. Ambiguous ownership and repairs that expand across unsupported owner groups require an explicit host-approved plan. Infrastructure failures remain unverified.

MCP project-QA mutations have a separate explicit allow-list, empty by default. Neither a passing generation nor model-authored prose can register an evaluator or certify project readiness.

## Project recreation and provider integration

Imports distinguish faithful recreation from an explicit policy that diagnoses source defects and records minimally corrected behavioral requirements. Defect correction requires a working model and summarizer. Saved attribution and hashes do not prove that the diagnosis is correct; generation and QA must still validate the resulting requirements. Corrected-behavior runs must be identified separately from exact-recreation benchmarks.

Imported projects also retain recognized build/package configuration, lockfiles, passive assets, and discovered entry points. Scaffolding restores these inputs into the generation destination while application implementations remain generation targets. Existing destination files and configuration are preserved. Setup status and code QA have separate results; native toolchains and unsupported local dependencies may still require preparation.

The Codex subscription provider uses a locally authenticated SDK session, with read-only provider threads and host-controlled writes. MCP routing supports explicit headless or live-session selection and retains observe/drive distinctions. Provider entitlement, host authorization, and route availability remain configuration-dependent.

## Persistent settings: bounded installed-host evidence

The settings panel now binds generation policies and feature controls to durable settings, with save/cancel behavior and safe browser projections. Host-only permissions remain native configuration.

A November 1–2 isolated installed-extension check exercised fresh defaults, save and panel reopen, cancel, the adaptive command gate, full host exit, and cold reopen with retained opt-in/timing settings. Defaults were restored afterward. That check added no model calls and does not qualify generation performance or every host/platform combination.

---

## Completed diagnostic-repair comparisons

Two separate six-case development campaigns do not justify promoting diagnostic guidance as a default.

### Earlier campaign

Direct repair passed 6/6 cases, deterministic guidance 5/6, and reasoned guidance 3/6 scheduled cases. The reasoned arm included two rejected diagnoses and one repair with invalid syntax. All scheduled cells and diagnosis costs remained in the denominator. Its reported token total was 126,354 versus 68,925 for direct repair. A later review found an interval-contract coverage gap; the frozen counts were retained rather than rewritten.

### Later bounded Codex comparison

| Arm | Passing repairs | Calls | Reported input + output tokens |
|---|---:|---:|---:|
| Direct repair | 6/6 | 6 | 101,851 |
| Original diagnostic guidance | 6/6 | 12 | 213,928 |
| Clarified diagnostic guidance | 6/6 | 12 | 213,834 |

The later campaign retained 30 calls, no retries, 12 valid diagnoses, and 18 passing repairs. Clarified guidance used 2.099 times the direct-repair token total and produced six paired passing ties. The requested provider configuration was checked independently; returned events did not attest the served model identity. Process lifetime measurements were not provider inference-latency measurements.

These were exposed development cases, not repeated holdout studies or full project-generation benchmarks. The campaigns have distinct protocols and must not be pooled as replications. No self-evolution or automatic harness promotion followed.

A separate deterministic invalidation pilot also retained its control: eager propagation evaluated excess nodes, while observed-contract seeding incurred additional evaluator calls without improving the frozen objective.

## Work in progress after the reviewed implementation

The November 4 working tree includes source-backed graph dependency/API-port reconciliation, an explicit graph-repair flow, further contract/conformance review changes, emitted-HTML syntax checks, and edge presentation controls. These changes are still uncommitted development work in the reviewed workspace. They are not included in the installed-build evidence above or claimed as a qualified release.

## Gates that remain open

- The fresh relation-surface/binding-contract provider A/B, with separately attributed structural contracts and at least five repetitions per arm, remains pending.
- The September one-origin/three-viewer synchronization measurements remain historical evidence. No newer complete all-origin performance matrix or passing 250 ms responsiveness result was established by this review.
- Full process memory, autonomous harness evolution, and general provider-quality or cost superiority remain unproven.
- Local fixtures, an isolated installed-host check, and small development comparisons do not establish production readiness or cross-platform qualification.

The governing boundary is [Current state](../CURRENT_STATE.md). Historical September evidence remains in [its original progress note](./progress-2026-09.md); the July benchmark correction remains unchanged.
