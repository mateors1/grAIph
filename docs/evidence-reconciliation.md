# Evidence and Topology Reconciliation

**Reviewed:** 2026-09-20

Evidence reconciliation compares declared graph architecture with usage observed in emitted code. Topology reconciliation applies an accepted, fingerprint-bound graph mutation to detached candidate state before the host may persist it.

They are related but distinct responsibilities:

- evidence asks what the implementation appears to use;
- topology planning asks what graph change is proposed;
- reconciliation asks whether that proposal can be applied coherently;
- host finalization asks what was durably committed.

None of these steps alone proves semantic correctness or research benefit.

---

## Two sources of architectural information

The graph can contain declared intent:

- user-drawn edges;
- required and provided surfaces;
- binding contracts;
- structural expectations;
- explicit external or dependency anchors.

Generated code provides observed evidence:

- imported symbols;
- calls;
- construction;
- injection;
- references;
- imported values consumed as data.

The system keeps these sources and their provenance distinct.

---

## Evidence-only rewiring

Evidence rewiring is intended to create or strengthen a relation only when emitted code contains an attributable usage site reached through an imported provider symbol.

A shared type name, textual coincidence, or import without observed use is not sufficient behavioral evidence.

When a generic edge already exists, the implementation can preserve its identity while adding evidence-derived handles and contracts. This avoids duplicating a user-visible edge merely because its description became more specific.

---

## No-clobber behavior

Declared and protected import-derived contracts are not silently overwritten by an evidence pass.

The boundary is:

- protected contracts remain intact;
- evidence may populate an uncontracted relationship;
- evidence may upgrade a generic edge where allowed;
- evidence does not silently replace declared intent;
- disagreement becomes a reportable gap.

---

## Retraction and unfulfilled requests

Evidence can disappear after regeneration. When an emitted file no longer imports or uses a symbol, an evidence-derived contract may be withdrawn or narrowed.

The edge itself is not automatically deleted. A declared edge is architectural intent and can remain even when one candidate failed to realize it.

With provenance:

- evidence-derived claims can be retracted;
- declared contracts remain requests;
- unfulfilled declared symbols remain visible;
- previous observations are not rewritten as if they never existed.

---

## Immutable topology plan

Topology analysis produces an immutable plan bound to the source graph fingerprint. The plan may include:

- generation waves and strongly connected components;
- deterministic inferred edges;
- bounded cycle-repair decisions;
- explicit added and removed edge identities;
- analysis, provenance, and evidence references;
- layout/adjacency information consumed by later phases.

Topology Scrutineer owns the plan. It does not directly mutate the authoritative graph.

---

## Reconciliation executor

One executor applies the graph portion of an accepted candidate to a detached clone. It validates:

- plan and graph fingerprint compatibility;
- edge identity and schema;
- add/remove coherence;
- cycle and policy constraints;
- candidate identity and accepted mutation set.

The same semantics are used at the host boundary to verify that committed graph state equals the accepted add/remove result.

This removes competing mutation paths. A planner proposes; the reconciliation executor applies to candidate state; the host alone commits.

---

## Candidate, persisted, and committed

The lifecycle distinguishes:

~~~text
observed evidence
      ↓
immutable topology plan
      ↓
candidate graph mutation
      ↓
accepted candidate
      ↓
host persistence
      ↓
canonical receipt
      ↓
verified committed graph/file state
~~~

A candidate may be coherent without being persisted. Persistence may be attempted without producing a valid receipt. A receipt must cover the accepted candidate before committed status is derived.

---

## Usage kinds and language boundaries

The public usage vocabulary includes:

- call;
- construct;
- inject;
- reference;
- imported-value read.

TypeScript can use syntax-aware detection when the corresponding capability is available. Other languages or unavailable strategies fall back conservatively or report the operation as unsupported. A feature flag being present is not proof that a syntax-aware result was used.

---

## Relationship to visual layout

Graph reconciliation changes architectural state. Topology-derived layout computes visual positions from graph state. They can consume related topology information, but they are not the same mechanism.

Layout does not authorize an edge mutation, and a reconciled edge does not prove that a particular visual placement improves generation.

See [Topology-derived layout](../TOPOLOGY_LAYOUT.md).

---

## Limitations

Evidence can be incomplete or ambiguous:

- dynamic imports and reflection may not be visible;
- language parsers differ in capability;
- generated or indirect calls may be missed;
- conservative scanning can produce false positives;
- a declared architecture can be intentionally unused in one candidate;
- an observed use does not establish semantic correctness;
- a coherent graph mutation does not establish a generation-quality benefit.

These limitations are why evidence carries provenance and why host finalization remains independent.

The open research question is whether reconciling implementation evidence back into the graph improves later generation quality, graph accuracy, or debugging visibility. See [Research hypotheses](../RESEARCH_HYPOTHESES.md).
