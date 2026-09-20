# September 2026 Progress and Limitations

**Reviewed:** 2026-09-20

This note summarizes public-safe bounded evidence from the private development workspace. It is not a release announcement, benchmark leaderboard, or substitute for the active provider-backed relation-surface experiment.

Historical failed and blocked runs remain part of the private evidence record. The results below describe the latest reviewed evidence for the named scope; they do not retroactively convert older failures into passes.

---

## Host-independent visual runtime

The visual architecture now uses:

- a versioned host-independent protocol;
- an authoritative revisioned graph/session runtime;
- a host-independent graph client;
- an authenticated loopback browser host;
- the shared UI in VS Code, standalone Chrome, and the Codex browser;
- MCP integration against the same runtime.

### Latest exact-build run

The latest reviewed run used actual Codex-browser Add Node actions while native VS Code and standalone Chrome were attached.

| Property | Result |
|---|---|
| Initial/final graph size | 500 → 535 nodes |
| Warmups / measured observations | 5 / 30 |
| Total actual edits | 35 |
| Revision/hash/receipt matches | 35/35 |
| Three-viewer layout matches | 35/35 |
| Native viewport failures | 0 |
| Live provider generation | Not exercised |

Receipt-to-layout timing:

| Viewer | Median | p95 | Minimum / maximum |
|---|---:|---:|---:|
| Native VS Code | 672 ms | 1266 ms | 544 / 1917 ms |
| Standalone Chrome | 776 ms | 1414 ms | 633 / 2014 ms |
| Codex browser | 820 ms | 1506 ms | 683 / 2045 ms |

All three p95 values fail the unchanged 250 ms target.

### What the run establishes

- One real edit origin converged across three real viewers.
- Durable revision, document hash, receipt, and rendered layout agreed.
- Viewer-local viewport state remained local.
- Browser-control interruption recovery did not duplicate an edit or delete a recorded sample.

### What the run does not establish

- the complete four-origin/twelve-cell final-build matrix;
- fixed-size comparative performance;
- pure transport or physical presentation latency;
- live provider-generation behavior;
- steady-state memory or leak freedom;
- long-soak reliability;
- release readiness.

---

## Minimum tool surface

The completed tool program is organized into:

1. trustworthy execution and durable invocation;
2. semantic capability metadata and minimum-surface planning;
3. read-only evidence, candidate-only repair, and brokered expansion.

Public-safe functional results:

| Check | Result |
|---|---:|
| Read-only semantic observations | 54/54 passed |
| Selected contract-byte reduction | 77.26% |
| Candidate repairs with finding progress | 30/30 |
| Critical findings introduced by repair | 0 |
| Normal tasks requiring expansion | 3/24 (12.5%) |
| Useful distinct grants | 15/18 (83.33%) |
| Final expansion schedule | 117/117 rows passed |
| Provider calls in final expansion schedule | 0 |

### Qualification

The tool program completed under amended functional criteria. The reduction is measured in selected contract bytes, not provider tokens or cost. The final expansion schedule is deterministic and provider-free.

The results do not establish:

- provider-quality improvement;
- causal improvement over a no-tool baseline;
- production expansion frequency;
- lower end-to-end latency;
- unrestricted or host-authoritative tool use.

---

## Separate evidence tracks

Three evidence tracks must remain separate:

| Track | Current public status |
|---|---|
| Relation surfaces, bindings, and structural contracts | Provider-backed A/B pending; release conclusion blocked |
| Host-independent visual runtime | Bounded synchronization correctness passed; responsiveness and release qualification blocked |
| Minimum capability surfaces and candidate tools | Bounded functional program complete; provider-quality effect unproven |

Success in one track does not promote another.

---

## Next evidence needed

### Visual runtime

- Calibrate separate budgets for local interaction and remote committed-change display.
- Run a final-build fixed-size all-origin/viewer matrix.
- Establish a steady-state memory baseline and long soak.
- Retain a clean release artifact and receipt.

### Candidate tools

- Compare active minimum surfaces against a controlled provider-backed baseline.
- Attribute first-pass quality, repair efficiency, context, latency, and provider usage separately.
- Preserve hard-policy exclusion and host/candidate authority boundaries.

### Relation-surface experiment

- Execute the existing source-sealed N≥5 arms with a real provider.
- Review manifests, leak audits, receipts, metrics, and spread.
- Record accept, reject, or rerun without importing conclusions from the other tracks.

See [Current state](../CURRENT_STATE.md), [Benchmark specification](../BENCHMARK_SPEC.md), and [Experiment protocol](./experiment-protocol.md).
