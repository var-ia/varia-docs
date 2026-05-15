# Varia: an open-source deterministic observation engine for public revision histories

**Varia answers: "What changed?"**

Varia is a domain-neutral observability layer for public knowledge. It ingests revision histories from Wikimedia, Fandom, and other public wikis, applies deterministic analysis to extract structured evidence, and surfaces what changed — without deciding what is true.

## Why Varia?

- **Deterministic by default.** Analysis is byte-for-byte reproducible. No model, no variance.
- **Provenance-tagged.** Every event carries revision, section, and timestamp context.
- **Pluggable architecture.** Swap analyzers and storage backends without changing the core pipeline.

## Contents

- [Quick start](quickstart.md)
- [Install](install.md)
- [Event schema reference](schema.md)
- [Event taxonomy](events.md)
- [CLI command reference](cli.md)
- [SDK / package reference](sdk.md)
- [MCP: AI agent integration](mcp.md)
- [Naming conventions](naming.md)
- [Security](security.md)
- [Tutorials](tutorials/wikipedia-history.md)
- [Boundary](boundary.md)
- [Frontier use cases](frontier-use-cases.md)

## Open source

Varia is open source under AGPL-3.0.
