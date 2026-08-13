# Topological Context Compilation — A Primer

Topological Context Compilation (TCC) is grAIph's name for compiling graph-structured architecture into generation context and ordering decisions.

It is a design approach and an implementation vocabulary. It is not a claim that LLM code generation follows a literal physical law.

---

## The problem

A large software-generation task contains more relationships than a single undifferentiated prompt can represent reliably:

- files import other files;
- interfaces constrain implementations;
- shared utilities are reused across branches;
- cycles require special handling;
- downstream code depends on upstream names and shapes.

A larger context window does not decide which information is necessary for a particular generation unit. Retrieval can find text without explaining how the retrieved material fits into the architecture.

TCC starts from an explicit graph and asks what representation of that graph is appropriate for the current unit and stage.

---

## 1. The graph is an intermediate representation

Nodes represent generation units. Edges represent dependencies or other relationships. Surfaces and contracts can describe what a node provides, what a consumer requires, and how usage is expected to occur.

The graph is not assumed to be perfect. It is an architectural projection that can be checked against emitted implementation evidence.

---

## 2. Topology supplies an order

A dependency graph can be analyzed into strongly connected components and topological waves.

~~~text
Wave 0: independent sources
Wave 1: nodes depending on Wave 0
Wave 2: nodes depending on earlier waves
...
~~~

This order gives the pipeline a deterministic starting point. It does not guarantee that a model will produce correct code, but it makes generation dependencies explicit and prevents a downstream unit from being treated as if its upstream context already existed when it does not.

---

## 3. Context is compiled, not simply copied

For a target node, the compiler can choose a representation for each neighbor:

| Relationship | Possible representation |
|---|---|
| target | task description and current constraints |
| direct dependency | interface, relation surface, bindings, and selected generated content |
| transitive dependency | symbols, signatures, or structural summary |
| distant or unrelated unit | identity or no context |

An attenuation formula may be used as one policy for choosing among these representations. Its parameters are configuration and experiment variables, not universal constants.

The key design question is:

> What information is necessary for this unit at this point in the generation sequence?

---

## 4. Contracts make requirements explicit

A model can receive more useful structure than raw source text:

- provided and required relation surfaces;
- usage-kind bindings;
- structural expectations;
- import/path constraints;
- language capability boundaries.

These contracts are intended to reduce ambiguity. They are not proof that a model followed them, which is why conformance and emitted-code analysis remain separate stages.

---

## 5. Generated code is evidence

After emission, code can be inspected for actual imported-symbol use. That evidence can refine graph relations or reveal that a declared request was not realized.

The system keeps two facts separate:

- **declared intent:** what the graph or user requested;
- **observed evidence:** what the emitted program actually used.

That distinction makes it possible to preserve a user-drawn architectural edge while reporting an unfulfilled binding rather than silently rewriting history.

---

## 6. Candidate state is not committed state

TCC ends in a host boundary, not at the model response:

~~~text
candidate → validation → conformance → persistence → receipt → committed state
~~~

The distinction matters for both user trust and evaluation. A candidate that failed to persist or failed required validation must not be counted as a committed generation.

---

## What TCC does not claim

TCC does not currently claim:

- that graph distance is a universal semantic measure;
- that exponential decay is optimal;
- that graph-structured generation beats frontier coding systems;
- that a previous benchmark score establishes the current result;
- that the mechanisms are novel without comparative literature work;
- that a model response is equivalent to accepted project state.

The active research questions are collected in [RESEARCH_HYPOTHESES.md](../RESEARCH_HYPOTHESES.md).
