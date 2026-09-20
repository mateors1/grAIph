# grAIph Benchmark Specification

## Version 2.1 — Research Preview

**Status:** Active protocol

**Current result status:** No current provider-backed release conclusion

**Reviewed:** 2026-09-20

This specification defines how the current experimental generation mechanisms should be evaluated. It is a protocol, not a leaderboard and not a claim that any arm is better.

---

## 1. Purpose

The benchmark asks whether a named grAIph mechanism changes generation behavior under controlled conditions.

A result is admissible only when the run itself is eligible. A score from an ineligible run is not evidence for the research conclusion, even if the generated files look plausible.

The current target is the relation-surface / binding-contract experiment, with structural contracts treated as a separately attributable lever when enabled.

Other evidence programs—such as host-independent visual synchronization and candidate-tool evaluation—have their own scopes and gates. Their results must not be pooled into the relation-surface conclusion.

---

## 2. Evidence-track separation

| Track | Governing question | Current status |
|---|---|---|
| Relation surfaces and bindings | Do explicit graph contracts change provider-backed generation behavior? | Eligible A/B pending |
| Visual runtime | Do origins and viewers converge on one authoritative graph, and at what responsiveness cost? | Bounded correctness evidence; release qualification blocked |
| Candidate tools | Do minimum surfaces, candidate repair, and brokered expansion behave safely and improve provider outcomes? | Bounded functional evidence; provider-quality effect open |

An implementation or result in one track does not promote another. A public statement must identify its track, evidence type, and limitations.

---

## 3. Non-negotiable controls

### Same source

All arms begin from the same pinned source revision. Record the source commit hash.

### Fresh graph per arm

Each arm independently imports or builds a fresh graph. Do not toggle treatment flags on a graph created under another arm.

The graph's effective flags, source mode, contracts, capability surface, and graph hash are part of provenance.

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
- generation profile and candidate-tool mode;
- number of repetitions;
- scoring and compiler environment.

### Real provider

The benchmark must use a real provider-backed path. Mock providers and unreceipted local placeholders are useful for tests but are not eligible for a provider-backed research conclusion.

API, CLI/SDK, subscription transport, and VS Code Language Model routes are distinct backends. Record the exact route and identity rather than treating a similar model label as equivalence.

---

## 4. Arms

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

Active capability planning, candidate tools, repair policy, or expansion policy must be identical between arms or explicitly disabled. They are not permitted undeclared co-treatments.

---

## 5. Preconditions and provenance

Before generation:

1. Pin and record the target source commit.
2. Pin and record the grAIph/runtime commit.
3. Select a real provider and record its backend identity and model.
4. Import the source into a new graph for each arm.
5. Capture the effective flag snapshot, graph hash, profile, and capability/tooling mode.
6. Enable benchmark manifests and source suppression.
7. Verify that near-neighbor source is self-generated and the context arm is declared.
8. Verify that no recovery, tool, or host path can introduce undeclared source.

A manifest should record, at minimum:

- schema version and artifact ID;
- run start time in UTC;
- graph hash;
- source and runtime commits;
- provider, backend, and model;
- effective flags and mode;
- contract, capability, and policy identity;
- source mode and integrity arm;
- prompt-leak audit;
- target measurements;
- candidate and commit receipt identity;
- eligibility and reason codes.

The recommended clean context channel is `nearNeighborContextMode=edges`, with bodies omitted. Other modes are allowed only when declared and audited as separate context arms.

---

## 6. Candidate and commit eligibility

A generated model response first becomes an unverified candidate.

For a benchmark datum to be eligible:

- required validation must pass;
- the independent Quality Gate must pass;
- host persistence must report committed state;
- the canonical receipt must cover every accepted candidate target;
- committed graph/file state must correspond to the candidate;
- the run must have complete provenance;
- the prompt-leak audit must pass;
- the integrity arm must be declared;
- the provider must be real rather than mock;
- no undeclared co-treatment may differ between arms.

Ordinary generation success and benchmark eligibility are separate properties. A candidate that never becomes committed state is not counted as a committed generation.

---

## 7. Repetitions and measurements

Run **N ≥ 5** repetitions per arm into separate output directories. Preserve each run's manifest, logs, candidate/commit receipt, generated output, and score artifacts.

Report:

- mean and spread or standard deviation;
- minimum and maximum;
- per-file similarity and line fidelity;
- surface or contract differences;
- compiler and validation errors;
- identity or import-resolution counts where relevant;
- provider-accounted tokens and provider usage;
- repair counts and final quality state;
- eligibility status and reason codes for every run.

The primary signal for the current hypothesis is run-to-run spread. Mean quality is secondary. A single high-scoring run does not establish the hypothesis.

Do not reuse historical headline scores as the baseline for v2. Historical results may be discussed only with their provenance status and correction history.

---

## 8. Leak and integrity audit

Before scoring, inspect both prompt and output:

- source-suppression manifests must be present;
- any byte-identical output requires positive generation evidence;
- a file written by fallback or recovery without generation evidence is contamination, not success;
- prompt context must not contain ground-truth source outside an explicitly declared arm;
- tool or evidence routes must not provide undeclared source;
- missing reference source, logs, manifests, or receipts makes the run ineligible.

Do not repair an ineligible manifest by hand. Rerun with the missing evidence.

---

## 9. Analysis and decision gate

A human owner records one of:

- **accept** — the named hypothesis is supported under the declared threshold;
- **reject** — the named hypothesis is not supported under the declared threshold;
- **rerun** — the run is inconclusive or a protocol issue requires another controlled sample.

The decision record must include exact arms, provider/model/backend, commits, graph hashes, contract and capability identities, manifest IDs, effective flags, sample size, metrics, threshold, and eligibility results.

Until that decision is recorded from eligible provider-backed runs:

- experimental defaults remain experimental;
- no benchmark-superiority conclusion is published;
- no visual-host or tooling result is substituted for the experiment;
- no historical score is promoted into current evidence.

---

## 10. Historical correction

The July 2026 benchmark correction is preserved in [archive/2026-07-17-benchmark-correction.md](./archive/2026-07-17-benchmark-correction.md).

The previous v1.1 protocol is preserved verbatim in [archive/BENCHMARK_SPEC-v1.1.md](./archive/BENCHMARK_SPEC-v1.1.md). The archive is part of the research record; it is not the active release protocol.
