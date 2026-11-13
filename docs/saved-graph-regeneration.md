# Saved-Graph Validation and Regeneration

**Status:** Committed development implementation with scoped evidence; release qualification open

**Reviewed:** 2026-11-13

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

The compiler, source-admission checks, prompt construction, parsing, dependency preparation, and runtime transaction executed. Initial generation, the repair response, and model Conformance were scripted; the VS Code filesystem used a test adapter. The oracle returned archived passing code. This establishes feedback routing and acceptance behavior, not fresh-model repair quality or installed-host qualification.

## Other checks and visible changes

Supported static HTML-emission checks parse inline JavaScript produced by strings/templates and recognized sinks without executing the application. Dynamic behavior remains outside this syntax check.

Audited default paths now derive framework/SDK choices from project evidence instead of generic architectural labels. Regression coverage for those paths is not proof of neutrality for every language or computed import.

The graph viewer offers uniform or relationship wire colors/styles and collapsed-edge classification. These are viewer presentation controls, not changes to dependency or commit authority.

## Current evidence boundary

Builds, production typechecks, selected regressions, and saved-graph replays passed within their retained scope. The November 13 checkpoint also recorded failing broad-suite and recursive-test checks. Only some failures were reproduced on the earlier baseline; the remainder cannot all be labeled pre-existing. Remediation is active, and no clean broad-validation result is claimed here.

A separate saved-graph application campaign completed its declared output set over multiple native batches, with rejected compiler samples and later attempts retained. It was a corrected-source, non-clean arm with remaining differential gaps and unmeasured native integration. It does not complete the relation-surface A/B or establish model superiority.

See [Current state](../CURRENT_STATE.md), [November progress](./progress-2026-11.md), [Pipeline overview](./pipeline-overview.md), and [Evidence reconciliation](./evidence-reconciliation.md).
