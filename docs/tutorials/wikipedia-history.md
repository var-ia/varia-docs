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

## TODO

- Add expected output examples for each step
- Add section on filtering events by type
- Add section on configuring wiki source (other language Wikipedias)
- Add troubleshooting tips for rate limits
