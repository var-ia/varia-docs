# CLI command reference

## `wikihistory analyze`

Fetch revisions and run all analyzers.

```bash
wikihistory analyze --page <page> [options]
```

**Options:**
| Flag | Default | Description |
|---|---|---|
| `--page` | required | Page title |
| `--wiki` | `en.wikipedia.org` | Wiki domain |
| `--limit` | `10` | Number of revisions |
| `--output` | stdout | Output file path |
| `--format` | `json` | Output format (json, ndjson) |

## `wikihistory claim`

Extract claim-level events from analysis output.

```bash
wikihistory claim --input <file> --output <file>
```

## `wikihistory export`

Export event data to a portable format.

```bash
wikihistory export --input <file> --format <format> --output <file>
```

**Formats:** `json`, `csv`, `sqlite`

## `wikihistory watch`

Watch a page for new revisions in real time.

```bash
wikihistory watch --page <page> --interval <seconds>
```

## `wikihistory cron`

Schedule recurring analysis of a page set.

```bash
wikihistory cron --config <cronfile> --db <sqlite-path>
```

## `wikihistory visualize`

Render a timeline of events.

```bash
wikihistory visualize --input <file> --output <file>
```

## `wikihistory diff`

Compare two analysis snapshots.

```bash
wikihistory diff --before <file> --after <file>
```

## `wikihistory eval`

Run the evaluation harness against ground truth.

```bash
wikihistory eval --input <events> --ground-truth <file>
```
