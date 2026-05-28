# grAIph

**Topological Context Compilation for LLM Code Generation**

> LLMs don't lack the capacity. They lack the structure.

---

## What grAIph Is

grAIph is a VS Code extension and code synthesis pipeline implementing **Topological
Context Compilation (TCC)** — a novel architecture that applies compiler theory to
LLM context window management.

The graph is the source language. The pipeline compiles it into production code.

---

## The Result

A 2.3 billion parameter model (Q4 quantized, fits in 2GB RAM) governed by grAIph
scores within 0.1 points of frontier models on a 37-file TypeScript benchmark.

The same model scored **2.8/10 before pipeline improvements** and **7.7/10 after**
with zero model change.

Pipeline infrastructure is the dominant quality variable. Not model scale.

---

## Three Pillars

### 1. Topological Context Compilation
grAIph treats a codebase as a dependency graph. An exponential decay formula
determines exactly what context each node needs during generation — the further
a dependency is in graph hops, the less information gets attached. No noise.
No drift. Kahn's algorithm dictates generation order, ensuring each file generates
only after its imports are ready, maximizing context window utility.

### 2. Quality Accountability Pipeline
Seven specialized agents catch and route errors to who should fix them — static
tooling or LLM agents. Converts stochastic LLM output into verifiable,
compilable code. Runs after generation, before output.

### 3. Visual IDE
A node-based canvas where developers work with architecture instead of syntax.
The graph is the source of truth. Bidirectional — the pipeline compiles graphs
into code, and imports existing codebases back into graphs.

---

## Why It Works: Spatial Physics

TCC is not just a compiler analogy — it obeys the same mathematical laws as
physical attenuation.

Context relevance decays exponentially with graph distance. Kahn wave ordering
propagates generation in dependency order — the same way neurons fire downstream
only after upstream signals arrive. The topology is geometry and gravity: nodes
cluster by semantic weight, edges carry influence that attenuates with distance.

The exponential decay formula `C(v) = α · e^{-β · d(v₀, v)}` is not a heuristic.
It is the mathematical form of physical signal attenuation applied to a new substrate.

LLM code generation obeys spatial physics. grAIph is designed accordingly.

---

## Model Agnostic

Tested on:
- GitHub Copilot SDK
- Anthropic API + Anthropic-compatible endpoints
- OpenAI-compatible endpoints
- Google Gemini SDK
- NVIDIA NIM free tier
- LM Studio (local models via llama.cpp)

Works with models as small as **Gemma 4 E2B (2.3B effective parameters)**.

---

## Benchmark History

23 runs across 6 models. Documented methodology. Reproducible results.

| Run | Model | Score | Notes |
|-----|-------|-------|-------|
| V-Poisoned-SF | GPT-4o | 100% byte-perfect | Pipeline determinism ceiling. External machine (SF). |
| V15 | Unknown frontier | 8.1/10 | First 4/4 hub byte-identical run |
| V22 | Gemma 4 E2B (2.3B Q4) | 8.1/10 adj | Ties frontier. Zero ENGINE-INJECT. Single-model pipeline. |
| V23 | Flash Lite | 7.7/10 | Was 2.8/10 in V10. +4.9pts from pipeline alone. |
| V10 | Flash Lite | 2.8/10 | Pre-pipeline-fixes baseline |

Full methodology: see [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md)

---

## Architecture & Research

This repository contains the full methodology and architecture documentation:

- [BENCHMARK_SPEC.md](./BENCHMARK_SPEC.md) — Versioned benchmark methodology v1.0
- [TOPOLOGY_LAYOUT.md](./TOPOLOGY_LAYOUT.md) — Topology layout algorithm specification
- [ACADEMIC_CONTRIBUTIONS.md](./ACADEMIC_CONTRIBUTIONS.md) — Publication map (9 contributions)
- [docs/tcc-primer.md](./docs/tcc-primer.md) — Topological Context Compilation explained
- [docs/pipeline-overview.md](./docs/pipeline-overview.md) — Architecture overview

---

## Status

- VS Code extension — private repo, pre-grant
- NLnet NGI Zero Commons Fund application submitted May 2026, decision pending
- Open core release after grant milestone 1

**Pipeline source opens at the open core release milestone.**
Watch this repo for updates.

---

## Research Contributions

grAIph has produced several publishable findings:

**Tier 1 (directly publishable):**
- Topological Context Compilation — novel compilation architecture (MSR/ICSE target)
- Context Decay in Graph-Structured Generation — `C(v) = α · e^{-β · d(v₀, v)}`, β=0.7 (EMNLP/ACL target)
- Two-Track Benchmark Methodology with AssertionHintContract
- Graph Diameter Threshold for Clustering Activation
- Rendon's Geometric Expansion (Kahn + gravitational Y-relaxation)

Full catalogue: [ACADEMIC_CONTRIBUTIONS.md](./ACADEMIC_CONTRIBUTIONS.md)

---

## Why This Matters

Current LLM code generation tools (Cursor, Copilot, LangChain) either rely on
model brute force or route prompts without structural guarantees. grAIph's
hypothesis: **LLMs don't lack capability — they lack structure.**

The benchmark results validate this. A 2.3B model with structure outperforms
larger models without it. The architecture layer contributes materially to
capability. This is a research claim with empirical evidence.

---

## Contact

**hello@graiph.dev**
[graiph.dev](https://graiph.dev)

Built by Mateo Rendon Suarez — Bogotá, Colombia.
NLnet grant application submitted May 2026. O-1A visa path in progress.

---

## License

Apache 2.0 — see [LICENSE](./LICENSE)

Pipeline source releases under the same license at open core milestone.
