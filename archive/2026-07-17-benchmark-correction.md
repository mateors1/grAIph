# Benchmark Correction — 2026-07-17

This document preserves the public correction record for grAIph's earlier benchmark claims.

---

## What was found

A forensic audit completed on 2026-07-16 examined the byte-identical output files in the available benchmark history.

When codebase import seeded a node with original source content and generation later produced no output, a fallback or batch-repair path could flush the seeded original bytes into the output directory. The resulting file looked like a successful reproduction even though the model had not generated it.

In every auditable run in the V33–V50 range, the byte-identical files examined by the audit correlated with a generation event reporting no generated files. The output timestamps also matched the recovery path. These were fallback copies, not model generations.

Earlier runs did not retain enough generation evidence to be retro-audited. Their byte-identical counts were therefore retracted as unverifiable.

---

## Impact

The following were retracted or placed under re-evaluation:

- byte-identical file counts;
- byte-perfect or determinism-ceiling statements derived from those counts;
- benchmark claims that treated those counts as generation fidelity;
- composite scores whose weighting depended materially on the affected metric;
- the historical 2.8-to-7.7 comparison as a current quantitative result.

The correction did not establish that every non-byte metric was invalid. Compile, intent, and semantic dimensions still require their own provenance and eligibility checks, and the current v2 protocol does not reuse the old headline scores as release evidence.

---

## Changes made in response

The benchmark methodology was changed to require:

- output provenance checks;
- positive generation evidence for byte-identical files;
- prompt-leak auditing;
- retained generation event logs;
- explicit source-suppression and integrity-arm declarations;
- immutable run manifests;
- candidate-to-commit receipts;
- benchmark eligibility separate from ordinary generation success;
- human review before a release or default decision.

The v1.1 specification that introduced the mandatory leak-detection precondition is preserved in [BENCHMARK_SPEC-v1.1.md](./BENCHMARK_SPEC-v1.1.md). The active research-preview protocol is [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md).

---

## Why this remains public

The correction is part of the project's research record. It explains why the current repository distinguishes implementation properties, experimental mechanisms, historical evidence, and eligible current results.

The current status is conservative by design:

> The architecture is implemented and evolving; the fresh provider-backed A/B is still pending; no current benchmark-superiority claim is being made.
