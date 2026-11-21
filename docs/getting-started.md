# Getting Started with the Public Preview

**Reviewed:** 2026-11-21

This repository is a documentation and research preview. The v0.4.0 public release contains these documents and their repository source archive, not an engine binary or installable extension. It includes architecture explanations, evidence summaries, hypotheses, and experiment protocols. The engine source and development extension remain private. This repository does not provide a verified public product installation path, a runnable sample, or a hosted editor. See the [release scope](./releases/2026-11-21.md).

## If you are evaluating the design

1. Read the [illustrative workflow](./illustrative-workflow.md) to see what a graph, candidate, and host receipt mean in one task.
2. Read the [pipeline overview](./pipeline-overview.md) for stage responsibilities and the persistence boundary.
3. Use the [evidence guide](./evidence-guide.md) to distinguish passing checks from preliminary observations and proposed experiments.
4. Consult [Current state](../CURRENT_STATE.md) for the governing claim boundary and open release gates.

You can inspect these materials without a provider account or local toolchain. Cloning this public repository supplies the same documentation; it does not install the implementation.

## If you want to reproduce a result

The public summaries describe retained private artifacts. They are not complete public reproduction packages: the engine, some task inputs, and raw provider/host evidence are not distributed here. Reading a summary is therefore different from independently reproducing it.

The [benchmark specification](../BENCHMARK_SPEC.md) explains the controls a new relation-surface experiment requires. It is a protocol, not a command that runs from this checkout. A reproduction would need an authorized implementation build, declared inputs and evaluator, provider configuration, and the corresponding retained artifacts.

For implementation access or a specific evidence question, contact [hello@graiph.dev](mailto:hello@graiph.dev). Access and distribution are handled separately; this preview does not promise package, platform, or account availability.

Existing implementation collaborators should use the onboarding instructions and pinned artifacts supplied with their checkout. This public guide does not replace that setup or declare a release-qualified build.

## If you want to contribute

Factual corrections, methodological criticism, clearer explanations, and adequately scoped replication reports are welcome. See [Contributing](../.github/CONTRIBUTING.md). Keep private source, credentials, and raw internal logs out of public issues and pull requests.
