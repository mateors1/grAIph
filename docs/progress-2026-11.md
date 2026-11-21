# November 2026 Progress and Limitations

**Reviewed:** 2026-11-21

Since the September public preview, grAIph has added controlled adaptive execution, native project QA, grouped generation for cyclic dependencies, persistent settings, and richer project recreation. Deterministic stage sequencing remains the default; model output can still vary. The shared tool executor, candidate leases, capability expansion, and adaptive controller are implemented opt-ins that default off.

This update distinguishes the early committed November 13 checkpoint from later uncommitted remediation and live-repair source cuts, documented in the November 15 review. The earlier verification receipt is dated November 14 UTC (November 13 in Bogotá); the November 15 runs and harness increment are recorded separately below. Historical failures and scope limits remain part of the record. Raw artifacts remain private; these summaries do not make the experiments independently reproducible from this repository.

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

## November 13: saved-graph regeneration checkpoint

The previously pending graph and presentation work is now committed locally. The checkpoint adds source-bound dependency/API validation and bounded graph repair, portable dependency contracts with independent Node QA environments, final-group compiler feedback into repair, static emitted-script syntax checks, and relationship wire colors/styles. Model-based behavioral graph review is optional; deterministic admission remains separate.

Validation environments can be prepared from saved metadata with project-file scaffolding off. Missing or stale environments remain unavailable evidence; they are not repaired by guessing changes to application code. Local/workspace dependencies, non-Node environments, and native integration still need additional coverage.

Final native/headless group compilation now routes owned compiler findings to retained candidates within their original budgets. A saved-graph replay rejected with zero commits when repair was disabled and committed one revision after repair, Conformance, and whole-group compilation when enabled. Initial generation, the repair response, and model Conformance were scripted; compiler, prompt construction, source admission, parsing, environment, and transaction code executed with a test filesystem adapter. This establishes routing, not fresh-model repair success.

A later uncommitted follow-up also checked that the complete diagnostics and rejected code reached the repair-provider request. It remains separate from the committed checkpoint.

A saved-graph application campaign completed its declared file set over multiple native batches. Rejected compiler samples, retries, usage, and remaining differential gaps were retained. It was a corrected-source/non-clean arm with unmeasured native packaging/hardware behavior, not an eligible clean leaderboard result or an isolated model comparison.

Builds, production typechecks, and selected regressions passed at the early checkpoint; broad-suite and recursive checks failed. Only a subset was then reproduced on an earlier baseline, so the failures were not all labeled pre-existing. The later results below supersede that test status without rewriting its artifacts. Release qualification remains open.

See [Saved-graph validation and regeneration](./saved-graph-regeneration.md) for the mechanisms and scoped evidence.

## Later November 13 verification and live repair

Retained follow-up receipts now show passing macOS build, production typechecks, recursive package checks, and a root run of 5,356 passing tests, zero failures, and 32 classified skips. The earlier failed run remains retained. Windows-only requirements, immutable historical replays, live-provider tests, and opt-in diagnostics explain the skips; four diagnostics passed separately. See the [evidence guide](./evidence-guide.md) for the breakdown and overlap limits.

A native application campaign recorded a natural live-model compiler repair accepted after real Conformance and group compilation. A separate controlled live canary repaired known faults and committed once; an earlier live candidate failed Conformance and a local readiness rejection made no remote call. These are separate scopes from the earlier scripted replay.

The 53-output-unit campaign required three native batches and two engine fixes/rebuilds between them, including a batch that exhausted its invocation allowance. It retained failed calls, used a corrected-source/non-clean arm, and still has differential gaps. It is not an unchanged single-build end-to-end result or a causal model ranking. Canary costs remain separate from campaign totals.

The suite remediation, failure-identity/diagnostic-custody fixes, and later live-repair evidence remain uncommitted in the reviewed private tree. Passing host-runnable checks do not qualify a clean published release, Windows-native behavior, hardware integration, or the complete visual-origin matrix.

## November 15: uninterrupted runs and provenance follow-up

The first unchanged native host batch completed **53/53 units** in **53m 26s**, recording **168 calls**, **2,690,898 reported tokens**, and four automatic repair calls, including two compiler-repair rounds. Retained checks found no restart, manual generated-code edit, or source/configuration/binary drift during the batch, and no failed, cancelled, or skipped generation units. Automatic bounded repair inside the run does not count as an operator interruption.

That completion had incomplete provenance: **43/53 admissions succeeded and ten were unavailable**. A controlled diagnostic reproduced a repair-context attribution loss, but the exact original cause remains unproved because full repair prompts were not retained. A separate adversarial test exposed unmarked protected source in steering reaching the provider and committing despite admission denial; watermarking blocked the marked counterpart. The first run is not retroactively relabeled as having full lineage coverage.

A later scoped repair addressed custody and enforcement on the tested paths. **V59** then completed **53/53 units and 53/53 provenance admissions**, uninterrupted, in **51m 26s**, with **166 calls** and **2,614,498 reported tokens**. Retained evidence pairs had no mismatch. The associated suite recorded **5,437 passing tests and 35 skipped**. Controlled tests blocked the previously exposed unmarked steering case before the provider and preserved custody for clean repair. That is scoped regression/adversarial evidence, not a proof against every possible source exposure or prompt injection.

