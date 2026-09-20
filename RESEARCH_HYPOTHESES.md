# grAIph — Research Hypotheses

**Last reviewed:** 2026-09-20

This document records testable hypotheses and their current evidence status. It is not a publication map, novelty declaration, venue plan, or benchmark leaderboard.

Implementation facts belong in [CURRENT_STATE.md](./CURRENT_STATE.md). The active controlled generation experiment is in [docs/experiment-protocol.md](./docs/experiment-protocol.md). Bounded visual and tooling evidence is summarized separately in [docs/progress-2026-09.md](./docs/progress-2026-09.md).

---

## How to read this document

Each hypothesis separates:

- **Mechanism:** what the implementation can do;
- **Question:** what remains empirically open;
- **Test:** how the question could be evaluated;
- **Falsifier:** what result would count against the hypothesis;
- **Status:** the current evidence boundary.

The existence of a mechanism is not evidence that it improves outcomes. Deterministic functional evidence, visual synchronization evidence, and provider-backed generation evidence are separate tracks.

---

## H1 — Explicit relation surfaces and usage bindings

**Mechanism:** A graph edge can carry provided and required surfaces and can describe usage kinds such as call, construct, inject, reference, or imported-value read.

**Question:** Do explicit relation surfaces and usage bindings reduce run-to-run variance or architectural drift compared with otherwise equivalent graph generation?

**Test:** Compare a baseline arm with relation-surface and binding contracts disabled against a treatment arm with them enabled, using a fresh graph per arm, source suppression, complete manifests, and a real provider.

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

**Mechanism:** Neighbor context can be represented at multiple levels: full body, symbol surface, relation information, structural contract, or identity. The implementation also contains distance-aware allocation policies.

**Question:** Which representation policy best balances fidelity, variance, token budget, and source isolation?

**Test:** Run separately attributed context arms with the same source, graph, provider, model, sampling, and sample size. Record provider-accounted tokens and the exact representation sent to each node.

**Falsifier:** A simpler policy matches or beats the graph-informed policy, the benefit disappears under equal token budgets, or the result depends on undeclared source exposure.

**Status:** Open research question. No fixed attenuation formula is treated as settled.

---

## H4 — Evidence and topology reconciliation

**Mechanism:** Generated code can be analyzed for actual imported-symbol use. Evidence can create or strengthen relations and retract unsupported evidence-derived contracts while preserving declared intent. Proposed graph mutations are carried by an immutable, fingerprint-bound topology plan and applied to detached candidate state before host finalization.

**Question:** Does feeding observed implementation evidence back into the graph improve later generation quality, graph accuracy, or debugging visibility?

**Test:** Compare generation with reconciliation disabled and enabled over multiple waves. Separate declared edges, observed evidence, candidate mutations, and committed graph changes. Record false additions, retractions, unfulfilled requests, and downstream outcomes.

**Falsifier:** Reconciliation increases false edges, fails to improve graph accuracy, creates stale or unauthorized mutations, or changes outcomes only through untracked prompt/context differences.

**Status:** Transaction and provenance mechanisms implemented; benefit remains experimental.

---

## H5 — Topology-derived visual layout

**Mechanism:** Graph waves and cross-wave relationships can produce a deterministic visual layout.

**Question:** Does topology-derived placement reduce human inspection cost or improve reproducibility compared with simpler deterministic layout policies?

**Test:** Compare topology-derived layout with baseline layouts under identical graph inputs. Evaluate visual stability and human inspection separately from generation outcomes.

**Falsifier:** No reproducibility or inspection benefit, unacceptable interaction cost, or any apparent generation effect disappears when context and path policy are controlled.

**Status:** Implemented behind explicit controls; the generation-quality and human-efficiency benefits are unvalidated.

---

## H6 — Minimum capability surfaces

**Mechanism:** Semantic task requirements can be resolved into a hard-policy-filtered, budgeted effective tool surface for one model turn.

