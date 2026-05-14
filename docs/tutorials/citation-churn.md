# Tutorial: Monitor citation churn on a medical/scientific page

## Goal

Track citation additions and removals on a high-traffic medical Wikipedia page to detect sourcing changes over time.

## Steps

### 1. Analyze a medical page

```bash
wikihistory analyze "COVID-19" --depth detailed -c
```

### 2. Export citation data as CSV

```bash
wikihistory export "COVID-19" --format csv
```

This produces a CSV with citation URLs, add/remove timestamps, and revision IDs — ready for spreadsheet analysis.

### 3. Pipe to file

```bash
wikihistory export "COVID-19" --format csv > churn-report.csv
```

## Use case: identifying biased sourcing

Citation churn patterns reveal source instability. A citation added in one edit and removed in the next signals contested sourcing. Citation replacement (journal swapped for a news article) indicates editors updating their evidence base. Churn concentrated on one page section may point to a content dispute.

## Example output

```json
{
  "eventId": "d1e3f5a7b9c2048a",
  "eventType": "citation_added",
  "fromRevisionId": 1280090010,
  "toRevisionId": 1280090100,
  "section": "Vaccine efficacy",
  "before": "",
  "after": "<ref>{{cite journal |last1=Smith |title=...}}</ref>",
  "timestamp": "2024-11-20T10:00:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "Citation added in section Vaccine efficacy",
      "provenance": {
        "analyzer": "citation-tracker",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

```json
{
  "eventId": "2b4d6f8a0c1e3059",
  "eventType": "citation_removed",
  "fromRevisionId": 1280090100,
  "toRevisionId": 1280090200,
  "section": "Vaccine efficacy",
  "before": "<ref>{{cite journal |last1=Smith |title=...}}</ref>",
  "after": "",
  "timestamp": "2024-11-21T14:30:00Z",
  "layer": "observed",
  "deterministicFacts": [
    {
      "fact": "Citation removed in section Vaccine efficacy",
      "provenance": {
        "analyzer": "citation-tracker",
        "version": "0.3.1",
        "inputHashes": []
      }
    }
  ]
}
```

## Notes

- Use `--format csv` or `--format ndjson` on `wikihistory export` for spreadsheet-importable churn reports.
- Group by source domain to see which types of sources churn most.
- Varia tracks the citation change, not source quality — that's outside the boundary.
