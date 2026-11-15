# grAIph

**A graph-first software environment with bounded AI generation and host-controlled commits.**

grAIph represents software architecture as shared graph state: generation units, dependencies, interfaces, constraints, and evidence. The engine uses that graph to choose generation order, compile relevant context, validate candidates, and reconcile accepted changes.

Its central systems question is: **what becomes easier to control and measure when architecture is explicit, rather than reconstructed independently by each model, tool, and interface?**

**Research preview · documentation reviewed November 15, 2026.** The implementation remains private; this repository contains documentation, evidence summaries, hypotheses, and protocols. Latest retained macOS checks passed with classified skips, and scoped live compiler-repair cases now exist. The tested follow-up changes are still uncommitted in the reviewed development tree. Production and cross-platform qualification remain incomplete. The latest retained verification receipt is dated November 14 UTC (November 13 in Bogotá).

## Start here

| If you want to… | Read |
|---|---|
| Understand one concrete task | [Illustrative workflow](./docs/illustrative-workflow.md) |
| Check availability or reproduction requirements | [Getting started](./docs/getting-started.md) |
| Assess the strength and limits of the evidence | [Evidence guide](./docs/evidence-guide.md) |
| Inspect the architecture and current claims | [Pipeline overview](./docs/pipeline-overview.md) · [Current state](./CURRENT_STATE.md) |

The [July benchmark correction](./archive/2026-07-17-benchmark-correction.md) remains part of the project record. Retracted byte-identical results are not current evidence.

## A concrete workflow

Imagine a program that parses CSV rows, calculates totals, and formats a report. Its graph records which unit provides the row shape, which consumes it, and what each output must satisfy.

1. Validate the supported graph/API contracts and derive dependency order.
2. Compile the target's requirements, relevant neighboring interfaces, source policy, and permitted tools into generation context.
3. Generate detached candidate code, run applicable QA and conformance checks, and repair within the invocation's limits.
4. Accept through the authoritative host, which persists graph/file state and returns a covering receipt.

A compiler rejection can return an owned candidate to bounded repair; missing validation dependencies remain unavailable evidence. A model response or successful tool call is not itself a committed change. The [worked explanation](./docs/illustrative-workflow.md) is illustrative, not a recorded benchmark.

## How the system fits together

~~~text
Shared graph + requirements
            ↓
Dependency order + context + opt-in bounded capabilities
            ↓
Candidate synthesis → QA/repair → conformance → gate
            ↓
Graph reconciliation + authoritative host persistence
            ↓
Committed graph/file state + receipt
~~~

The default route uses deterministic stage sequencing. Model output can still vary. The shared tool executor, candidate leases, capability expansion, and adaptive controller default off. Their controls apply to explicitly enabled routes and scoped qualification runs. The adaptive controller requires a registered independent evaluator and retains the same host acceptance boundary.

The private development implementation includes:

- **Shared project state:** VS Code, authenticated local browser viewers, and MCP operators attach to one revisioned runtime. Selection and viewport state remain local.
- **Graph-informed context:** Topological Context Compilation derives ordering and context representations from dependencies, surfaces, and contracts.
- **Candidate-first generation:** synthesis and repair operate on proposed state; independent gates and the host determine acceptance.
- **Saved-graph regeneration:** source-bound dependency/API evidence and portable dependency requirements support later admission and independent Node QA preparation.
- **Attempt history and project QA:** retained candidates/evaluations support later repair; project readiness requires its own trusted checks and integration/reopen evidence.

See [Pipeline overview](./docs/pipeline-overview.md), [Saved-graph regeneration](./docs/saved-graph-regeneration.md), and [Controlled adaptation](./docs/controlled-adaptation.md).

## What the evidence supports

| Evidence level | Current position |
|---|---|
| Implemented mechanisms | Graph/runtime, candidate pipeline, opt-in bounded capabilities/adaptation, attempt history, and host-controlled finalization exist in development code |
| Scoped verified behavior | Retained macOS build/type/test checks, transaction and repair fixtures, and limited installed-settings/viewer checks passed within their declared scope |
| Preliminary observations | Natural and controlled live compiler repairs were accepted; an application campaign completed over three batches with intervening engine fixes |
| Proposed experiments | The fresh relation-surface/binding-contract A/B and general quality/efficiency benefits remain unestablished |

The latest passing tests cover an uncommitted source cut. The live campaign retained rejected attempts, used a corrected-source/non-clean arm, and has remaining differential gaps. It is not an unchanged single-build end-to-end result or a model ranking. The 250 ms visual responsiveness target has no newer passing qualification.

The [evidence guide](./docs/evidence-guide.md) explains these limits and the skip categories. [Current state](./CURRENT_STATE.md) governs all public claims. grAIph does not claim production readiness, general benchmark superiority, or demonstrated speed/cost advantages.

## Research questions

The active controlled experiment asks whether explicit relation surfaces, usage bindings, and structural contracts change architectural fidelity or run-to-run spread under source suppression.

Eligible arms require a pinned source/runtime, a fresh graph per arm, the same declared provider/backend and settings, complete provenance and leak audits, and at least five repetitions per arm. The experiment remains pending; implementation and local test results do not answer it.

Other hypotheses concern context allocation, reconciliation, candidate repair, and evidence-informed selection. Each needs its own control and decision criteria. See [Research hypotheses](./RESEARCH_HYPOTHESES.md), [Benchmark specification](./BENCHMARK_SPEC.md), and [Experiment protocol](./docs/experiment-protocol.md).

## Availability and documentation

This checkout is a documentation preview, not an engine distribution. It provides no verified public install command, runnable sample, or hosted editor. See [Getting started](./docs/getting-started.md) for what can be inspected now and what independent reproduction would require.

Further architecture references:

- [Topological Context Compilation](./docs/tcc-primer.md)
- [Host-independent visual runtime](./docs/visual-runtime.md)
- [Bounded tool runtime](./docs/tool-runtime.md)
- [Runtime observability](./docs/runtime-observability.md)
- [Evidence reconciliation](./docs/evidence-reconciliation.md)
- [Topology-derived layout](./TOPOLOGY_LAYOUT.md)
- [November progress](./docs/progress-2026-11.md) and historical [September evidence](./docs/progress-2026-09.md)
- [Contributing](./.github/CONTRIBUTING.md) and [citation metadata](./CITATION.cff)

## Contact and license

Mateo Rendon Suarez · Bogotá, Colombia · [hello@graiph.dev](mailto:hello@graiph.dev) · [graiph.dev](https://graiph.dev)

Apache 2.0 — see [LICENSE](./LICENSE).
