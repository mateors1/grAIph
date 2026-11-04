# Controlled Adaptation and Project QA

**Status:** Implemented as explicit controlled paths; benefits remain experimental

**Reviewed:** 2026-11-04

grAIph now has an opt-in adaptive controller and a native project-QA workflow. They use independent evaluation and host-controlled acceptance. The ordinary decompressed generation pipeline remains the default.

---

## Controller and host responsibilities

A controlled run pins its targets, policy, evaluator, provider selection, and budgets. Native execution requires explicit enablement and a trusted evaluator profile registered by the host. Evaluator registration is immutable while a run owns the workspace.

The selector can propose reading state, generating or retrying a node, evaluating a detached candidate, bounded replanning, stopping, or finishing. The controller validates the proposal against current host state before dispatch. Dependency readiness, attempt limits, graph freeze, stale identities, and progress limits constrain admission.

A selector decision is not an accepted candidate. Completion requires target acceptance evidence bound to the current candidate and evaluation. The host retains graph/file persistence and commit authority. Provider-native orchestration is a separate generation mode; it is not this controller.

## Durable accounting and interruption

Run records retain policy identity, action tickets, reservations, usage, observations, and terminal status. Calls and tokens are reserved before dispatch. Missing usage remains charged through its reservation. Selector and generation work both consume the controlled run's allowance.

Cancellation and exhausted budgets stop further admission. An unresolved pending action or uncertain host effect enters recovery-required state. Restarting a process does not authorize replay or erase provider charges. Recovery must reconcile existing evidence before new work can proceed.

These controls constrain orchestration and accounting. They do not prove optimal action selection, monetary savings, or provider billing limits.

## Attempt memory and advisory diagnosis

Attempt storage can retain immutable rejected candidates, raw evaluator reports, and lineage into later repairs. Retrieval verifies candidate, report, base revision, and evaluator identities. A repair receives a new candidate identity and fresh evaluation; an earlier passing result cannot be transferred to changed code.

Advisory diagnosis can explain a finding or propose a repair without rewriting the raw report. The independent evaluator and Quality Gate remain separate. Fresh-process repair fixtures establish this retrieval and acceptance plumbing, not general LLM repair quality.

Full Record Keeper process memory—including broad recurrence retrieval and evidence-informed strategy selection—remains incomplete. Persisted attempt lineage is an implemented subset.

## Project readiness after generation

Native project QA asks whether the current project satisfies trusted requirements. Its report binds the profile, evaluator, graph revision, source, runtime, and host instance. A profile supplies independent commands and graph-owned paths; generated prose cannot register those checks.

The workflow distinguishes assessment, repair, integration, and reopen qualification:

1. Run the registered checks against the current project.
2. Attribute actionable findings to an unambiguous generation owner.
3. Generate a detached repair using the controlled native path.
4. Evaluate it against original contracts and relevant project regressions.
5. Accept through the host and reassess the project.
6. Verify integration and the complete tested artifact inventory.
7. Reopen in a different native host instance and verify the sealed evidence before projecting qualified status.

Infrastructure failure remains unverified. Unsupported multiple-owner/SCC expansion requires a host-approved repair plan. Later source, profile, runtime, or artifact changes invalidate the projected verdict. An accepted node is not automatically a qualified project.

## Permissions and interfaces

Native commands provide evaluator registration and controlled start/status/cancel operations. Project QA has separate run, repair, status, cancellation, reconciliation, and reopen checks.

MCP project-QA mutations require a dedicated explicit allow-list, empty by default. Graph-edit drive access does not grant those operations or evaluator-registration authority. Safe browser settings cannot enlarge native host permissions.

## Evidence and open questions

Deterministic controller, memory, evaluator, and host fixtures cover bounded lifecycle behavior. An isolated installed-extension check covers persistent settings and adaptive command availability. Small provider-backed diagnostic comparisons have not shown a reason to promote diagnostic guidance as the default.

The remaining question is whether controlled adaptation improves quality or total work over deterministic generation on fresh, independently evaluated tasks. It requires its own declared arms and accounting; it does not satisfy the separate relation-surface experiment.

See [November progress and limitations](./progress-2026-11.md), [Pipeline overview](./pipeline-overview.md), and [Current state](../CURRENT_STATE.md).