**Question:** Can a smaller capability surface retain actionable coverage while reducing model-visible schema/context and avoiding quality regressions?

**Test:** Compare flag-off and active-minimum-surface provider runs with the same graph, contract, provider/model, candidate policy, and repetitions. Measure actionable requirement recall, provider-accounted tokens, first-pass quality, repair count, latency, and final outcome.

**Falsifier:** High recall requires nearly the full catalog, hard-policy exclusions leak into the surface, or quality/repair regressions erase the context benefit.

**Status:** Deterministic functional evidence includes 77.26% fewer selected contract bytes at equal semantic accuracy. Provider-quality and provider-token effects remain unproven.

---

## H7 — Candidate-only repair

**Mechanism:** A bounded repair phase can use read-only evidence, apply a before-hash-guarded patch to ephemeral candidate state, and compare finding deltas without gaining workspace or host commit authority.

**Question:** Does candidate-only repair improve final quality or repair efficiency relative to equivalent non-tool repair while preserving independent evaluation?

**Test:** Compare controlled repair arms with the same initial candidate and findings. Measure fixed, introduced, unchanged, and severity-changed findings; attempts; provider usage; final quality; and stale/scope rejections.

**Falsifier:** Repair introduces critical findings, accepts stale/out-of-scope patches, increases attempts without final benefit, or bypasses the independent Quality Gate.

**Status:** Bounded functional evidence shows finding progress in 30/30 attempted repairs with zero critical findings introduced. General provider-backed benefit remains open.

---

## H8 — Shared visible project state

**Mechanism:** VS Code, browser/Codex UI, and MCP can operate on one revisioned graph runtime while keeping viewer-local state separate.

**Question:** Does a shared visible project representation reduce coordination failures, stale edits, or operator reconstruction cost compared with independent interface state?

**Test:** Run comparable multi-operator tasks with shared runtime synchronization and with isolated interface state. Measure conflicts, stale-command rejection, recovery time, duplicated effects, and human/agent reconciliation work.

**Falsifier:** Shared state does not reduce coordination failures, introduces unacceptable interaction cost, or hides divergence behind eventual snapshot replacement.

**Status:** Bounded one-origin/three-viewer synchronization evidence exists. Comparative coordination benefit and release performance remain open.

---

## H9 — Durable process memory and adaptive revision

**Mechanism:** Runtime events, invocation records, candidates, receipts, finding deltas, and graph fingerprints can form evidence inputs for future Record Keeper process memory.

**Question:** Can durable attempt lineage improve later orchestration or topology revision without turning observation into self-authorizing control?

**Test:** After a durable Record Keeper implementation exists, compare evidence-informed replay/selection with stateless execution. Require explicit provenance, bounded changes, deterministic evaluation, and a fixed control mode.

**Falsifier:** Retrieved history causes recurrence, stale strategy reuse, hidden authority expansion, or no improvement over a stateless control.

**Status:** Planned research. Full process memory, adaptive orchestration, and AVO-style control are not active capabilities.

---

## Evidence tracks must remain separate

| Evidence track | What it can currently support |
|---|---|
| Relation-surface provider experiment | No conclusion yet; eligible A/B pending |
| Visual-host evidence | Bounded synchronization correctness and observed responsiveness |
| Candidate-tool program | Bounded functional and safety behavior |

No track promotes another. In particular, deterministic tool results do not satisfy H1/H2, and visual convergence does not establish a generation-quality benefit.

---

## What this document does not claim

These hypotheses do not establish:

- benchmark superiority or parity with other coding systems;
- a universal law of LLM code generation;
- production or publication readiness;
- novelty without literature review and external validation;
- that any experimental mechanism should be enabled by default;
- that deterministic functional evidence is provider-quality evidence;
- that historical byte-identical scores are valid current evidence;
- that process memory, adaptive orchestration, or AVO is already implemented.

The July correction and active eligibility rules are documented in [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md) and [archive/2026-07-17-benchmark-correction.md](./archive/2026-07-17-benchmark-correction.md).
