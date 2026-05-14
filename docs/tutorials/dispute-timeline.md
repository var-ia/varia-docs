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

```bash
wikihistory analyze --page "Talk:Global_warming" --wiki "en.wikipedia.org" --limit 100 --output ./gw-talk.json
```

Talk pages are fetched by prefixing the page title with `Talk:`. Correlate the revert timestamps from step 2 with talk page revision timestamps to identify dispute intensity.

### 4. Build the timeline

```bash
wikihistory visualize --input ./gw-reverts.json --output ./gw-dispute-timeline.html
```

## Use case: editorial dispute analysis

Revert clusters combined with talk page activity indicate dispute intensity. High revert frequency with little talk page discussion suggests unilateral editing or edit-warring. Reverts accompanied by long talk threads indicate active deliberation. Varia's deterministic events let you quantify both dimensions from the same L1 pipeline.

## Example output

```json
{
  "eventId": "revert001a",
  "eventType": "revert_detected",
  "fromRevisionId": 1280100100,
  "toRevisionId": 1280100200,
  "section": "Scientific consensus",
  "before": "The scientific consensus is settled",
  "after": "The scientific consensus is disputed",
  "revertedBy": "UserA",
  "timestamp": "2024-11-18T09:00:00Z",
  "layer": "observed"
}
```

```json
{
  "eventId": "revert001b",
  "eventType": "revert_detected",
  "fromRevisionId": 1280100200,
  "toRevisionId": 1280100300,
  "section": "Scientific consensus",
  "before": "The scientific consensus is disputed",
  "after": "The scientific consensus is settled",
  "revertedBy": "UserB",
  "timestamp": "2024-11-18T09:30:00Z",
  "layer": "observed"
}
```

## Notes

- Talk pages use the same API; prefix the page title with `Talk:`
- Consider rate limits when fetching both article and talk revisions
- Usernames in output should be handled per your privacy policy
