# Varia

**Varia answers: "What changed?"**

An open-source, deterministic observation engine for public revision histories. Varia tracks what changes in collaborative knowledge spaces — Wikipedia, Fandom wikis, public registries — and makes those changes auditable, analyzable, and interpretable.

---

## Quick start

```bash
bun add @var-ia/cli
npx wikihistory analyze --page "Earth"
```

See [docs/quickstart.md](docs/quickstart.md) for a full walkthrough.

## Packages

| Package | Description |
|---|---|
| `@var-ia/evidence-graph` | Core types & event schemas |
| `@var-ia/ingestion` | Wikimedia API adapters |
| `@var-ia/analyzers` | Deterministic analyzers |
| `@var-ia/interpreter` | Model adapter interface |
| `@var-ia/cli` | CLI tool (`wikihistory`) |
| `@var-ia/persistence` | SQLite storage |
| `@var-ia/eval` | Evaluation harness |

## Project status

Varia is in active development by [NextConsensus](https://nextconsensus.com). The core L1 (deterministic) layer is usable. L2 (model-assisted) and L3 (ground truth) layers are in progress.

## License

MIT
