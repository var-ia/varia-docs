# Varia: the git log for public knowledge

**Varia answers: "What changed?"**

Varia is a deterministic observation layer for public knowledge. It ingests
revision histories from Wikipedia, Fandom, and any MediaWiki instance, extracts
structured evidence, and emits a provenance-tagged event stream — without calling
a model, without deciding what is true.

## Why Varia?

- **Deterministic by default.** Every run on the same revision range produces identical output. No model, no variance.
- **Provenance-tagged.** Every event carries revision, section, timestamp, and analyzer identity.
- **Reusable primitive.** `claim + source + wording + placement + stability + time` — a substrate downstream systems build on, without contaminating.

## Contents

- [Quick start](quickstart.md)
- [Install](install.md)
- [Concepts](concepts.md)
- [Event schema reference](schema.md)
- [Event taxonomy](events.md)
- [CLI command reference](cli.md)
- [SDK / package reference](sdk.md)
- [Analysis depth levels](depth.md)
- [Export formats: bundles and manifests](bundle-manifest.md)
- [MCP: AI agent integration](mcp.md)
- [Naming conventions](naming.md)
- [Glossary](glossary.md)
- [Troubleshooting / FAQ](faq.md)
- [Security](security.md)
- [Tutorials](tutorials/wikipedia-history.md)
- [Boundary](boundary.md)
- [Frontier use cases](frontier-use-cases.md)

## Open source

Varia is open source under AGPL-3.0.
