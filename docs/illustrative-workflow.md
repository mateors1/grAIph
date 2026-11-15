# An Illustrative Workflow

**Reviewed:** 2026-11-15

This example explains the implemented workflow. It is not a recorded generation, a shipped sample, or a benchmark result.

Suppose you want a small program that reads a CSV of transaction amounts and produces a summary report. You describe three generation units and their obligations:

| Unit | Required behavior | Dependency |
|---|---|---|
| Parse rows | Validate CSV fields and return an array of typed rows; reject malformed input | None |
| Summarize | Consume those rows and calculate a count and total | Parse rows |
| Format report | Turn the summary into display text | Summarize |

The graph records those relationships explicitly. It can also record the interfaces that cross each edge, such as the row fields or summary shape.

## 1. Check the architecture

The engine checks the supported graph/API contracts and derives a dependency order. For a saved imported project, source-bound evidence can help reconcile imports and ports. Optional model-based behavioral review is a separate action.

If the parser is allowed to return `null` while the summary requires an array, the intended contract needs clarification. A graph-repair proposal must be revalidated and accepted by the host; a code-repair attempt cannot silently authorize new requirements.

## 2. Compile context and generate candidates

For the summary unit, grAIph compiles the task, relevant parser interface, graph contracts, source-access policy, and any explicitly enabled tool surface into generation context. This is the role of [Topological Context Compilation](./tcc-primer.md).

The default route sequences planning, synthesis, QA/repair, conformance, and gating. A candidate is proposed code with findings and identity. Deterministic sequencing does not make the model's output deterministic.

## 3. Validate and repair within bounds

Assume a candidate violates the declared array contract and compilation reports an owned type error. Where compiler repair is enabled, the engine can send the retained candidate and diagnostics into a bounded repair, keeping the original invocation accounting. The corrected result must pass the applicable checks again. Cyclic dependency groups require joint validation before acceptance.

Missing compiler dependencies produce unavailable evidence rather than a guessed source patch. Dependency-environment preparation and project-file scaffolding have separate outcomes. See [Saved-graph regeneration](./saved-graph-regeneration.md).

## 4. Accept through the host

For the required checks in this example, a passing candidate proceeds to host acceptance. The authoritative host persists the accepted graph/file state and supplies a receipt covering it. A model saying "done," a successful tool call, or a stream of progress events does not establish that commit.

Connected viewers observe the resulting revision. Selection, zoom, and wire-color presentation remain viewer-local. A stale edit is rejected rather than overwriting a newer revision.

## 5. Assess project readiness separately

A committed unit is not automatically a working application. Native project QA uses host-registered independent checks, then integration and reopen evidence where required. The current implementation has explicit ownership and platform limits; see [Controlled adaptation and project QA](./controlled-adaptation.md).

The research question is whether this explicit structure improves outcomes over a controlled alternative. This example explains the mechanism; the [evidence guide](./evidence-guide.md) explains what has actually been checked.
