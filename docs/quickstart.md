# Quick start

## Prerequisites

- [Node.js](https://nodejs.org) 20+ or [Bun](https://bun.sh) v1.2+ runtime
- Internet connection (for fetching revision data from Wikimedia)

See [installation](install.md) for all options (npx, npm, bun, Docker, from source).

## Install the CLI

**Zero install** (no download needed):

```bash
npx @refract-org/cli analyze "Earth" --depth brief
```

**Or install locally:**

```bash
bun add @refract-org/cli
refract analyze "Earth" --depth brief
```

(`wikihistory` also works as an alias.)

See [installation](install.md) for all options (bun, npm, Docker, from source).

## Your first analysis

```bash
refract analyze "Earth"
```

By default, this fetches up to 20 recent revisions of the Wikipedia page "Earth" and runs all available deterministic analyzers (section changes, citation changes, revert detection, template changes). Use `--from` and `--to` to scope to a specific revision range.

Output is printed to stdout as structured JSON.

## View results in the web UI

```bash
refract explore "Earth"
```

Opens a local web server (default port 8899) with a timeline view, evidence table, and diff viewer — the most user-friendly way to browse results.

## Saving output

```bash
refract export "Earth" --format ndjson > earth-events.jsonl
```

Use `refract export` to format output as json, csv, ndjson, html, a signed bundle (`--bundle`), or a replay manifest (`--manifest`).

## Next steps

- [Concepts](concepts.md)
- [CLI reference](cli.md)
- [Analysis depth levels](depth.md)
- [Tutorial: Track Wikipedia changes](tutorials/wikipedia-history.md)
