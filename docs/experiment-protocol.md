# grAIph Relation-Surface Experiment Protocol

**Status:** Pending fresh provider-backed execution

**Reviewed:** 2026-09-20

This is the operational summary for the current relation-surface, binding-contract, and structural-contract A/B. The root [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md) is the governing protocol.

Visual-runtime synchronization and candidate-tool evidence are separate tracks. They do not satisfy this experiment or unlock its release decision.

---

## Research question

Do explicit relation surfaces, usage bindings, and structural contracts change generation behavior under source suppression, especially run-to-run spread and architectural fidelity?

The experiment is human-gated. No default change or release conclusion follows until eligible provider-backed runs are completed and reviewed.

---

## Arms

Start with the same pinned source revision and build a fresh graph independently for each arm.

| Setting | Baseline | Treatment |
|---|---:|---:|
| enableUnifiedSurface | false | true |
| enableBindingContract | false | true |
| enableStructuralContract | false | named lever only |
| enableProvisionalCardinality | false | false |
| enableEvidenceRewiring | false | false unless separately attributed |
| enableAstUsageDetection | false | false unless separately attributed |
| enableBenchmarkLogging | true | true |
| enableBenchmarkSourceSuppression | true | true |
| enableDeterminismMode | true | true |
| enableBenchmarkManifest | true | true |
| nearNeighborSourceMode | self-generated | self-generated |
| nearNeighborContextMode | edges | edges |

If more than one experimental lever changes, record it as a separate arm. Active candidate-tool or adaptive behavior must remain identical across arms or be disabled; it must not become an undeclared co-treatment.

---

## Preconditions

1. Pin the source commit and record it.
2. Pin the grAIph runtime commit and record it.
3. Select a real provider and record the exact backend identity and model.
4. Import or build a fresh graph for each arm.
5. Confirm effective flags, graph hash, contract version, and capability/tooling mode before generation.
6. Verify that source suppression is active.
7. Verify that near-neighbor source is self-generated.
8. Verify that the context mode is declared.
9. Confirm candidate receipts and benchmark manifest writing are enabled.
10. Confirm that tool, visual-host, or recovery mechanisms cannot introduce undeclared source or change the tested lever.

A mock provider is not eligible for the research conclusion.

---

## Repetitions

Run **N ≥ 5** repetitions per arm into separate output directories.

Preserve, for every repetition:

- source and runtime commits;
- graph hash;
- provider and model;
- effective flags, profile, and mode;
- generation contract and declared capability/tooling mode;
- manifest and prompt-leak audit;
- candidate and commit receipt;
- generated output;
- score and compiler artifacts;
- eligibility reason codes.

Do not combine runs from different graph builds, provider identities, model identities, or effective capability surfaces on one chart.

---

## Measurements

Primary:

- run-to-run spread in similarity, line fidelity, and surface/contract differences.

Secondary:

- mean and standard deviation;
- minimum and maximum;
- compiler and validation errors;
- import/path resolution;
- structural-surface conformance;
- evidence-backed relation accuracy;
- provider-accounted token and context cost;
- repair count and provider usage;
- candidate and commit success.

A single unusually good run is not enough to support the hypothesis.

---

## Integrity and eligibility

Before scoring:

- inspect prompt context for ground-truth source leakage;
- verify every byte-identical output has positive generation evidence;
- classify fallback or recovery copies without generation evidence as contamination;
- confirm the integrity arm and source mode;
- confirm that validation passed and persistence committed;
- confirm that the commit receipt covers candidate targets;
- reject runs with missing provenance, missing reference source, missing receipts, changed co-treatments, or failed leak audits.

Do not repair an ineligible manifest manually. Rerun the experiment with the missing evidence.

---

## Separate evidence tracks

The following do not count as evidence for this relation-surface conclusion:

- deterministic tool-schema or contract-byte reduction;
- candidate-repair progress on a tooling corpus;
- visual-host convergence or latency;
- historical generation scores;
- fixture-only or mock-provider runs;
- a provider result produced under a different graph, capability surface, source mode, or contract version.

They may motivate separate hypotheses, but they cannot be pooled into this decision.

---

## Decision record

A human owner records **accept**, **reject**, or **rerun** with:

- the hypothesis and named lever;
- exact arm settings;
- source and runtime commits;
- provider/model and backend identity;
- graph hashes;
- contract and tooling identities;
- manifest IDs;
- sample size;
- metrics and spread;
- threshold;
- eligibility results;
- known limitations.

Until that record exists, the provider-backed A/B is **pending** and the related release/default gate is **blocked**.

---

## Historical context

The earlier benchmark correction is documented in [archive/2026-07-17-benchmark-correction.md](../archive/2026-07-17-benchmark-correction.md). The preserved v1.1 specification is [archive/BENCHMARK_SPEC-v1.1.md](../archive/BENCHMARK_SPEC-v1.1.md).
