# Runtime Observability

**Status:** Implemented locally; durable process-memory consumption remains future work

**Reviewed:** 2026-09-20

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

Record Keeper is the intended durable consumer for accepted and rejected attempts, evidence, provenance, recurrence, and retrieval. The current runtime provides useful substrates:

- bounded runtime events;
- durable invocation lifecycle records;
- candidate and host receipts;
- graph and topology fingerprints;
- evaluation and finding deltas.

The full process-memory role is not yet claimed. In particular, the current event stream is bounded and does not itself provide long-horizon retrieval, attempt lineage, or adaptive strategy selection.

---

## Adaptive and AVO-style loops

An external improvement loop could use the sequence:

~~~text
observe → attribute → replay → evaluate → propose a bounded change
~~~

That direction remains planned research. Adaptive execution is not enabled merely because the system can observe events. Reliable state, evaluation, provenance, and memory must exist before evidence can safely influence orchestration or topology.

See [Bounded tool runtime](./tool-runtime.md), [Pipeline overview](./pipeline-overview.md), and [Current state](../CURRENT_STATE.md).
