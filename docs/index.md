# Refract: the open claim-history layer for public knowledge

**Refract reveals how claims change across public revision histories.**

The printing press froze knowledge in editions. Wikipedia made it mutable. Refract
makes the mutation legible — a deterministic event stream showing where every claim
came from, what changed, what supported it, what challenged it, when it stabilized,
and what context altered its meaning.

Machines do not just need more retrieved text. They need provenance, instability,
disagreement, and temporal change. Refract is the open layer that makes knowledge
legible to machines: `refract observe`, `refract timeline`, `refract diff`, `refract analyze`.

## Why Refract?

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

Refract is open source under AGPL-3.0. Built and maintained by [NextConsensus](https://nextconsensus.com).
