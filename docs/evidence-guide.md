# Reading the Evidence

**Reviewed:** 2026-11-15

grAIph has implemented mechanisms, passing checks under declared conditions, preliminary real-provider observations, and experiments that remain proposed. These support different conclusions. [Current state](../CURRENT_STATE.md) is the governing claim boundary; this guide explains how to read it.

## Four kinds of statement

| Kind | What it means here | What it does not establish |
|---|---|---|
| Implemented mechanism | The private development code contains the described path, such as graph-based scheduling or host-controlled finalization | Distribution, general correctness, or comparative benefit |
| Scoped verified behavior | Retained checks passed for identified inputs, source, environment, and evaluator | Every platform, workload, or future source revision |
| Preliminary observation | A small development campaign or live-provider case exhibited the recorded behavior | A causal improvement, model ranking, or population-level success rate |
| Proposed experiment | A question, comparison, controls, and decision criteria are specified | A completed result |

Deterministic orchestration describes how software selects and sequences work. It does not imply reproducible model output. Acceptance receipts establish what the host committed; they are not proofs of semantic correctness.

## Configuration is part of the claim

The shared tool executor, candidate leases, capability expansion, and adaptive controller default off (`enableUnifiedToolExecutor`, `enableCandidateToolLeases`, `enableCapabilityExpansion`, and `enableAdaptiveOrchestrator`). Qualification runs enable particular routes explicitly. Their bounded controls are not universal/default protection for every generation mode.

Tool/action/resource allow-lists are runtime policies, not decoder-level vocabularies that constrain each component's generated tokens. Their implementation does not demonstrate a complete return-channel bypass or specialist sanitization architecture.

## Latest local verification

The early November 13 checkpoint recorded 120 failures and 27 skips. Those failures remain part of the record. Later remediation separated platform requirements from host-runnable failures and retained the earlier artifacts.

The latest retained verification receipt is dated **November 14, 2026 UTC**, covering checks completed November 13 in Bogotá. This documentation review is dated November 15. The macOS root run passed **5,356 tests with zero failures and 32 classified skips**. Workspace build, production typechecking, and recursive package checks also passed. These are overlapping verification scopes, so their counts must not be added as independent evidence.

The 32 standard-run skips comprise:

- 12 Windows-native tests requiring that platform;
- 14 historical replays whose accepted source/schema/catalog identities are immutable;
- two live-provider tests outside the standard run;
- four opt-in diagnostics that passed separately.

Seven earlier failures required Windows APIs and became explicit platform deferrals. Two earlier skips became runnable and passed. The later root run has the same skipped-test identities as the remediation audit. A passing current suite does not rewrite historical receipts.

The remediation and subsequent live-repair changes remain **uncommitted in the reviewed private working tree**. Their receipts are evidence for those source cuts, not for a clean published release. Windows-native qualification, native integration, the full visual-origin matrix, soak testing, and release packaging remain open.

## Compiler repair: three separate scopes

1. **Scripted replay:** retained faulty candidates and diagnostics exercise real compiler checks, prompt construction, parsing, and host transaction code. Scripted responses establish routing and acceptance behavior rather than model repair ability.
2. **Natural live repair:** a native application campaign recorded a real model repairing an owned compiler error, followed by real Conformance, group recompilation, and acceptance. This is a case observation, not a general success rate.
3. **Controlled live canary:** a separate fixture supplied known compiler faults; real repair and post-repair Conformance led to one accepted revision. An earlier candidate failed Conformance, and a local readiness rejection made no remote call. The fixture and its costs remain separate from the application campaign.

The application campaign completed 53 declared output units across three native batches, with two engine fixes/rebuilds between them and one batch exhausting its call allowance. No manual generated-code edits or re-import were recorded. It was a corrected-source, non-clean arm with remaining differential gaps. It is not an unchanged single-build end-to-end result or evidence of model, speed, or cost superiority.

## Other tracks keep their own limits

The September visual run showed one mutation origin converging across three viewers. Its p95 receipt-to-layout values exceeded the 250 ms target. Later opt-in diagnostics do not replace the complete origin/viewer matrix or establish that target is met.

Small diagnostic-guidance comparisons did not justify changing the default: the later six-case comparison passed all cases in every arm while clarified guidance used 2.099 times direct-repair tokens. Deterministic tool-surface results describe functional behavior, not measured provider-quality gains. See [November progress](./progress-2026-11.md) and the historical [September note](./progress-2026-09.md).

The relation-surface A/B remains pending. It requires fresh graphs per arm, controlled provider/backend/settings, source-suppression and leak audits, complete receipts, and at least five repetitions per arm under the [benchmark specification](../BENCHMARK_SPEC.md).

## Research and security scope

The implementation provides evidence about stage sequencing, bounded candidate processing, and the separation between model proposals and host acceptance. Role-specific agents and policy checks do not establish prompt-injection resistance or a complete security architecture for cooperating models. No adversarial prompt-injection evaluation is reported here. Broader architecture and security proposals require their own implementation and evaluation.

The raw implementation, logs, and some task inputs remain private. These public summaries are not independently executable reproduction packages. See [Getting started](./getting-started.md) for availability and evidence-access guidance.

## Integrity history

The project [retracted contaminated byte-identical benchmark claims in July 2026](../archive/2026-07-17-benchmark-correction.md). That correction remains prominent and unchanged. New results require positive generation evidence, declared source access, and complete provenance; earlier retracted scores are not valid baselines.
