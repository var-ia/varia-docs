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
npx wikihistory analyze --page "Earth"
```

## Your first analysis

```bash
wikihistory analyze --page "Earth" --limit 10
```

This fetches the 10 most recent revisions of the Wikipedia page "Earth" and runs all available analyzers (section changes, citation changes, revert detection, template changes).

Output is printed to stdout as structured JSON.

## Saving output

```bash
wikihistory analyze --page "Earth" --limit 50 --output ./output/earth-events.json
```

## Next steps

- [Concepts: Three-Knowledge-Split](concepts.md)
- [CLI reference](cli.md)
- [Tutorial: Track Wikipedia changes](tutorials/wikipedia-history.md)
