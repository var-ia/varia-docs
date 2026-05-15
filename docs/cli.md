# CLI command reference

## `wikihistory analyze`

Analyze full edit history of a page.

```bash
wikihistory analyze [page] [options]
```

| Flag | Default | Description |
|---|---|---|
| `page` | required (positional) | Page title |
| `-d, --depth <d>` | `detailed` | Analysis depth: `brief`, `detailed`, `forensic`. See [depth levels](depth.md). |
| `--from <revId>` | — | Start revision ID |
| `--to <revId>` | — | End revision ID |
| `--since <ts>` | — | Filter revisions after this ISO timestamp |
| `-c, --cache` | off | Cache revisions in SQLite (`~/.wikihistory/varia.db`) |
| `--pages-file <path>` | — | Batch file of page titles (one per line) |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |
| `--cache-dir <path>` | `~/.wikihistory` | Cache directory path |

## `wikihistory claim`

Track a specific claim across revisions.

```bash
wikihistory claim <page> [options]
```

| Flag | Default | Description |
|---|---|---|
| `page` | required (positional) | Page title |
| `-t, --text <text>` | required | Claim text to track |
| `-c, --cache` | off | Cache revisions in SQLite |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |

## `wikihistory cron`

One-shot re-observation for cron.

```bash
wikihistory cron <pages-file> [options]
```

| Flag | Default | Description |
|---|---|---|
| `pages-file` | required (positional) | Batch file of page titles (one per line) |
| `-i, --interval <hrs>` | from last observation | Lookback window in hours |
| `--notify-slack` | off | Send Slack notification on changes |
| `--notify-email` | off | Send email notification on changes |
| `--notify-webhook <u>` | — | Send generic webhook POST on changes |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |

## `wikihistory diff`

Cross-wiki comparison (2+ wikis).

```bash
wikihistory diff <topic> [options]
```

| Flag | Default | Description |
|---|---|---|
| `topic` | required (positional) | Page title to compare across wikis |
| `--wiki-a <url>` | required | First wiki API URL |
| `--wiki-b <url>` | required | Second wiki API URL |
| `--wiki-c <url>` | optional | Third wiki API URL |
| `-d, --depth <d>` | `detailed` | Analysis depth. See [depth levels](depth.md) |

## `wikihistory eval`

Run the evaluation harness.

```bash
wikihistory eval [options]
```

| Flag | Default | Description |
|---|---|---|
| `--page <title>` | — | Run only for a specific page |
| `--ground-truth <path\|builtin>` | — | Validate against ground truth labels |

## `wikihistory export`

Export analysis as structured data.

```bash
wikihistory export <page> [options]
```

| Flag | Default | Description |
|---|---|---|
| `page` | required (positional) | Page title |
| `-f, --format <fmt>` | `json` | Output format: `json`, `csv`, `ndjson`, `html` |
| `--bundle` | off | Export as signed evidence bundle (SHA-256). See [bundle format](bundle-manifest.md). |
| `--manifest` | off | Export as replay manifest with all hashes. See [manifest format](bundle-manifest.md). |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |

## `wikihistory explore`

Start a local web explorer with timeline, evidence table, and diff viewer.

```bash
wikihistory explore <page> [options]
```

| Flag | Default | Description |
|---|---|---|
| `page` | required (positional) | Page title |
| `-p, --port <n>` | `8899` | Server port |
| `--no-open` | off | Don't open browser automatically |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |

## `wikihistory visualize`

Export evidence graph as a diagram.

```bash
wikihistory visualize <page> [options]
```

| Flag | Default | Description |
|---|---|---|
| `page` | required (positional) | Page title |
| `-f, --format <fmt>` | `mermaid` | Output format: `mermaid`, `dot` |
| `--all` | off | Show all event types (default: claim events only) |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |

## `wikihistory watch`

Live polling daemon for new edits.

```bash
wikihistory watch <page> [options]
```

| Flag | Default | Description |
|---|---|---|
| `page` | required (positional) | Page title |
| `-s, --section <name>` | — | Watch a specific section only |
| `-i, --interval <ms>` | `60000` | Poll interval in milliseconds |
| `--api <url>` | `en.wikipedia.org` | MediaWiki API base URL |

## `wikihistory mcp`

Start MCP server for AI agent integration.

```bash
wikihistory mcp
```

## Global options

These options apply to most commands:

| Flag | Description |
|---|---|
| `--api <url>` | MediaWiki API base URL (default: `en.wikipedia.org`) |
| `--cache-dir <path>` | Cache directory path (default: `~/.wikihistory`) |
| `--api-key <token>` | API key for bearer token auth |
| `--api-user <user>` | Username for basic auth |
| `--api-password <pass>` | Password for basic auth |
