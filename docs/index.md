# Varia: an open-source deterministic observation engine for public revision histories

**Varia answers: "What changed?"**

Varia is a domain-neutral observability layer for public knowledge. It ingests revision histories from Wikimedia, Fandom, and other public wikis, applies deterministic analysis to extract structured evidence, and surfaces what changed — without deciding what is true.

## Why Varia?

- **Deterministic by default.** L1 analysis is byte-for-byte reproducible. No model, no variance.
- **Three-Knowledge-Split.** Separate layers for observable facts, model-assisted interpretation, and independent ground truth.
- **Pluggable architecture.** Swap analyzers, storage backends, and model adapters without changing the core pipeline.

## Contents

- [Quick start](quickstart.md)
- [Concepts: Three-Knowledge-Split](concepts.md)
- [Event schema reference](schema.md)
- [CLI command reference](cli.md)
- [SDK / package reference](sdk.md)
- [Tutorials](tutorials/wikipedia-history.md)
- [Boundary](boundary.md)
- [Frontier use cases](frontier-use-cases.md)

## Open source

**What changed?** — Everything Varia produces is auditable, reproducible, and inspectable.

**Not included** — Domain-specific decision judgment. Varia observes change. It does not tell you what to do about it.
