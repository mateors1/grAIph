# grAIph Benchmark Specification
## Version 1.0 — May 2026

**Status:** Stable. Cite this version alongside every benchmark run.  
**Maintained by:** Mateo Rendon Suarez (hello@graiph.dev)  
**History:** Derived from 23 benchmark runs (V1-V23) over six weeks. Methodology
refined iteratively as new failure modes were discovered. This document represents
the stabilized framework as of V23.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2026-04 | Initial byte-identical metric. Flat scoring. No pipeline failure distinction. |
| 0.2 | 2026-04 | originalSource poisoning discovered. Reconstruction track retired. Two-track framework introduced. |
| 0.3 | 2026-05 | Known bug catalog formalized. ENGINE-PLUGIN-INJECT, DROP-DECORATOR, BLOB-ENCODE added. |
| 0.4 | 2026-05 | Adjusted score concept introduced for pipeline-fixable gaps. Comment-stripped diff (code-only metric) added. |
| 0.5 | 2026-05 | Four-metric framework formalized. Adjusted score definition tightened — style differences excluded. Functional equivalence added as separate metric. |
| **1.0** | **2026-05-21** | **Stable. BENCHMARK_SPEC.md formalized. Version tracking begins.** |

---

## Part 1 — Ground Truth

**Project:** TEM (Template Extension for Marvel) — Elgato Stream Deck plugin for
Unreal Engine integration  
**Repository:** Private (pre-grant). Available for verification on request.  
**Ground truth path:** `c:\Users\mateo\elgatoUengine\src\`  
**Ground truth authors:** Human-written by Mateo Rendon Suarez with Claude Opus 4.6
and Gemini 2.5 Pro assistance

### Ground Truth Metrics
| Metric | Value |
|--------|-------|
| TypeScript files | 37 |
| Total TS LOC | 4,004 |
| Python files | 5 |
| JavaScript files | 5 |
| Critical hub files | 4 (ViewManager, ActiveTargetService, PropertyVirtualView, MaterialSlotVirtualView) |
| Action files | 14 (all with @action decorators and correct UUIDs) |

### Ground Truth Integrity Rules
- Ground truth files are NEVER modified between benchmark runs
- Ground truth is the human-written original, not any generated output
- Any change to ground truth requires a version bump in this document
- The originalSource benchmark track (using ground truth as generation input) is
  archived and excluded from the clean leaderboard — results are theoretical ceilings only

---

## Part 2 — Benchmark Tracks

### Track A: Pure Generation (primary)
**Definition:** Graph-only generation. No source code anywhere in the pipeline.
`suppressOriginalSource: true` throughout. Measures generation quality from
graph specification alone.

**Valid run requirements:**
- `enableBenchmarkLogging: true`
- `suppressOriginalSource: true` in all agents including QA Squire and Conformance
- No `originalSource` field populated on any node
- All 37 ground truth TS files must be attempted (missing = MISS, not skip)

**This is the primary scientific track.** All leaderboard positions are Track A
unless explicitly marked otherwise.

### Track B: Assisted Generation
**Definition:** Graph + AssertionHintContract. QA Squire and Conformance receive
sealed metadata (exported signatures, behavioral markers, structural metrics) derived
from source. Coder receives NO source content. Measures the contribution of
informed reviewing to generation quality.

**Valid run requirements:**
- `enableBenchmarkAssisted: true`
- AssertionHintContract populated for each node
- Coder context verified to contain no raw source content
- Must be compared against a Track A run of the same model and config

**The delta between Track A and Track B scores** is the informed-reviewing contribution.

### Track C: Topology Generation
**Definition:** Track A with topology layout enabled. Files generated to
`core/`, `modules/`, `orphans/` directories derived from Kahn wave coordinates.
Measures topology algorithm contribution.

**Valid run requirements:**
- All Track A requirements
- `enableTopologyLayout: true`
- Shadow log present at `.graiph/topology_<timestamp>.json`
- Shadow log must report `graphDiameter` and `clusteringEnabled`
- Byte-identical metric: **NOT APPLICABLE** (all import paths change with topology)
- Compile check: uses native topology `tsconfig.json` (includes `core/**/*`, `modules/**/*`)

### Track D: Poisoned (archived)
**Definition:** originalSource present in Coder context. Measures theoretical ceiling
when source is available. **NOT a generation benchmark.** Archived for reference only.
Results not entered in clean leaderboard. Reported separately as pipeline determinism
ceiling (100% byte-perfect on external machine, May 2026).

---

## Part 3 — Run Context Declaration

Every benchmark run MUST include a run context declaration before any analysis.
This is mandatory — a run without a context declaration is not a valid benchmark.

```
RUN CONTEXT
───────────────────────────────────────────────
Benchmark Spec Version:  1.0
Benchmark Track:     [ ] A-pure  [ ] B-assisted  [ ] C-topology  [ ] D-poisoned
Model:               ___________________
Model parameters:    ___________________ (if known)
Quantization:        ___________________ (if applicable)
Folder:              graiph-generated<LABEL>
QA Squire:           [ ] full  [ ] incomplete  [ ] disabled
Topology enabled:    [ ] yes  [ ] no
Source in context:   [ ] yes (poisoned)  [ ] no (clean)
Pipeline version:    ___________________ (date or plan reference)
Other variances:     ___________________
───────────────────────────────────────────────
```

**Pipeline version** refers to which pipeline improvement plan was active during
the run. Examples: "pre-Wave-0", "post-Wave-0-2", "post-plan-20260521".
This field makes before/after comparisons possible across runs.

---

## Part 4 — The Four Metrics

Every valid run reports all four metrics. They answer different questions.

### Metric 1: Byte-Identical (raw)
**Question:** Did the model reproduce this file exactly, byte for byte?
**Method:** `fc.exe /N <ground_truth> <generated>` — reports "no differences" or diff.
**Reports as:** `X/37 (Y%)`
**Not applicable:** Track C (topology) runs — all import paths change.
**Interpretation:** High byte-identical means the model reproduced the original
faithfully. Low byte-identical may indicate model quality issues OR style differences.
Use Metric 2 to distinguish.

### Metric 2: Byte-Identical (code-only)
**Question:** Is the logic equivalent after stripping comments and whitespace?
**Method:** Strip all TypeScript comments (JSDoc, inline, block) and normalize
whitespace from both files, then `fc.exe`. See SKILL.md Step 3b for implementation.
**Reports as:** `X/37 (Y%)`
**Interpretation:** If raw=5/37 but code-only=20/37, the model preserved logic but
stripped documentation. The delta (15 files) is the COMMENT-STRIP signal.
If raw≈code-only, differences are logic-level not cosmetic.

### Metric 3: Adjusted Score
**Question:** What would the score be with pipeline-fixable gaps closed?
**Eligible adjustments (pipeline-fixable only):**
- JSON blob extraction failure (file writer bug)
- ORCHESTRATOR-SKIP (missing file, post-batch completeness check)
- Mandatory compile check errors (tsc catches ALL syntax errors)
- Known conformance rule gaps (declared but not yet enforced)

**NOT eligible for adjustment:**
- Style differences (`.js` suffixes, `??` vs `||`, `interface` vs `type`)
- JSDoc additions or removals
- Logging provider differences (`console` vs `streamDeck.logger`)
- Model architectural opinions (singleton vs static class)
- Verbosity differences (more or fewer lines for same logic)

**Reports as:** `X.X/10 (adj)` or omitted if no pipeline-fixable gaps present
**Rule:** The adjusted score must specify WHICH pipeline gap justifies each point.
Unspecified adjustments are invalid.

### Metric 4: Functional Equivalence
**Question:** How much of the generated code produces the same runtime behavior
under reasonable tolerance?
**Tolerance rules:**
- `.js` import suffixes → style, passes
- `public override` vs `override` → equivalent, passes
- `interface` vs `type` for same shape → equivalent, passes
- `readonly` additions → defensive, passes
- `??` vs `||` where semantically equivalent → passes
- JSDoc additions → passes
- Extra try/catch wrapping same logic → passes
- `console` vs `streamDeck.logger` → passes (logging tolerance)
- Content-correct refactors (same output, different encoding) → passes

**Does NOT pass under tolerance:**
- Wrong method signatures (different parameters)
- Missing methods (features absent)
- Wrong property access patterns
- Placeholder UUIDs
- Compile errors (these are real bugs)
- Missing files

**Reports as:** `X/37 (Y%) functional equivalence`

---

## Part 5 — Score Modifiers

Applied BEFORE scoring. Must be declared in the run report.

| Variance | Score Label | Rule |
|----------|-------------|------|
| QA Squire incomplete/disabled | `X.X/10 (floor)` | Score is minimum achievable. State expected ceiling if repair loop had run. |
| Track C (topology) | `X.X/10 (topology)` | Byte-identical weight → 0%, redistributed (+5% compile, +5% intent). Not comparable to Track A scores directly. |
| Track D (poisoned) | `CEILING ONLY` | Not entered in leaderboard. Reported as pipeline determinism ceiling. |
| Single-model pipeline | Note in metadata | All agents ran on same model tier. Strengthens the pipeline-as-bottleneck claim. |
| External machine | Note `✓ ext` | Confirms cross-environment reproducibility. |
| Cascade compile errors | Note root cause + cascade count | 33 errors from one spec gap ≠ 33 independent bugs. Weighted differently in scoring. |

---

## Part 6 — Scoring Rubric v1.0

### Dimension Weights

| Dimension | Track A/B Weight | Track C Weight | Notes |
|-----------|-----------------|----------------|-------|
| Critical hub files (4/4 pass) | 20% | 20% | Pass = byte-identical (A/B) or architecturally correct (C) |
| Compile check (tsc --noEmit) | 15% | 20% | Track C weighted higher — compile is primary quality signal |
| Intent fidelity (avg 6 files) | 15% | 20% | Track C weighted higher — byte-identical N/A |
| Functional bugs | 15% | 15% | Known signatures + heuristic scan |
| Workability rating | 10% | 10% | 0-10, "could a developer ship from this?" |
| Identical file ratio | 10% | 0% | Raw for "with comments", code-only for "code-only". N/A for Track C. |
| File layout correctness | 8% | 8% | Correct subdirectory placement |
| Ghost/empty files | 7% | 7% | Zero = full marks |

### Score Thresholds

| Score | Criteria |
|-------|----------|
| 9.0–10.0 | All 4 hubs pass; 0 functional bugs; 85%+ identical; tsc clean; intent ≥2.5; workability ≥9 |
| 8.0–8.9 | All 4 hubs pass; ≤2 functional bugs; 70%+ identical; ≤10 tsc errors; workability ≥7 |
| 7.0–7.9 | All 4 hubs pass; ≤4 functional bugs; 60%+ identical; ≤25 tsc errors; workability ≥6 |
| 6.0–6.9 | 3/4 hubs pass OR critical bugs limited; 50%+ identical; workability ≥5 |
| 5.0–5.9 | 2/4 hubs pass; multiple compile bugs; 40%+ identical; workability ≥4 |
| 3.0–4.9 | 1–2 hubs fail; may not boot; ghost files possible; workability ≤3 |
| <3.0 | Plugin dead on arrival; 2+ hub failures; workability ≤1 |

### Intent Fidelity Scale (per file, 0–3)

| Score | Meaning |
|-------|---------|
| 3/3 | Public API surface, exports, and architectural role fully preserved |
| 2/3 | Core role preserved; 1-2 secondary methods missing or signature drift on non-critical surfaces |
| 1/3 | Shell present; most methods stubbed or hallucinated; imports from this file would break |
| 0/3 | Wrong abstraction (CRUD-STUB, generic scaffold, unrelated class) |

### Workability Scale (0–10)

| Score | Meaning |
|-------|---------|
| 9–10 | Drop-in replacement. Boots, all actions register, all views render. |
| 7–8 | Boots; 1-5 small bugs; <2h dev time to parity. |
| 5–6 | Architecturally sound; multiple features broken; half-day fixup. |
| 3–4 | Boots but most features broken; OR doesn't boot but recoverable. Multi-day. |
| 1–2 | Useful as reference skeleton only. |
| 0 | Worse than starting from scratch. |

---

## Part 7 — Known Bug Catalog v1.0

| Bug ID | Detection Pattern | Effect | Severity |
|--------|-------------------|--------|----------|
| `NEWLINE-ESCAPE` | `\\n` in string/template literals | Titles display `\n` literally | 🟠 Runtime |
| `CRUD-STUB` | Hub file → 25-line CRUD template | Complete feature loss | 🔴 Fatal |
| `GHOST-FILE` | 0-byte file | Import resolution failure | 🔴 Fatal |
| `BLOB-ENCODE` | Entire file as 1-line JSON/escaped string | TypeScript won't parse | 🔴 Compile |
| `DROP-DECORATOR` | `@action` missing on SingletonAction subclass | Action never registered | 🔴 Runtime |
| `DROP-TYPE` | Local `type FooSettings` removed | tsc compile error | 🔴 Compile |
| `PHANTOM-IMPORT` | Import from non-existent module path | Compile break | 🔴 Compile |
| `INTERFACE-GUT` | Interface body stripped (VirtualView 29→5 lines) | Cascade compile errors | 🔴 Cascade |
| `ORCHESTRATOR-SKIP` | Root/entry node not generated | Plugin cannot boot | 🔴 Fatal |
| `ENGINE-PLUGIN-INJECT` | Phantom `IFooEngine`/`IFooPlugin` appended | Dead code; spreads via reusable exports | ⚠️ Additive |
| `TYPE-ERASURE` | Typed params → `any` | Type safety regression | ⚠️ Safety |
| `DECORATOR-PLACEHOLDER` | `@action({ UUID: "com.example.action" })` | Action won't match manifest | 🟠 Runtime |
| `DECORATOR-FORMAT` | `@action("uuid")` vs `@action({ UUID: "uuid" })` | Action may not register | 🟠 Runtime |
| `DROP-OVERRIDE` | `override` keyword stripped | Cosmetic only | ✅ Cosmetic |
| `ADD-DEFAULT-EXPORT` | Redundant `export default` appended | Harmless in most cases | ✅ Cosmetic |
| `CASE-RENAME` | `NumericHandler.ts` → `numericHandler.ts` | Case-sensitive FS break | ⚠️ Compile |
| `COMMENT-STRIP` | JSDoc/inline comments removed | Logic intact, maintainability loss | ✅ Cosmetic |
| `CATCH-STRIP` | `catch(err)` body gutted/empty | Silent error swallowing | 🟠 Runtime |
| `FILE-DUPLICATION` | Files in both `src/` AND topology paths | Flat-path writer running alongside topology | ⚠️ Infrastructure |
| `ABSOLUTE-IMPORT` | `from "src/services/Foo"` instead of `"../services/Foo"` | Compile break without baseUrl | ⚠️ Compile |
| `PATTERN-DRIFT` | Singleton instance instead of static class | API change for all consumers | ⚠️ Architecture |
| `DUPLICATE-IMPORT` | Same module imported twice | tsc warning, harmless | ✅ Cosmetic |
| `OVER-ENGINEERING` | Dead interfaces/abstractions added | Maintenance overhead | ⚠️ Cosmetic |

---

## Part 8 — Abstraction Hierarchy for Pipeline Fixes

When a repeating failure is identified, fix it at the correct abstraction level.
Fix as high as possible — Level 1 prevents, Level 5 patches.

| Level | Name | When to use | Generalizes? |
|-------|------|-------------|--------------|
| 1 | SPEC (graph node) | Model couldn't have known without being told | Yes — fixes every future run of every model |
| 2 | PROMPT (Coder brief) | Model knew the concept, applied inconsistently | Yes — fixes pattern for all files |
| 3 | CONFORMANCE (rule in registry) | Structural invariant violation declared in graph | Yes — for declared contracts |
| 4 | QA SQUIRE (compile + assertions) | Syntactic malformation, language-level truth | Maximally — syntax is universal |
| 5 | POST-PROCESSING (file writer) | Wrong artifact format from pipeline infrastructure | Yes — infrastructure fix for all models |

**Rule:** A patch that fixes one run without generalizing to future runs is not a
pipeline improvement. It is a workaround. Document it as such.

---

## Part 9 — Leaderboard Rules

### Clean Leaderboard Eligibility
- Track A or Track B only (no Track D)
- All 37 ground truth TS files attempted
- Run context declaration complete
- Benchmark Spec version cited

### Reporting Format
```
| Run | Model | Params | Track | Spec | Score | Adj | Identical | tsc | QA | Notes |
```

### Comparison Rules
- Track A scores are comparable to each other
- Track B scores are comparable to each other
- Track A and Track B are NOT directly comparable (different information conditions)
- Track C scores are NOT directly comparable to Track A/B (different output structure)
- Runs under different pipeline versions are comparable only for before/after analysis
  of the same model — not for cross-model comparison

### The Longitudinal Claim
The V10→V23 Flash Lite comparison (+4.9 points, same model) is a valid pipeline
improvement claim DESPITE different pipeline versions because:
1. The model is identical (same architecture, same quantization)
2. The graph specification is identical (same TEM project)
3. The only variable is the pipeline improvements applied between runs
4. The delta is large enough (4.9 points) to exceed any reasonable measurement noise

This is the benchmark's strongest empirical claim. Future runs should attempt similar
before/after controls on additional models to triangulate the finding.

---

## Part 10 — Benchmark Evolution Policy

### When to bump the version
- New metric added
- Scoring rubric weights changed
- New bug pattern added to catalog
- Tolerance rules changed
- Ground truth modified (requires major version bump)

### How to bump
1. Update the version table at the top of this document
2. Note what changed and why
3. Update any affected benchmark run reports to cite the new version
4. Run reports from prior versions remain valid under their cited version

### What does NOT require a version bump
- New benchmark runs (they cite the current version)
- New models tested
- Pipeline improvements (these are tracked in DEVLOG.md and plan files)

---

## Appendix A — Single-Model Pipeline Note

As of V22/V23, all grAIph pipeline agents — TopologyScrutineer, PatternSpecialist,
CodeExplorer, TaskDecomposer, Coder, Conformance, QualityGate — run on the SAME
model tier when a single model is configured. This means:

A V22 run (Gemma 4 E2B 2.3B Q4, 8.1 adjusted) used a 2.3B model for:
- Architectural pattern identification (PatternSpecialist)
- Task decomposition (TaskDecomposer)
- Code generation (Coder)
- Conformance evaluation (Conformance)
- Quality gate decisions (QualityGate)

This strengthens the "pipeline as bottleneck" claim: the accountability layer
achieves high quality at small model tier, not just the generation step.

Future runs should note when different models are used per agent role (triage routing).

---

## Appendix B — Precomputed Stub Architecture (planned)

A future Phase 0C in the batch runner will precompute the entire file structure
as TypeScript stubs before any generation begins:

```
Phase 0A: Compute topology (Kahn waves, coordinates)
Phase 0B: Create directory structure
Phase 0C: Generate stubs from declared outputs (no LLM)
           - Read relationFace.outputs per node
           - Emit minimal valid TypeScript skeleton at resolved path
           - Mark node as stub-pending in state
Phase 1-N: Wave generation (fill stubs with implementations)
Phase N+1: Post-batch completeness check (no stubs remain empty)
```

Stubs export declared symbols with empty implementations, making import chains
valid from day one. PHANTOM-IMPORT becomes structurally impossible.
ORCHESTRATOR-SKIP becomes immediately detectable.

This is the TypeScript header file equivalent for LLM compilation.
When implemented, it will be documented as a benchmark infrastructure change
requiring a version bump.
