# Saved-Graph Validation and Regeneration

**Status:** Committed development implementation with scoped evidence; release qualification open

**Reviewed:** 2026-11-15

A saved grAIph graph can now carry source-backed dependency/API evidence and portable dependency requirements into later regeneration. Project-file scaffolding, validation-environment preparation, model generation, and host acceptance have separate results.

## Validate the graph before generation

For supported JavaScript/TypeScript paths, deterministic checks reconcile relative and lazy imports and validate exported symbols, member visibility, constructor/static/instance use, supported call shapes, and port ownership. Persisted evidence is bound to source and graph identities; stale or incompatible evidence cannot establish admission.

A bounded explicit repair works on detached graph state, revalidates the result, and commits through the authoritative host. Changed dependency/API requirements invalidate affected generation state. Optional behavioral model review is separate from deterministic admission. Attributed model judgments remain judgments rather than semantic proofs.

Retained source-bound receipts can support later checks without re-importing the project. This does not authorize undeclared source access during synthesis or waive benchmark source-suppression rules.

## Prepare validation dependencies independently

Portable graph metadata records project scopes, manifest/lock hashes, package requirements, resolved versions where known, runtime constraints, and consumers. Machine-specific paths and readiness remain in local environment receipts.

The Node path can reconstruct an isolated QA environment even with project-file scaffolding disabled. It honors captured lockfiles, disables installation lifecycle scripts, and verifies installed versions, declarations, and toolchain constraints. Reproducibility depends on the captured inputs; an unpinned resolution is not reported as locked.

Readiness is bound to dependency and consumer identities. Missing environments, stale receipts, and unsupported scopes produce unavailable or partial evidence. They do not become syntax faults to repair in candidate code. Local/workspace packages need an appropriate resolver; non-Node environments and native toolchains remain incomplete.

## Route final group compiler feedback into repair

Native and headless group compilation can route owned compiler errors to the retained rejected candidate through bounded Coder repair. The corrected candidate traverses normalization, Conformance, and validation again; the complete group is reassembled and compiled before host acceptance. Unaffected members are retained.

Repair preserves the original invocation accounting and configured attempt limit, including coordinator retries. Missing dependency evidence, unowned diagnostics, non-compiler gates, invalid candidate hashes/paths, and stopped invocations remain rejections. Failed candidates cannot publish files or index changes.

A November 13 saved-graph replay checked that repair disabled committed nothing and repair enabled made one repair request, revisited Conformance, and committed one graph revision. Inputs and graph intent were preserved, with scaffolding off and no import. A subsequent uncommitted follow-up also verified that both complete compiler diagnostics and the rejected code reached the provider request.

In that earlier replay, the compiler, source-admission checks, prompt construction, parsing, dependency preparation, and runtime transaction executed. Initial generation, the repair response, and model Conformance were scripted; the VS Code filesystem used a test adapter and the oracle returned archived passing code. That replay remains routing evidence.

Later receipts record a natural compiler error repaired by a live model in the native host, followed by real Conformance, group compilation, and acceptance. A separate controlled live canary accepted a repair to known faults with a test filesystem adapter; an earlier live candidate failed Conformance, and a readiness rejection made no remote call. This adds scoped live evidence rather than a general repair-success claim.

## Other checks and visible changes

Supported static HTML-emission checks parse inline JavaScript produced by strings/templates and recognized sinks without executing the application. Dynamic behavior remains outside this syntax check.

Audited default paths now derive framework/SDK choices from project evidence instead of generic architectural labels. Regression coverage for those paths is not proof of neutrality for every language or computed import.

The graph viewer offers uniform or relationship wire colors/styles and collapsed-edge classification. These are viewer presentation controls, not changes to dependency or commit authority.

## Current evidence boundary

The early November 13 checkpoint recorded broad and recursive-check failures. Later retained macOS runs passed build, production typechecks, recursive package checks, and the root suite, with 32 skips classified. Those remediation/repair changes remain uncommitted; the passing source cut is not a clean released artifact. See the [evidence guide](./evidence-guide.md) for counts, skip categories, and remaining gates.

The later saved-graph application campaign completed 53 output units across three native batches with two intervening engine fixes/rebuilds; rejected attempts and a budget-exhausted batch were retained. No manual generated-code edits or re-import were recorded. It was a corrected-source, non-clean arm with remaining differential gaps and unmeasured native integration. It is not an unchanged single-build end-to-end run, does not complete the relation-surface A/B, and does not establish model, speed, or cost superiority.

See [Current state](../CURRENT_STATE.md), [November progress](./progress-2026-11.md), [Pipeline overview](./pipeline-overview.md), and [Evidence reconciliation](./evidence-reconciliation.md).
