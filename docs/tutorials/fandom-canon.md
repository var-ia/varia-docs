# Tutorial: Track canon changes on a Fandom wiki

## Goal

Monitor how a Fandom wiki's canon pages change over time, tracking additions and removals of lore-critical statements.

## Steps

### 1. Configure a Fandom wiki

Fandom wikis use the same MediaWiki API. Point `--api` at the wiki's API endpoint:

```bash
wikihistory analyze "Darth_Vader" --api https://starwars.fandom.com/api.php --depth detailed
```

### 2. Run analyzers with caching

```bash
wikihistory analyze "Darth_Vader" --api https://starwars.fandom.com/api.php --depth detailed -c
```

### 3. Export results

```bash
wikihistory export "Darth_Vader" --api https://starwars.fandom.com/api.php --format json
```

### 4. Track a specific claim

```bash
wikihistory claim "Darth_Vader" --text "midichlorian count is over 20,000" --api https://starwars.fandom.com/api.php -c
```

### 5. Watch for new edits

```bash
wikihistory watch "Darth_Vader" --api https://starwars.fandom.com/api.php --interval 60000
```

## Use case: preserving canonicity

Fandom canon pages drift as new media releases or retcons earlier material. Character backstories, power levels, timelines, and faction alignments are frequently updated. Varia tracks these changes as `claim_first_seen` and `claim_removed` events, letting you see exactly which lore-critical statement changed and when. Compare claim stability across pages to see which characters or settings have the most contested canon.

## Example output

```json
{
  "eventId": "f7a2d0e3c1b84906",
  "eventType": "claim_first_seen",
  "fromRevisionId": 1280012345,
  "toRevisionId": 1280016789,
  "section": "Powers and abilities",
  "before": "",
  "after": "Darth Vader's midichlorian count is over 20,000",
  "timestamp": "2024-12-01T08:15:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "New claim first appeared: Darth Vader's midichlorian count is over 20,000",
      "provenance": {
        "analyzer": "section-differ",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

## Notes

- Fandom wikis use the standard MediaWiki API — pass the API URL with `--api`.
- Fandom wiki APIs may have different rate limits; use `-c` to cache revisions.
- For multi-page monitoring, use a pages file with `wikihistory cron`.
