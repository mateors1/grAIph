# grAIph Pipeline Overview

## Architecture

grAIph is structured as a monorepo with four packages:

```
@graiph/shared      — types, enums, messaging, feature flags (leaf package)
@graiph/core        — agents, orchestrator, pipeline, analysis
@graiph/vscode-ext  — VS Code extension host, IPC bridge  
@graiph/webview-ui  — React + XYFlow canvas
```

---

## The Seven Agents

The pipeline runs seven specialized agents per node in topological order:

| Agent | Role | Notes |
|-------|------|-------|
| **TopologyScrutineer** | Validates graph coherence, detects cycles, computes wave assignments | Runs Tarjan's SCC before Kahn's |
| **PatternSpecialist** | Identifies architectural patterns, applies structural contracts | Per-project SQLite skill cache |
| **TaskDecomposer** | Decomposes generation into atomic tasks with contracts | Produces markdown manifest |
| **Coder** | Generates code from task manifest + pruned context | The only stochastic step |
| **QASquire** | Compile verification, syntax checking, assertion hints | tsc --noEmit + language strategies |
| **ConformanceAgent** | Validates against declared contracts and graph spec | Catches spec-level violations |
| **QualityGate** | Final pass/fail decision, triggers repair loop | Binary decision with escalation |

**The LLM touches only the Coder step.** All other agents use static analysis,
deterministic algorithms, or structured verification. The stochastic component
is isolated and bounded.

---

## The Two Execution Paths

**PATH B (default):** Sequential multi-call pipeline. Every provider routes through
this path by default. Validated across 23 benchmark runs. This is the scientifically
grounded path.

**PATH A (opt-in):** Native orchestration for providers that support it.
Requires explicit `enableNativeOrchestration: true`. Not the default.

---

## Context Assembly (ScopePruning)

Before the Coder runs, ScopePruning assembles the context window:

```typescript
// Exponential decay by graph distance
C(v) = α · e^{-β · d(v₀, v)}  // β=0.7, α=1.0

// Three-tier degradation
d=0:  full content
d=1:  interface contracts + generated files  
d=2:  symbol + return type
d=3+: registry identity only
```

The context window contains exactly what the target node needs.
No more. No less.

---

## The Benchmark Framework

grAIph includes a rigorous two-track benchmark methodology:

**Track A (pure generation):** Graph-only. No source code. Measures what
the model achieves from specification alone.

**Track B (assisted generation):** Graph + sealed metadata hints to reviewing
agents only. The Coder never sees source. Measures the contribution of
informed reviewing to generation quality.

The delta between Track A and Track B isolates the reviewing contribution.

Full specification: [BENCHMARK_SPEC.md](../BENCHMARK_SPEC.md)

---

## Supported Providers

grAIph is model-agnostic. Current provider implementations:

- GitHub Copilot SDK
- Anthropic API
- Anthropic-compatible endpoints
- OpenAI-compatible endpoints  
- Google Gemini SDK

Tested on: NVIDIA NIM free tier, LM Studio (llama.cpp), Claude, GPT-4.1,
Gemini Flash Lite, Gemma 4 E2B, GLM-5.1.

---

## Key Design Decisions

**Determinism over improvisation.** Generation follows the pipeline:
Graph → Topology → Pattern → Decompose → Generate → QA → Conform → Gate.
Never skip steps.

**Language agnostic.** Target language configured per node via
`editionFace.targetLanguage`. No TypeScript assumptions in core logic.

**Pipeline is the bottleneck, not the model.** Empirically validated:
same model, pipeline improvements, +4.9 points. Architecture layer
contributes materially to capability.

---

## Status

Pipeline source is not yet public. Open core release is Task 1 of the
funded NLnet grant period.

Architecture documentation, benchmark methodology, and algorithm
specifications are fully public in this repository.

*hello@graiph.dev — graiph.dev*
