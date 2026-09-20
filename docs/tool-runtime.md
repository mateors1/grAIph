# Bounded Tool Runtime

**Status:** Implemented with bounded functional evidence

**Reviewed:** 2026-09-20

grAIph treats tool use as an authority and evidence problem, not merely a list of functions attached to a prompt.

The current tool architecture was developed in three layers:

1. a trustworthy execution boundary;
2. a deterministic minimum-capability surface;
3. a bounded candidate-tool loop.

The system does not expose unrestricted shell or general-purpose workspace mutation to the generation agent.

---

## 1. Trustworthy execution boundary

Supported adapters route tool calls through one executor contract. The boundary is responsible for:

- schema and descriptor validation;
- trusted principal and route identity;
- authorization and confirmation before effects;
- input snapshotting before asynchronous work;
- source-access policy;
- cancellation and timeout;
- pre- and post-execution hooks;
- bounded model/log projection;
- local artifact retention;
- exactly one typed terminal state;
- durable lifecycle settlement where configured.

Security or authority metadata omitted by a route fails closed. HTTP or model-provided metadata cannot grant itself permission. A mutation with uncertain settlement is not automatically replayed.

Invocation evidence remains separate from project commit authority. A tool record can prove that a handler ran without proving that the host accepted graph or file state.

---

## 2. Semantic capability planning

Task planning expresses semantic requirements rather than concrete tool names. Examples include reading candidate symbols, inspecting repository symbols, or comparing diagnostics.

After the generation contract is built, a deterministic planner resolves requirements against the capability catalog.

The resolution order is:

1. hard policy exclusion;
2. compatibility and prerequisite checks;
3. required-capability coverage;
4. deterministic ranking;
5. tool-count and schema-size budgets;
6. reason-coded plan output.

Hard-policy-excluded schemas never reach the model. Plans are reusable only when their graph, contract, catalog, policy, provider/model, and phase identities still match.

---

## 3. Exact leases and effective surfaces

A plan is not authority. At an actual model turn, the host intersects the plan with current policy and issues an exact lease.

A lease is bound to:

- run and task;
- candidate identity and before hash;
- agent and phase;
- provider and model;
- exact tool set and resources;
- call, byte, and time budgets;
- catalog, contract, and policy identity;
- expiry and terminal state.

The model sees the effective surface produced by that intersection. It cannot widen the surface by naming another tool or editing transport metadata.

---

## Candidate-tool sequence

### Read-only evidence

The first active layer supplies bounded evidence such as:

- generation-contract facts;
- candidate symbols;
- repository symbols allowed by source policy;
- candidate diagnostics.

Read tools cannot commit graph or workspace state.

### Candidate-only repair

Repair may apply a patch only to ephemeral candidate state. The operation is before-hash guarded and scope checked. Findings are compared across candidate states so that fixed, introduced, unchanged, and severity-changed findings are explicit.

Repair loops stop on bounded attempts, no progress, repetition, oscillation, stale candidate identity, or policy failure.

### Brokered capability expansion

When the current surface cannot satisfy an evidenced need, the agent may request expansion. The request is evidence, not authority.

Only the capability broker can issue a lease delta. It applies hard policy, budgets, idempotency, current phase, and denial alternatives. Execution cannot expand itself, and a previously consumed grant cannot be reused merely because a replay reports that it once existed.

---

## Independent boundaries

The following remain outside adaptive candidate tooling:

- Quality Gate;
- Grapher and graph reconciliation authority;
- host persistence and finalization;
- settings and credential mutation;
- dependency installation;
- unrestricted workspace writes;
- general-purpose shell;
- confined multi-tool program execution.

QA and Conformance produce evidence. Repair may consume their findings, but the final gate remains tool-free and independent.

---

## Bounded evidence

The completed minimum-tool-surface program records:

| Layer | Bounded result | Qualification |
|---|---|---|
| Read-only evidence | 54/54 deterministic semantic observations passed | Functional evidence, not provider quality |
| Minimum surface | 77.26% fewer selected contract bytes at equal deterministic semantic accuracy | Contract bytes, not measured provider tokens or cost |
| Candidate repair | 30/30 attempted repairs showed finding progress; zero critical findings introduced | Bounded deterministic schedule |
| Expansion frequency | 3/24 representative normal tasks expanded | Frozen schedule, not production frequency |
| Expansion usefulness | 15/18 distinct grants were useful | Unused grants retained in the denominator |
| Final expansion campaign | 117/117 rows passed, zero provider calls | Functional and safety evidence only |

The program completed under amended functional criteria. It does not claim provider-quality improvement, causal code-quality gain, production latency, or general workload frequency.

---

## Deferred work

Confined composition—one capability that executes an internally bounded multi-tool program—was deliberately excluded. Individual leases, candidate-only mutation, and broker-controlled expansion must remain independently understandable before a later program could evaluate composition.

Durable process memory and adaptive orchestration are also separate concerns. Invocation records and runtime events provide evidence substrates, but Record Keeper does not yet consume them as complete long-horizon process memory.

See [Runtime observability](./runtime-observability.md), [Pipeline overview](./pipeline-overview.md), and [Current state](../CURRENT_STATE.md).
