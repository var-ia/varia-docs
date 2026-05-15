# Varia Docs

Public documentation for [Varia](https://github.com/var-ia/varia), the open-source deterministic observation engine for public revision histories.

**Varia answers: "What changed?"**

---

**Live at:** [var-ia.github.io/varia-docs](https://var-ia.github.io/varia-docs/)

## Quick start

```bash
bun add @var-ia/cli
npx wikihistory analyze --page "Earth"
```

See [docs/quickstart.md](docs/quickstart.md) for a full walkthrough.

## Packages

| Package | Status | License | Description |
|---|---|---|---|---|
| `@var-ia/evidence-graph` | Published | CC0-1.0 | Core types & event schemas |
| `@var-ia/ingestion` | Published | AGPL-3.0 | Wikimedia API adapters |
| `@var-ia/analyzers` | Published | AGPL-3.0 | Deterministic analyzers |
| `@var-ia/interpreter` | Published | AGPL-3.0 | Model adapter interface |
| `@var-ia/cli` | Published | AGPL-3.0 | CLI tool (`wikihistory`) |
| `@var-ia/persistence` | Not published | AGPL-3.0 | SQLite storage |
| `@var-ia/eval` | Published | AGPL-3.0 | Evaluation harness |

## Project status

Varia is in active development by [NextConsensus](https://nextconsensus.com). The core L1 (deterministic) layer is usable. L2 (model-assisted) and L3 (ground truth) layers are in progress.

## License

CC-BY-4.0
