# Contributing to grAIph

Thank you for taking the time to read or improve the public grAIph materials.

This repository is a public research teaser, architecture record, and methodology record. It does not contain the private implementation workspace.

---

## What is welcome

Issues and pull requests are useful for:

- correcting a factual or link error;
- identifying ambiguous terminology;
- pointing out a missing methodological control;
- suggesting a clearer public explanation;
- adding a relevant citation or prior-work pointer;
- reporting an attempted replication with enough context to inspect it.

Please keep proposed changes scoped to public documentation and research methodology.

---

## Current public boundary

The public repository contains:

- the canonical claim boundary;
- architecture and host/runtime descriptions;
- research hypotheses;
- benchmark and experiment protocols;
- bounded public evidence summaries;
- topology/layout and evidence-reconciliation descriptions;
- citation metadata and historical corrections.

It does not contain:

- private source code;
- raw internal planning or development logs;
- credentials or provider configuration;
- raw private benchmark or host evidence;
- unpublished implementation artifacts.

Do not submit private source or internal-only material in an issue or pull request.

---

## Before opening an issue

Search existing issues and read the maintained boundaries:

- [CURRENT_STATE.md](../CURRENT_STATE.md)
- [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md)
- [RESEARCH_HYPOTHESES.md](../RESEARCH_HYPOTHESES.md)
- [docs/progress-2026-09.md](../docs/progress-2026-09.md)

For a benchmark concern, include the protocol version, evidence track, exact claim, and evidence or reproduction that motivates it.

For a documentation concern, include the affected path and a suggested correction when possible.

---

## Documentation change checklist

Before opening a pull request:

- confirm links resolve within the repository;
- keep verified implementation facts separate from hypotheses;
- identify whether evidence is deterministic, fixture, live local, provider-backed, or production;
- do not use evidence from one track to promote another;
- label historical, failed, blocked, or retracted evidence clearly;
- avoid unverified novelty, superiority, parity, performance, or release-readiness claims;
- preserve the candidate/host-commit distinction;
- record dates when changing the public claim boundary;
- do not include private source, raw logs, credentials, local machine paths, or internal operational ledgers;
- update related documents when a filename, status, or governing boundary changes.

The July 2026 correction remains part of the public record. It should be clarified when necessary, not silently removed.

---

## Evidence-track separation

The public record currently distinguishes:

1. relation-surface and binding-contract provider experiments;
2. host-independent visual synchronization and performance;
3. minimum capability surfaces and candidate tools.

A result from one track cannot satisfy another track's gate. For example, visual convergence does not establish generation quality, and deterministic tool-surface results do not complete the relation-surface provider A/B.

---

## Replications and experiments

The active relation-surface protocol requires a fresh graph per arm, pinned source and runtime revisions, explicit provider/backend identity, complete manifests, prompt-leak auditing, candidate/commit receipts, controlled capability/tooling mode, and at least five repetitions per arm.

A replication may report negative or inconclusive results. Do not present a single successful run as a general conclusion.

See [docs/experiment-protocol.md](../docs/experiment-protocol.md) and [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md).

---

## Review expectations

Pull requests should explain:

1. what changed;
2. why the change is supported;
3. which evidence track and documents are affected;
4. what was checked;
5. whether the change affects a verified property, bounded result, hypothesis, protocol, blocker, or historical record.

The maintainer may ask for a narrower statement or additional evidence. The goal is a public record that remains legible and correct as the implementation evolves.
