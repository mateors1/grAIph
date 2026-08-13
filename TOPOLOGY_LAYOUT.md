# grAIph Topology-Derived Layout

**Status:** Implemented behind explicit experimental controls  
**Reviewed:** 2026-08-13

This document describes a deterministic layout implementation derived from graph topology. It does not claim that visual layout improves model quality, that layout is a physical law, or that the current policy is optimal.

---

## Why layout is part of the architecture

A generation system must make structural decisions such as:

1. where a generated file or module is placed; and
2. how related units are arranged for human inspection and downstream path decisions.

If those decisions are left entirely to a stochastic model, equivalent graph inputs can produce inconsistent placement and import paths.

The graph already contains useful structure: dependency waves, cross-wave edges, hubs, bridges, and isolated nodes. A deterministic layout policy can use those inputs before generation.

---

## Current controls

The implementation is guarded by explicit settings:

- enableTopologyLayout — enables the topology-derived strategy;
- topologyLayoutLogOnly — records layout decisions without applying them;
- layoutStrategy — selects the strategy; the current default remains grid;
- enablePrecomputedLayout — controls precomputed layout use where applicable.

Topology-derived layout is not the default. The capability ledger records the current defaults and review date.

---

## Algorithm

The implementation is organized as deterministic phases.

### Phase 1 — Wave assignment

Use the graph's topological generation waves. The wave index supplies the primary horizontal coordinate:

~~~text
x(node) = wave index
~~~

Strongly connected components are handled by the topology layer before ordinary wave placement.

### Phase 2 — Deterministic initial ordering

Within each wave, sort node IDs lexicographically and assign deterministic initial vertical positions. Isolated nodes are identified separately from nodes connected across waves.

### Phase 3 — Cross-wave relaxation

Cross-wave edges attract related nodes toward compatible vertical positions. Connection counts are aggregated by wave before applying the configured gravity exponent. Iteration order is sorted and bounded by an explicit convergence threshold and maximum iteration count.

This is a deterministic relaxation heuristic. “Gravity” is an implementation metaphor for weighted attraction, not a claim about software or LLM physics.

### Phase 4 — Stability and core detection

The engine records movement from the initial positions and can identify high-connectivity core nodes while keeping isolated nodes distinct. Debug trajectories and convergence information are available to the implementation for inspection.

### Phase 5 — Placement

The selected strategy turns the settled coordinates into layout positions and preserves deterministic ordering for nodes with equal scores.

---

## Inputs and outputs

Inputs include:

- graph nodes and edges;
- topological wave assignment;
- node identity and labels;
- edge multiplicity and cross-wave relationships;
- layout configuration.

Outputs include:

- deterministic node positions;
- wave and convergence metadata;
- optional debug trajectories or logs;
- a layout decision that can be applied or observed in log-only mode.

The layout engine does not generate source code, validate model output, or decide benchmark eligibility.

---

## What is verified

The implementation provides:

- an explicit topology-derived strategy;
- deterministic sorting and bounded iteration;
- an opt-in/log-only control boundary;
- a separation between layout computation and generation validation.

---

## What remains open

The following require measurement:

- whether topology-derived placement improves path consistency;
- whether it reduces human inspection cost;
- whether it changes generation quality independently of context changes;
- which configuration is most stable across graph shapes;
- whether the strategy should ever become the default.

See [RESEARCH_HYPOTHESES.md](./RESEARCH_HYPOTHESES.md) and [docs/experiment-protocol.md](./docs/experiment-protocol.md).
