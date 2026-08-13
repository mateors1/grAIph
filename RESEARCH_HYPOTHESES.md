# grAIph — Research Hypotheses

**Last reviewed:** 2026-08-13

This document records testable hypotheses and their current evidence status. It is not a publication map, novelty declaration, venue plan, or benchmark leaderboard.

Implementation facts belong in [CURRENT_STATE.md](./CURRENT_STATE.md). The active controlled experiment is in [docs/experiment-protocol.md](./docs/experiment-protocol.md).

---

## How to read this document

Each hypothesis has four separate fields:

- **Mechanism:** what the implementation can do;
- **Question:** what remains empirically open;
- **Test:** how the question could be evaluated;
- **Falsifier:** what result would count against the hypothesis.

The existence of a mechanism is not evidence that it improves outcomes. A prior score is not reused as current evidence unless it satisfies the active benchmark protocol.

---

## H1 — Explicit relation surfaces and usage bindings

**Mechanism:** A graph edge can carry provided and required surfaces and can describe usage kinds such as call, construct, inject, reference, or imported-value read.

**Question:** Do explicit relation surfaces and usage bindings reduce run-to-run variance or architectural drift compared with otherwise equivalent graph generation?

**Test:** Compare a baseline arm with relation-surface and binding contracts disabled against a treatment arm with them enabled, using a fresh graph, source suppression, complete manifests, and a real provider.

**Falsifier:** No reduction in spread, or a consistent quality/regression cost after controlling for provider, source, graph, and sampling conditions.

**Status:** Implemented but experimental; fresh provider-backed A/B pending.

---

## H2 — Structural contracts under source suppression

**Mechanism:** A node can carry a source-derived structural surface describing class shape, public members, and inheritance or implementation expectations without exposing source bodies.

**Question:** Do structural contracts improve architectural fidelity when the model cannot see the original source body?

**Test:** Hold the graph, provider, model, and source-suppression policy constant while varying only the structural-contract lever. Measure public shape, interfaces, imports, compiler results, and per-file fidelity.

**Falsifier:** No improvement over the baseline, or improvement that disappears when structural content is accounted for as a separate context cost.

**Status:** Implemented but experimental; not promoted to a default.

---

## H3 — Graph-informed context allocation

**Mechanism:** Neighbor context can be represented at multiple levels: full body, symbol surface, relation/edge information, or adaptive reductions. The current implementation also contains a distance-aware allocation policy.

**Question:** Which representation policy best balances fidelity, variance, and token budget?

**Test:** Run separately attributed context arms with the same source, graph, provider, model, sampling, and sample size. Record token budgets and the exact representation sent to each node.

**Falsifier:** A simpler policy matches or beats the graph-informed policy, or the proposed policy's benefit depends on unreported source leakage.

**Status:** Open research question. No fixed decay constant is treated as settled.

---

## H4 — Evidence reconciliation

**Mechanism:** Generated code can be analyzed for actual imported-symbol use. Evidence can create or strengthen edges and can retract unsupported evidence-derived contracts while preserving declared architectural intent.

**Question:** Does feeding observed implementation evidence back into the graph improve subsequent generation quality, graph accuracy, or debugging visibility?

**Test:** Compare generation with reconciliation disabled and enabled over multiple waves. Separate newly observed evidence from user-declared edges and record any contract retractions or unfulfilled requests.

**Falsifier:** Reconciliation increases false edges, fails to improve graph accuracy, or changes outcomes only through untracked prompt/context differences.

**Status:** Implemented but experimental; AST-assisted analysis is capability-graded and opt-in.

---

## H5 — Topology-derived layout

**Mechanism:** Graph waves and cross-wave relationships can produce a deterministic layout for nodes and output placement.

**Question:** Does topology-derived placement improve reproducibility, import/path consistency, or generation outcomes compared with simpler deterministic layout policies?

**Test:** Compare topology-derived layout with the baseline layout under identical graph and generation conditions. Evaluate placement stability and generation-related measures separately.

**Falsifier:** No reproducibility benefit, worse path behavior, or no measurable generation effect after controlling for context and pipeline changes.

**Status:** Implemented behind explicit controls; generation-quality benefit is unvalidated.

---

## What this document does not claim

The hypotheses above do not establish:

- benchmark superiority over other coding systems;
- a universal law of LLM code generation;
- publication readiness;
- research novelty without a literature review and external validation;
- that any experimental mechanism should be enabled by default;
- that historical byte-identical scores are valid current evidence.

The July correction and the active eligibility rules are documented in [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md) and [archive/2026-07-17-benchmark-correction.md](./archive/2026-07-17-benchmark-correction.md).
