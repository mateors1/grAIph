# Evidence Reconciliation

Evidence reconciliation compares declared graph architecture with actual usage observed in emitted code.

It is an implementation mechanism behind experimental controls, not a claim that every observed reference proves a meaningful architectural dependency.

---

## Two sources of truth

The graph can contain declared architecture:

- user-drawn edges;
- required and provided surfaces;
- binding contracts;
- structural expectations.

Generated code provides a second source:

- imported symbols;
- calls;
- construction;
- injection;
- references;
- imported values consumed as data.

The system keeps these sources distinct.

---

## Evidence-only rewiring

Evidence rewiring is intended to create or upgrade a relation only when the emitted code contains a usage site reached through an imported provider symbol.

A shared type name or a name coincidence is not sufficient evidence.

When a generic edge already exists, the implementation can preserve its identity while adding evidence-derived handles and contracts. This avoids duplicating a user-visible edge merely because its description became more specific.

---

## No-clobber behavior

Import-derived or user-declared contracts are protected from an evidence pass that would overwrite them.

The evidence pass therefore has a no-clobber boundary:

- existing protected contracts are left alone;
- evidence may populate an uncontracted pair;
- evidence may upgrade a generic edge where appropriate;
- evidence does not silently replace declared architecture.

---

## Contract retraction

Evidence can disappear after regeneration. When an emitted file no longer imports a symbol, an evidence-derived contract may be withdrawn or narrowed.

The edge itself is not automatically deleted. A declared edge is architectural intent and can remain even when a particular generation failed to realize it.

When contract provenance is available:

- evidence-derived claims can be retracted;
- declared contracts remain as requests;
- unfulfilled declared symbols are reported instead of erased.

---

## Usage kinds and language boundaries

The implementation distinguishes structural usage from behavioral usage. The public vocabulary includes:

- call;
- construct;
- inject;
- reference;
- imported-value read.

TypeScript can use syntax-aware usage detection when the corresponding opt-in capability is available. Other languages or unavailable strategies fall back to conservative scanning or report the capability as unsupported. A flag being present is not proof that a particular analysis strategy ran.

---

## Relationship to contracts

Relation surfaces describe what a node can provide or require. Binding contracts describe how a consumer uses a provider. Structural contracts describe shape that should survive source suppression.

Evidence reconciliation checks realized usage against those descriptions. It does not turn a single successful match into an empirical validation of the broader generation hypothesis.

---

## Limitations

Evidence can be incomplete or ambiguous:

- dynamic imports and reflection may not be visible;
- language parsers differ in capability;
- generated or indirect calls may be missed;
- conservative scanning can produce false positives;
- a declared architecture can be intentionally unused in one candidate;
- an observed use does not establish semantic correctness.

These limitations are why evidence is recorded with provenance and confidence, not treated as an unconditional truth value.

---

## Open evaluation question

The open research question is whether reconciling implementation evidence back into the graph improves later generation quality, graph accuracy, or debugging visibility. See [RESEARCH_HYPOTHESES.md](../RESEARCH_HYPOTHESES.md).
