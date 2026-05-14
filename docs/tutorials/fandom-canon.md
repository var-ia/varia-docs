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

TODO: Describe how to monitor canon disputes over time, compare claim stability across different Fandom communities.

## TODO

- Add example output for a canon-relevant event
- Add note about Fandom API differences from Wikimedia
- Add section on multi-page monitoring (tracking a topic across pages)