Controlled/mock behavioral evaluation retained **288/309 normalized differential cases** and **9.4865/10 (9.49 rounded), confidence B**. The workload is TEM regeneration from a graph containing requirements derived from corrected source. Keep its **non-clean, source-corrected** classification: 53/53 generation is not 309/309 behavioral parity or a clean new-product benchmark. The two run totals are separate observations, not a controlled speed or token comparison.

The uninterrupted-run milestone is now met. It does not establish a clean committed release, independent reproducibility, Windows-native qualification, live Unreal/Stream Deck behavior, native packaging, or hardware integration. The reported V59 checkpoint still contained uncommitted changes. Raw artifacts stay private and the July correction is unchanged.

## November 15: graTeha first increment

**graTeha**, also called **Gratia**, is a separate semiautomatic experiment harness rather than a replacement name for grAIph. Its first completed increment provides:

- relational experiment provenance and versioned ordered test specifications;
- graph import and a human-readable contract/prompt explorer with filtering and field-linked issues;
- evaluator review/calibration controls, keeping model-reported issues separate from verified findings;
- shared frontend and MCP access to the experiment engine;
- a scripted repair lifecycle that requires human approval before dispatch.

The first increment recorded **58 passing tests**. Two isolated grAIph processes exercised real MCP/direct-graph operations without model calls. Codex thread creation and discovery of 31 MCP tools were checked without inference. A retained TEM adapter replay reproduced **9.4865/10, confidence B**, without new model calls; it preserves V59's non-clean label and is not another generation trial.

At this dated checkpoint, native VS Code host execution was unsupported; live generation and complete usage accounting were incomplete; Claude interoperability was untested; and live repair was blocked pending source-confinement qualification. A scripted six-cell matrix and MCP discovery do not qualify the live six-cell benchmark matrix, real-provider parallelism, or model-generated authoring/evaluation. Later harness increments must report their own evidence rather than silently expanding this first-increment claim.

## Later November 15: reporting, consoles, and live authoring

The later graTeha increment recorded **79 Python tests passed**, plus Node chart checks and report-schema validation. A shared versioned report projection now serves frontend and MCP views. Project/suite/run dashboards separate token, time, and quality scopes; stage consoles expose bounded stdout, stderr, and events. Model notes and unverified diagnoses remain distinct from executed findings. The frontend connects Design → Evaluator → Prepare → Run → Inspect controls.

A bounded live Codex authoring trial produced a **12-step prompt-only software design with criteria**. The first evaluator attempt hit its output guard while preserving the design; an evaluator-only retry produced a **10-check draft**, still unreviewed and uncalibrated. The three turns recorded **61,716 tokens**. This is live authoring evidence, not generated application code, a completed experiment, or live repair. The unsuccessful attempt remains part of the cost and outcome record.

The expanded MCP catalog was discovered through the native Codex client without inference turns. The TEM replay completed 17 substeps, reproducing **9.4865/10, confidence B**, without new model calls. Historical generation usage is counted once rather than duplicated for each replay observation; unavailable historical logs remain unavailable.

Claude native interoperability was still untested, live generation/usage adapters remained incomplete, and native VS Code host execution remained unsupported. These reporting and authoring results do not remove the separate live-repair source-confinement gate or qualify all six live benchmark cells.

## November 21: retained-evidence recheck and documentation release

PR #4’s earlier November update is already merged. The v0.4.0 documentation-only preview preserves it and adds the current evidence review. The latest retained successful saved-graph batch remains November 15’s V59 result, not a new November 21 generation. The review rehashed source/extension/output inventories and immutable inputs and independently matched graph intent, edges, and dependency contracts. Its complete logging record still covers 166 input/response pairs.

The later harness audit distinguishes real workspace/tool use from full-harness equivalence: direct Codex used an isolated custom permissions/tool profile; grAIph used externally supplied planning, topology, and dispatch. These constrained configurations do not establish a general harness ranking. Future comparisons must deliver the same prompts one at a time and let each full harness use its normal workspace editing, tools, and verification loop. Historical code-only controls remain outside that baseline.

The public release is documentation, not private implementation or product qualification. No product tests or external model runs were started for this review. See [Harness comparison scope](./harness-comparison-scope.md) and [release notes](./releases/2026-11-21.md).

## Gates that remain open

- The fresh relation-surface/binding-contract provider A/B, with separately attributed structural contracts and at least five repetitions per arm, remains pending.
- The September one-origin/three-viewer synchronization measurements remain historical evidence. No newer complete all-origin performance matrix or passing 250 ms responsiveness result was established by this review.
- Full process memory, autonomous harness evolution, and general provider-quality or cost superiority remain unproven.
- The later passing macOS source cut is uncommitted; clean release and Windows/native qualification remain open.
- Local fixtures, an isolated installed-host check, and small development comparisons do not establish production readiness or cross-platform qualification.

The governing boundary is [Current state](../CURRENT_STATE.md). Historical September evidence remains in [its original progress note](./progress-2026-09.md); the July benchmark correction remains unchanged.
