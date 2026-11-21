# Coding Harness Comparison Scope

**Reviewed:** 2026-11-21

A coding harness comparison must exercise each complete harness: its normal workspace editing, tool use, and verification loop. Deliver the same frozen benchmark prompts one at a time. A code-only prompt or a returned file snapshot is a different intervention and belongs only in an explicitly labeled historical control.

## What the retained pilot exercised

The later graTeha pilot used real inference and retained measured outcomes, but its configurations impose important limits:

| Route | Exercised behavior | Qualification |
|---|---|---|
| Direct Codex session | Workspace edits, command execution, and verification in a saved session | A custom isolated profile disabled web search and shell network access and set approvals to `never`. This does not establish parity with the normal Desktop tool inventory or approval profile. |
| grAIph through MCP or the native host | Generation, QA, repair, and publication inside a supplied graph arrangement | An external planner converted prompts into cumulative contracts for a fixed Python generation unit; the driver chose graph updates and generation dispatch. This measures constrained graph generation, not grAIph independently choosing the workflow from the original prompts. |
| Earlier code-only trials | Structured file snapshots with model tools disabled | Historical controls, excluded from the full-harness baseline. |
| Test/evaluator authoring | Structured proposals for benchmark tasks and grading | A separate authoring activity, not application generation or a harness result. |

The native route really dispatched host generation. The direct route really used workspace tools. Those facts do not remove the profile and topology restrictions. Scores remain measurements of the tested configurations, not a conclusion about which unrestricted harness is better.

## Requirements for a new full-harness baseline

- Give each harness the same original prompts, one at a time; retain exact prompt delivery.
- Exercise its normal editing, tools, and verification loop. Declare and verify the intended permissions, tool inventory, environment, and approval policy.
- Let the harness decide the work it normally owns. If topology, contracts, or dispatch are supplied externally, declare that as a constrained or saved-graph arm.
- Hold the evaluator and task inputs fixed. Retain failures, unavailable checks, retries, and all attributable model/controller work.
- Keep code-only and file-snapshot trials outside the full-harness baseline. Do not silently promote historical controls.
- Treat provider cache observations, requested model identity, and served-model attestation as separate evidence. Similar labels do not prove equivalent serving conditions.

No new comparison was run for this documentation review. It does not establish speed, cache, token-cost, or quality superiority. The relation-surface experiment in [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md) remains a separate track.

See [Reading the evidence](./evidence-guide.md), [Research hypotheses](../RESEARCH_HYPOTHESES.md), and [Current state](../CURRENT_STATE.md).
