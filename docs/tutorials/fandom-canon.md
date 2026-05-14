# Tutorial: Track canon changes on a Fandom wiki

## Goal

Monitor how a Fandom wiki's canon pages change over time, tracking additions and removals of lore-critical statements.

## Steps

### 1. Configure a Fandom wiki

```bash
wikihistory analyze --page "Darth_Vader" --wiki "starwars.fandom.com" --limit 20
```

### 2. Run analyzers

```bash
wikihistory analyze --page "Darth_Vader" --wiki "starwars.fandom.com" --limit 50 --output ./canon-events.json
```

### 3. Examine claim changes

```bash
wikihistory claim --input ./canon-events.json --output ./canon-claims.json
```

### 4. Watch for new edits

```bash
wikihistory watch --page "Darth_Vader" --wiki "starwars.fandom.com" --interval 300
```

## Use case: preserving canonicity

Fandom canon pages drift as new media releases or retcons earlier material. Character backstories, power levels, timelines, and faction alignments are frequently updated. Varia tracks these changes as `claim_added`, `claim_modified`, and `claim_removed` events, letting you see exactly which lore-critical statement changed, when, and by whom. Compare claim stability across pages to see which characters or settings have the most contested canon.

## Example output

```json
{
  "eventId": "canon001a",
  "eventType": "claim_added",
  "fromRevisionId": 1280012345,
  "toRevisionId": 1280016789,
  "section": "Powers and abilities",
  "before": "",
  "after": "Darth Vader's midichlorian count is over 20,000",
  "timestamp": "2024-12-01T08:15:00Z",
  "layer": "observed"
}
```

## Notes

- Fandom uses the same MediaWiki API as Wikipedia — `--wiki starwars.fandom.com` works
- Fandom wiki APIs may have different rate limits
- For multi-page monitoring, use a config file with `wikihistory cron`
