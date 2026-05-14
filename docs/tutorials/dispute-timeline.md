# Tutorial: Build a dispute timeline from talk page activity

## Goal

Combine article revision history with talk page activity to build a timeline of a dispute.

## Steps

### 1. Analyze the article

```bash
wikihistory analyze "Global_warming" --depth detailed -c
```

### 2. Analyze the talk page

Talk pages are fetched by prefixing the page title with `Talk:`:

```bash
wikihistory analyze "Talk:Global_warming" --depth detailed -c
```

### 3. Visualize the combined timeline

```bash
wikihistory visualize "Global_warming" --format mermaid --all
```

### 4. Export for further analysis

```bash
wikihistory export "Global_warming" --format ndjson > global-warming-events.ndjson
```

Correlate revert timestamps with talk page revision timestamps to identify dispute intensity.

## Use case: editorial dispute analysis

Revert clusters combined with talk page activity indicate dispute intensity. High revert frequency with little talk page discussion suggests unilateral editing or edit-warring. Reverts accompanied by long talk threads indicate active deliberation. Varia's deterministic events let you quantify both dimensions from the same L1 pipeline.

## Example output

```json
{
  "eventId": "3c5e7f9a1b2d4068",
  "eventType": "revert_detected",
  "fromRevisionId": 1280100100,
  "toRevisionId": 1280100200,
  "section": "Scientific consensus",
  "before": "The scientific consensus is settled",
  "after": "The scientific consensus is disputed",
  "timestamp": "2024-11-18T09:00:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "Edit reverted in section Scientific consensus",
      "detail": "Content replaced with previous version",
      "provenance": {
        "analyzer": "revert-detector",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

```json
{
  "eventId": "4d6f8a0b2c3e5179",
  "eventType": "revert_detected",
  "fromRevisionId": 1280100200,
  "toRevisionId": 1280100300,
  "section": "Scientific consensus",
  "before": "The scientific consensus is disputed",
  "after": "The scientific consensus is settled",
  "timestamp": "2024-11-18T09:30:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "Edit reverted in section Scientific consensus",
      "detail": "Content replaced with previous version",
      "provenance": {
        "analyzer": "revert-detector",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

## Notes

- Talk pages use the standard MediaWiki API; prefix the page title with `Talk:`.
- Consider caching with `-c` when fetching both article and talk revisions.
- Varia does not expose editor usernames in event output — the `revert_detected` event does not include a `revertedBy` field. If you need contributor attribution, use the MediaWiki API directly.
