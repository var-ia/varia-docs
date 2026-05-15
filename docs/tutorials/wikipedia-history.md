# Tutorial: Track changes on a Wikipedia page

## Goal

Use Varia to analyze the revision history of a Wikipedia page and see what changed.

## Steps

### 1. Run your first analysis

Zero install:

```bash
npx @var-ia/cli analyze "Earth" --depth detailed
```

Or with a local install:

```bash
bun add @var-ia/cli
wikihistory analyze "Earth" --depth detailed -c
```

### 2. View results

Events print to stdout. For structured export or an HTML report:

```bash
wikihistory export "Earth" --format html > earth-report.html
```

### 3. Understand the events

- **Section events** — sections were reorganized (`section_reorganized`)
- **Citation events** — citations were added, removed, or replaced (`citation_added`, `citation_removed`, `citation_replaced`)
- **Revert events** — someone reverted an edit (`revert_detected`)

### 4. Visualize

```bash
wikihistory visualize "Earth" --format mermaid
```

To see all event types (not just claim events):

```bash
wikihistory visualize "Earth" --format mermaid --all
```

## Next steps

- [Track canon changes on Fandom](fandom-canon.md)
- [Monitor citation churn on medical pages](citation-churn.md)
- [Build a dispute timeline](dispute-timeline.md)

## Example output

`wikihistory analyze` produces a stream of events. Here are two example events from the Earth page:

```json
{
  "eventId": "a3f5c2e1b7d409fa",
  "eventType": "section_reorganized",
  "fromRevisionId": 1280110001,
  "toRevisionId": 1280110100,
  "section": "Geology",
  "before": "",
  "after": "== Geology ==\nEarth's crust consists of tectonic plates...",
  "timestamp": "2024-11-25T12:00:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "Section Geology added with 3 paragraphs",
      "provenance": {
        "analyzer": "section-differ",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

```json
{
  "eventId": "8e12b4f1a6d3c097",
  "eventType": "citation_added",
  "fromRevisionId": 1280110100,
  "toRevisionId": 1280110200,
  "section": "Atmosphere",
  "before": "",
  "after": "<ref>{{cite web |title=Earth Fact Sheet |url=...}}</ref>",
  "timestamp": "2024-11-25T14:00:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "Citation added in section Atmosphere",
      "provenance": {
        "analyzer": "citation-tracker",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

## Troubleshooting

- **Rate limits**: Use `-c` to cache revisions and avoid re-fetching.
- **Large pages**: Analyze a range with `--from` and `--to` revision IDs.
- **Other wikis**: Use `--api` to point to a different MediaWiki API (e.g., `--api https://de.wikipedia.org/w/api.php`).
- **Output too large**: Use `wikihistory export "Earth" --format ndjson` for line-delimited output.
