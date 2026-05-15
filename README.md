# Varia Docs

Public documentation for [Varia](https://github.com/var-ia/varia), the open-source deterministic observation engine for public revision histories.

**Varia answers: "What changed?"**

---

**Live at:** [var-ia.github.io/varia-docs](https://var-ia.github.io/varia-docs/)

## Quick start

```bash
# One command, zero install
npx @var-ia/cli analyze "Earth" --depth brief
```

See [quickstart](https://var-ia.github.io/varia-docs/quickstart/) for a full walkthrough.

## Packages

| Package | Status | License | Description |
|---|---|---|---|
| `@var-ia/evidence-graph` | [![npm](https://img.shields.io/npm/v/@var-ia/evidence-graph)](https://www.npmjs.com/package/@var-ia/evidence-graph) | CC0-1.0 | Core types & event schemas |
| `@var-ia/ingestion` | [![npm](https://img.shields.io/npm/v/@var-ia/ingestion)](https://www.npmjs.com/package/@var-ia/ingestion) | AGPL-3.0 | Wikimedia API adapters |
| `@var-ia/analyzers` | [![npm](https://img.shields.io/npm/v/@var-ia/analyzers)](https://www.npmjs.com/package/@var-ia/analyzers) | AGPL-3.0 | Deterministic analyzers |
| `@var-ia/cli` | [![npm](https://img.shields.io/npm/v/@var-ia/cli)](https://www.npmjs.com/package/@var-ia/cli) | AGPL-3.0 | CLI tool (`wikihistory`) |
| `@var-ia/persistence` | Not published | AGPL-3.0 | SQLite storage |
| `@var-ia/eval` | Not published | AGPL-3.0 | Evaluation harness |
| `@var-ia/observable` | Not published | AGPL-3.0 | Observable data loader |

## Project status

Varia is in active development by [NextConsensus](https://nextconsensus.com). The deterministic observation layer is usable.

## License

CC-BY-4.0
