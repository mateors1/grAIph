# grAIph Benchmark Specification

## Version 2.0 — Research Preview

**Status:** Active protocol  
**Current result status:** No current provider-backed release conclusion  
**Reviewed:** 2026-08-13

This specification defines how the current experimental mechanisms should be evaluated. It is a protocol, not a leaderboard and not a claim that any arm is better.

---

## 1. Purpose

The benchmark asks whether a named grAIph mechanism changes generation behavior under controlled conditions.

A result is admissible only when the run itself is eligible. A score from an ineligible run is not evidence for the research conclusion, even if the generated files look plausible.

The current target is the relation-surface / binding-contract experiment, with structural contracts treated as a separately attributable lever when enabled.

---

## 2. Non-negotiable controls

### Same source

All arms begin from the same pinned source revision. Record the source commit hash.

### Fresh graph per arm

Each arm independently imports or builds a fresh graph. Do not toggle treatment flags on a graph created under another arm.

The graph's effective flags, source mode, contracts, and graph hash are part of provenance.

### Same generation conditions

Unless a tested lever requires otherwise, hold constant:

- provider backend;
- model identity;
- sampling configuration;
- grAIph/runtime revision;
- source revision;
- workspace and environment;
- near-neighbor source mode;
- near-neighbor context mode;
- number of repetitions;
- scoring and compiler environment.

### Real provider

The benchmark must use a real provider-backed path. Mock providers and unreceipted local placeholders are useful for tests but are not eligible for a provider-backed research conclusion.

Supported provider identities may include API, CLI/SDK, and VS Code Language Model backends. Record the exact identity; copilot-sdk and vscode-lm are different backends.

---

## 3. Arms

The default research-preview comparison is:

| Setting | Baseline | Treatment |
|---|---:|---:|
| enableUnifiedSurface | false | true |
| enableBindingContract | false | true |
| enableStructuralContract | false | true only when structural contracts are the named lever |
| enableProvisionalCardinality | false | false unless separately attributed |
| enableEvidenceRewiring | false | false unless separately attributed |
| enableAstUsageDetection | false | false unless separately attributed |
| enableBenchmarkLogging | true | true |
| enableBenchmarkSourceSuppression | true | true |
| enableDeterminismMode | true | true |
| enableBenchmarkManifest | true | true |
| nearNeighborSourceMode | self-generated | self-generated |
| nearNeighborContextMode | edges | edges |

The baseline and treatment rows are the only intended relation-surface differences. If another setting changes, record it as a separate arm.

---

## 4. Preconditions and provenance

Before generation:

1. Pin and record the target source commit.
2. Select a real provider and record its backend identity and model.
3. Import the source into a new graph for each arm.
4. Capture the effective flag snapshot and graph hash.
5. Enable benchmark manifests and source suppression.
6. Verify that the near-neighbor source is self-generated and that the context arm is declared.
7. Verify that the experiment is running in the intended mode.

A manifest should record, at minimum:

- schema version and artifact ID;
- run start time in UTC;
- graph hash;
- source and runtime commits;
- provider and model;
- effective flags;
- mode and contract versions;
- source mode and integrity arm;
- prompt-leak audit;
- target measurements;
- eligibility and reason codes.

The recommended clean context channel is nearNeighborContextMode=edges, with bodies omitted. Other modes are allowed only when declared and audited as separate context arms.

---

## 5. Candidate and commit eligibility

A generated model response first becomes an unverified candidate.

For a benchmark datum to be eligible:

- required validation must pass;
- host persistence must report committed state;
- the receipt must cover every candidate target;
- committed graph/file state must correspond to the candidate;
- the run must have complete provenance;
- the prompt-leak audit must pass;
- the integrity arm must be declared;
- the provider must be real rather than mock.

Ordinary generation success and benchmark eligibility are separate properties. A candidate that never becomes committed state is not counted as a committed generation.

---

## 6. Repetitions and measurements

Run **N ≥ 5** repetitions per arm into separate output directories. Preserve each run's manifest, logs, candidate/commit receipt, generated output, and score artifacts.

Report:

- mean and spread or standard deviation;
- minimum and maximum;
- per-file similarity and line fidelity;
- surface or contract differences;
- compiler and validation errors;
- identity or import-resolution counts where relevant;
- eligibility status and reason codes for every run.

The primary signal for the current hypothesis is run-to-run spread. Mean quality is secondary. A single high-scoring run does not establish the hypothesis.

Do not reuse historical headline scores as the baseline for v2. Historical results may be discussed only with their provenance status and correction history.

---

## 7. Leak and integrity audit

Before scoring, inspect both the prompt and the output:

- source-suppression manifests must be present;
- any byte-identical output requires positive generation evidence;
- a file written by fallback or recovery without generation evidence is a contamination signal, not a success;
- prompt context must not contain ground-truth source outside an explicitly declared arm;
- missing reference source, missing logs, or missing receipts makes the run ineligible.

Do not repair an ineligible manifest by hand. Rerun with the missing evidence.

---

## 8. Analysis and decision gate

A human owner records one of:

- **accept** — the named hypothesis is supported under the declared threshold;
- **reject** — the named hypothesis is not supported under the declared threshold;
- **rerun** — the run is inconclusive or a protocol issue requires another controlled sample.

The decision record must include the exact arms, provider/model, commits, graph hashes, manifest IDs, effective flags, sample size, metrics, threshold, and eligibility results.

Until that decision is recorded from eligible provider-backed runs:

- experimental defaults remain experimental;
- no benchmark-superiority conclusion is published;
- no historical score is promoted into current evidence.

---

## 9. Historical correction

The July 2026 benchmark correction is preserved in [archive/2026-07-17-benchmark-correction.md](./archive/2026-07-17-benchmark-correction.md).

The previous v1.1 protocol is preserved verbatim in [archive/BENCHMARK_SPEC-v1.1.md](./archive/BENCHMARK_SPEC-v1.1.md). The archive is part of the research record; it is not the active release protocol.
