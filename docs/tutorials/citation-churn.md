# Tutorial: Monitor citation churn on a medical/scientific page

## Goal

Track citation additions and removals on a high-traffic medical Wikipedia page to detect sourcing changes over time.

## Steps

### 1. Analyze a medical page

```bash
wikihistory analyze --page "COVID-19" --limit 100 --output ./covid-citations.json
```

### 2. Filter citation events

```bash
wikihistory claim --input ./covid-citations.json --type citation_added,citation_removed --output ./citations-only.json
```

### 3. Track citation churn

TODO: Use `wikihistory export` to produce a churn report showing which citations appear, disappear, and reappear across revisions.

## Use case: identifying biased sourcing

TODO: Describe how citation churn patterns can reveal coordinated editing or shifts in cited evidence.

## TODO

- Add churn analysis script or workflow
- Add example of a citation being added then removed in a subsequent edit
- Add section on grouping citations by source type (journal, news, government)
- Add note about the L1/L2 boundary — Varia tracks the citation change, not the quality of the source
