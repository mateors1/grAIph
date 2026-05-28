# Contributing to grAIph

Thank you for your interest in grAIph.

This repository currently contains **methodology documentation only** — the benchmark
specification, architecture overview, topology algorithm, and academic contributions
map for the grAIph pipeline.

---

## Current Status

The pipeline source code is **not yet public**.

grAIph is in pre-grant development. The NLnet NGI Zero Commons Fund application
was submitted May 2026. Open core release is tied to **grant milestone 1** — the
pipeline architecture refactor and benchmark validation deliverable.

When the source opens, this guide will be updated with real contribution instructions.

---

## What You Can Do Right Now

### Read and cite the methodology

All architecture documentation is public and citable:

- [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md) — Two-track benchmark methodology v1.0
- [docs/tcc-primer.md](tcc-primer.md) — Topological Context Compilation explained
- [ACADEMIC_CONTRIBUTIONS.md](../ACADEMIC_CONTRIBUTIONS.md) — Publication map
- [TOPOLOGY_LAYOUT.md](../TOPOLOGY_LAYOUT.md) — Layout algorithm specification

Academic citation: see [CITATION.cff](../CITATION.cff) at the repo root.

### Report issues with the methodology

If you find errors, gaps, or inconsistencies in the documented methodology, open
an issue. Label it `methodology`.

Specifically useful:
- Errors in the benchmark specification
- Missing prior work citations (if you know of related work on context-window
  management using graph structure, please open an issue)
- Inconsistencies between documents

### Replicate the benchmark

The benchmark methodology is fully documented in [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md).
If you replicate any aspect of the benchmark with your own models or infrastructure,
open an issue with your results. Replication is the foundation of the research claims.

### Watch for open core release

Star or watch this repository to be notified when the pipeline source opens.
The release will happen as a single coherent push at milestone 1 delivery,
not as an incremental trickle.

---

## What To Expect at Open Core Release

When the source opens:

- Full monorepo: `@graiph/shared`, `@graiph/core`, `@graiph/vscode-ext`, `@graiph/webview-ui`
- Seven-agent pipeline source
- Benchmark harness
- Real contribution guide with setup instructions, architecture decisions, and PR process

The pipeline is TypeScript throughout. VS Code extension development experience
is useful but not required for core pipeline contributions.

---

## Contact

Questions about the methodology, research directions, or collaboration:

**hello@graiph.dev**

For potential research collaboration on the academic contributions (particularly
the context decay paper and benchmark methodology paper), direct email is preferred
over GitHub issues.

---

## Code of Conduct

grAIph is a research project. Contributions are expected to be technically rigorous,
honest about uncertainty, and respectful of the work's empirical grounding.

The benchmark results are what they are. The methodology is documented precisely
so they can be reproduced, challenged, and extended — not defended.

---

*grAIph — graiph.dev*
