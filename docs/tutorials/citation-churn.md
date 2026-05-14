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

```bash
wikihistory export --input ./citations-only.json --format csv --output ./churn-report.csv
```

This produces a CSV with citation URLs, add/remove timestamps, and revision IDs — ready for spreadsheet analysis.

## Use case: identifying biased sourcing

Citation churn patterns reveal source instability. A citation added in one edit and removed in the next signals contested sourcing. Citation replacement (journal swapped for a news article) indicates editors updating their evidence base. Churn concentrated on one page section may point to a content dispute.

## Example output

```json
{
  "eventId": "cite001a",
  "eventType": "citation_added",
  "fromRevisionId": 1280090010,
  "toRevisionId": 1280090100,
  "section": "Vaccine efficacy",
  "before": "",
  "after": "<ref>{{cite journal |last1=Smith |title=...}}</ref>",
  "timestamp": "2024-11-20T10:00:00Z",
  "layer": "observed"
}
```

```json
{
  "eventId": "cite001b",
  "eventType": "citation_removed",
  "fromRevisionId": 1280090100,
  "toRevisionId": 1280090200,
  "section": "Vaccine efficacy",
  "before": "<ref>{{cite journal |last1=Smith |title=...}}</ref>",
  "after": "",
  "timestamp": "2024-11-21T14:30:00Z",
  "layer": "observed"
}
```

## Notes

- Use `--format csv` to produce spreadsheet-importable churn reports
- Group by source domain to see which types of sources churn most
- Varia tracks the citation change, not source quality — that's outside the boundary
