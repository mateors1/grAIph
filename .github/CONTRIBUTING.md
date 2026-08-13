# Contributing to grAIph

Thank you for taking the time to read or improve the public grAIph materials.

This repository is a public research teaser and methodology record. It does not contain the private implementation workspace.

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

The public repository contains documentation, hypotheses, benchmark protocol, topology/layout description, evidence-reconciliation description, citation metadata, and historical corrections.

It does not contain:

- private source code;
- raw internal planning or development logs;
- credentials or provider configuration;
- unreviewed benchmark outputs;
- unpublished implementation artifacts.

Do not submit private source or internal-only material in an issue or pull request.

---

## Before opening an issue

Search existing issues and read the current claim boundary:

- [CURRENT_STATE.md](../CURRENT_STATE.md)
- [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md)
- [RESEARCH_HYPOTHESES.md](../RESEARCH_HYPOTHESES.md)

For a benchmark concern, include the protocol version, the exact claim being questioned, and the evidence or reproduction that motivates it.

For a documentation concern, include the affected path and a suggested correction when possible.

---

## Documentation change checklist

Before opening a pull request:

- confirm links resolve within the repository;
- keep verified implementation facts separate from hypotheses;
- label historical or retracted evidence clearly;
- avoid unverified novelty, superiority, parity, or publication-readiness claims;
- record dates when changing the public claim boundary;
- do not include raw internal planning material;
- update related documents when a filename or status changes.

The July 2026 correction remains part of the public record. It should be clarified when necessary, not silently removed.

---

## Replications and experiments

The active protocol requires a fresh graph per arm, pinned source and runtime revisions, explicit provider identity, complete manifests, prompt-leak auditing, candidate/commit receipts, and at least five repetitions per arm.

A replication may report negative or inconclusive results. Do not present a single successful run as a general conclusion.

See [docs/experiment-protocol.md](../docs/experiment-protocol.md) and [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md).

---

## Review expectations

Pull requests should explain:

1. what changed;
2. why the change is supported;
3. which documents are affected;
4. what was checked;
5. whether the change affects a claim, hypothesis, protocol, or historical record.

The maintainer may ask for a narrower statement or additional evidence. The goal is a public record that remains legible and correct as the implementation evolves.
