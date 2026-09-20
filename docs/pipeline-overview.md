# grAIph Pipeline Overview

**Reviewed:** 2026-09-20

This overview describes the public architecture boundary. The implementation remains private; names and phase descriptions explain the design without exposing private source.

---

## From shared graph to committed state

~~~text
authoritative graph + edition face
              ↓
analysis + immutable topology plan
              ↓
task plan + generation contract
              ↓
minimum capability plan
              ↓
code synthesis + normalization
              ↓
QA repair + conformance repair
              ↓
independent Quality Gate
              ↓
graph evidence + reconciliation
              ↓
unverified candidate assembly
              ↓
host persistence + canonical receipt
              ↓
verified committed graph/file state
~~~

The pipeline is longer than a prompt-to-completion call because planning, generation, validation, repair, graph evidence, persistence, and commit authority answer different questions.

---

## Orchestrator and PATH B

The Orchestrator is the public sequencing facade. It selects the explicit mode, creates run context, delegates the default decompressed PATH B route to a phase executor, and exposes host-finalization seams.

The phase executor is not an autonomous scheduler. It preserves a typed, bounded order:

| Phase | Responsibility |
|---|---|
| Analysis | Inspect graph coherence, generation order, cycle policy, and topology plan |
| Task Planning | Resolve patterns and decompose the node request into bounded tasks |
| Generation Contract | Compile the authoritative task, graph, language, source, and policy brief |
| Capability Planning | Resolve semantic requirements to a minimum policy-eligible tool surface |
| Code Synthesis | Produce candidate files and deterministic normalization artifacts |
| QA Repair | Run language-appropriate checks and bounded candidate repair |
| Conformance Repair | Compare candidate behavior with graph/structural contracts and repair within bounds |
| Quality Gate | Decide independently whether blocking findings remain |
| Grapher | Produce graph-facing relations, surfaces, and evidence |
| Graph Reconciliation | Apply the accepted topology/evidence mutation to detached state |
| Candidate Assembly | Bind files, mutations, findings, provenance, and candidate identity |

Host persistence and Record Keeper follow candidate processing. The phase executor does not grant itself durable graph or workspace authority.

Alternate modes—including whisper regeneration, compressed profiles, Energy Saver, and provider-native orchestration—are explicit paths. Their existence does not mean every stage runs in every mode, and each route must preserve the candidate/commit boundary.

---

## Topology analysis

Topology Scrutineer produces an immutable plan bound to the input graph fingerprint. The plan can contain:

- graph analysis and strongly connected components;
- deterministic generation waves;
- edge additions or removals;
- cycle-repair decisions;
- layout and adjacency information consumed by later phases;
- provenance and evidence needed to reject stale reuse.

Planning does not mutate the authoritative graph. One reconciliation executor later applies an accepted plan to detached candidate state.

---

## Generation contract

The generation contract is the authoritative brief for synthesis. It combines:

- target description and edition-face metadata;
- graph neighbors and dependency direction;
- provided and required relation surfaces;
- binding and structural contracts when enabled;
- generated files and safe symbol surfaces;
- language capability information;
- source-access and benchmark policy;
- token, context, and model constraints;
- the topology-plan identity.

The graph can represent a direct dependency as source, symbols, structural surface, relation information, or identity. Which representation is best remains an empirical question.

---

## Capability planning and leases

Task planning may describe semantic needs, but it does not choose its own authority. After the generation contract exists, a deterministic planner filters the capability catalog through hard policy, compatibility, coverage, and budget.

At an actual model turn, the host materializes an exact effective surface from:

- the plan;
- current agent and phase;
- candidate identity;
- provider/model identity;
- session and source policy;
- tool, call, byte, time, and expiry budgets.

Read-only evidence precedes candidate-only mutation. Capability expansion requires a broker-issued lease delta. See [Bounded tool runtime](./tool-runtime.md).

---

## Candidate validation and repair

Checks are appropriate to the selected language and profile:

- structure and syntax;
- compiler or best-effort language QA;
- import/path and emission integrity;
- graph and contract conformance;
- source-access and benchmark policy;
- quality-gate rules.

Repair operates only on ephemeral candidate state. Before-hash, scope, attempt, progress, repetition, and oscillation checks keep the loop bounded. QA and Conformance produce findings; Quality Gate remains independent and tool-free.

Unknown languages do not silently inherit TypeScript behavior. Capability is graded per operation.

---

## Evidence and graph reconciliation

After candidate emission, the pipeline can inspect actual use of neighboring symbols. Evidence may describe calls, construction, injection, references, or imported values consumed as data.

Evidence-derived relations remain distinct from declared architecture. Protected declared or import-derived contracts are not silently overwritten. Unsupported evidence can be retracted while an unfulfilled declared request remains visible.

The accepted graph mutation is applied to detached state and bound to graph/topology fingerprints. The host later verifies that persisted state matches the accepted result.

See [Evidence reconciliation](./evidence-reconciliation.md).

---

## Candidate versus committed outcome

An unverified candidate can contain:

- candidate identity;
- generated files;
- proposed graph additions and removals;
- topology and graph fingerprints;
- warnings, errors, and finding deltas;
- tool and provider provenance;
- benchmark eligibility information.

It becomes committed only when:

1. required validation and Quality Gate pass;
2. the authoritative host accepts and persists it;
3. a canonical receipt covers the candidate targets;
4. committed graph/file state is verified against the candidate.

A model response, successful tool call, validation pass, or graph proposal does not cross this boundary alone.

---

## Host and interface boundary

VS Code, browser clients, MCP, and headless execution may have different capabilities, but they must not invent separate authority models.

The host owns:

- workspace and credential authority;
- durable persistence;
- effect approval;
- graph revision and publication;
- commit receipts;
- verification of committed graph/file state.

Streaming and runtime events expose progress without replacing this boundary. See [Runtime observability](./runtime-observability.md).

---

## What this overview does not establish

This architecture does not establish that:

- graph structure always improves generation;
- the default pipeline outperforms a raw model or another coding system;
- a minimum capability surface improves provider quality;
- every provider or language is interchangeable;
- candidate repair causes better final code in general workloads;
- experimental contracts should become defaults;
- Record Keeper already provides complete process memory;
- adaptive orchestration or AVO is active.

Those questions and boundaries are tracked in [Current state](../CURRENT_STATE.md), [Research hypotheses](../RESEARCH_HYPOTHESES.md), and [Experiment protocol](./experiment-protocol.md).
