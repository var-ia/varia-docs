# Tutorial: Build a dispute timeline from talk page activity

## Goal

Combine article revision history with talk page activity to build a timeline of a dispute.

## Steps

### 1. Analyze the article

```bash
wikihistory analyze --page "Global_warming" --limit 100 --output ./gw-revisions.json
```

### 2. Get revert events

```bash
wikihistory claim --input ./gw-revisions.json --type revert_detected --output ./gw-reverts.json
```

### 3. Cross-reference with talk page

TODO: Fetch talk page revision history and correlate revert clusters with discussion periods.

### 4. Build the timeline

```bash
wikihistory visualize --input ./gw-reverts.json --output ./gw-dispute-timeline.html
```

## Use case: editorial dispute analysis

TODO: Describe how revert frequency, participant diversity, and talk page activity together indicate dispute intensity.

## TODO

- Add talk page ingestion workflow
- Add correlation analysis between reverts and talk page threads
- Add example output showing a dispute timeline with annotated revert clusters
- Add section on privacy considerations (usernames in output)
