# Quick start

## Prerequisites

- [Bun](https://bun.sh) v1.2+ runtime (or Node.js 20+)
- Internet connection (for fetching revision data from Wikimedia)

See [installation](install.md) for all options (npm, Docker, from source).

## Install the CLI

```bash
bun add -g @var-ia/cli
```

Or use directly with `npx`:

```bash
npx wikihistory analyze "Earth"
```

## Your first analysis

```bash
wikihistory analyze "Earth"
```

By default, this fetches up to 20 recent revisions of the Wikipedia page "Earth" and runs all available deterministic analyzers (section changes, citation changes, revert detection, template changes). Use `--from` and `--to` to scope to a specific revision range.

Output is printed to stdout as structured JSON.

## View results in the web UI

```bash
wikihistory explore "Earth"
```

Opens a local web server (default port 8899) with a timeline view, evidence table, and diff viewer — the most user-friendly way to browse results.

## Saving output

```bash
wikihistory export "Earth" --format ndjson > earth-events.jsonl
```

Use `wikihistory export` to format output as json, csv, ndjson, a signed bundle (`--bundle`), or a replay manifest (`--manifest`).

## Next steps

- [Concepts](concepts.md)
- [CLI reference](cli.md)
- [Analysis depth levels](depth.md)
- [Tutorial: Track Wikipedia changes](tutorials/wikipedia-history.md)
