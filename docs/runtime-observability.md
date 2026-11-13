# Runtime Observability

**Status:** Observation and bounded attempt memory implemented; full process memory incomplete

**Reviewed:** 2026-11-04

grAIph separates observation from authority. Streaming tokens, lifecycle events, diagnostics, and replay records can explain what is happening without becoming a second control plane.

---

## Provider streaming

Providers may expose different native streaming APIs. grAIph normalizes them into provider-neutral execution events while preserving one invariant:

> The final provider response is authoritative for pipeline interpretation.

Streaming events may carry execution identity, provider/model identity, token deltas, accumulated text, completion, cancellation, or failure information. A partial stream is not accepted as a complete response, and UI progress does not commit files or graph state.

---

## Runtime events

The runtime event surface is a bounded, sanitized, read-only projection. Events have stable identity, monotonic sequence, timestamp, type, source, and optional correlation such as run, task, candidate, job, graph, or tool invocation.

Current producer classes include:

- provider and agent lifecycle;
- host candidate/finalization boundaries;
- graph changes;
- MCP tool lifecycle;
- job lifecycle;
- hook-registry observation;
- durable tool-invocation settlement.

Subscribers cannot block or alter the underlying generation or tool execution. Slow or failing observation consumers do not become runtime authority.

---

## Bounded replay and MCP projection

Recent events and run-scoped events can be exposed as bounded MCP resources and subscriptions. Replay is ordered and limited; it is not an unbounded event store.

Payloads are allow-listed and privacy-sanitized. Source bodies, secrets, raw provider prompts, unrestricted tool output, and host credentials are not made observable merely because an event exists.

---

## Authority hierarchy

The following artifacts answer different questions:

| Artifact | What it can establish |
|---|---|
| Stream event | Observable provider or agent progress |
| Runtime event | A bounded lifecycle or state-transition observation |
| Tool invocation record | The settled lifecycle and effect status of one tool call |
| Unverified candidate | Proposed files and graph mutations after pipeline processing |
| Host receipt | What the authoritative host persisted |
| Committed state | Graph/file state verified against the accepted candidate and receipt |

No earlier row implies a later row.

---

## Record Keeper and process memory

Record Keeper can now retrieve durable revision-attempt records, including rejected candidate snapshots, raw evaluations, and accepted-to-rejected lineage. Local repair fixtures exercise retrieval in a fresh process and require new evaluation before a repaired candidate can be accepted.

This bounded attempt memory is separate from event replay. The runtime event stream remains bounded and observational; it does not supply complete long-horizon memory or independently select strategies. Full recurrence retrieval and the broader Record Keeper process-memory role remain incomplete.

---

## Controlled adaptation and AVO-style research

An explicitly enabled controller can use current host state and evaluation evidence to propose bounded generation, retry, evaluation, and replanning actions. It records policy, reservations, usage, and interruption state. The trusted host enforces action admission and candidate acceptance; observations never grant themselves authority.

Ordinary Generate remains deterministic. Controlled adaptation requires its own enablement, evaluator, and budgets; the presence of events or memory does not enable it. Autonomous harness evolution and automatic promotion remain unimplemented research directions.

Small diagnostic-repair comparisons have not justified changing the default. See [Controlled adaptation and project QA](./controlled-adaptation.md) and [November progress](./progress-2026-11.md).

See [Bounded tool runtime](./tool-runtime.md), [Pipeline overview](./pipeline-overview.md), and [Current state](../CURRENT_STATE.md).
