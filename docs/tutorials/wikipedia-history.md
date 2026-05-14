# Tutorial: Track changes on a Wikipedia page

## Goal

Use Varia to analyze the revision history of a Wikipedia page and see what changed over the last 50 edits.

## Steps

### 1. Install the CLI

```bash
bun add -g @var-ia/cli
```

### 2. Analyze

```bash
wikihistory analyze --page "Earth" --limit 50 --output ./earth-events.json
```

### 3. View results

```bash
cat ./earth-events.json | bunx json
```

### 4. Understand the events

- **Section events** — sections were added, removed, or renamed
- **Citation events** — citations were added or removed
- **Revert events** — someone reverted an edit

### 5. Visualize

```bash
wikihistory visualize --input ./earth-events.json --output ./earth-timeline.html
```

## Next steps

- [Track canon changes on Fandom](fandom-canon.md)
- [Monitor citation churn on medical pages](citation-churn.md)
- [Build a dispute timeline](dispute-timeline.md)

## Example output

`wikihistory analyze` produces a stream of events. Here are two example events from the Earth page:

```json
{
  "eventId": "wiki001a",
  "eventType": "section_added",
  "fromRevisionId": 1280110001,
  "toRevisionId": 1280110100,
  "section": "Geology",
  "before": "",
  "after": "== Geology ==\nEarth's crust consists of tectonic plates...",
  "timestamp": "2024-11-25T12:00:00Z",
  "layer": "observed"
}
```

```json
{
  "eventId": "wiki001b",
  "eventType": "citation_added",
  "fromRevisionId": 1280110100,
  "toRevisionId": 1280110200,
  "section": "Atmosphere",
  "before": "",
  "after": "<ref>{{cite web |title=Earth Fact Sheet |url=...}}</ref>",
  "timestamp": "2024-11-25T14:00:00Z",
  "layer": "observed"
}
```

## Troubleshooting

- **Rate limits**: add `--delay 1000` between requests
- **Large pages**: start with `--limit 5` to test
- **Other wikis**: use `--wiki de.wikipedia.org` for German Wikipedia
- **Output too large**: use `--format ndjson` for line-delimited output
