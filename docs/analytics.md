# Analytics with DuckDB

Refract's event output is newline-delimited JSON (NDJSON). [DuckDB](https://duckdb.org) can query it directly with SQL, no loading step needed.

## Quick start

```bash
# Export events
refract export "Bitcoin" --format ndjson > bitcoin-events.jsonl

# Query with DuckDB
duckdb -c "SELECT event_type, count(*) as cnt
           FROM 'bitcoin-events.jsonl'
           GROUP BY event_type ORDER BY cnt DESC"
```

## Example queries

### Event type distribution

```sql
SELECT event_type, count(*) as cnt
FROM 'bitcoin-events.jsonl'
GROUP BY event_type
ORDER BY cnt DESC;
```

### Citation churn over time

```sql
SELECT strftime(timestamp, '%Y-%m') as month,
       count(*) as citation_events
FROM 'bitcoin-events.jsonl'
WHERE event_type IN ('citation_added', 'citation_removed', 'citation_replaced')
GROUP BY month
ORDER BY month;
```

### Template disputes

```sql
SELECT section, count(*) as template_events
FROM 'bitcoin-events.jsonl'
WHERE event_type = 'template_added'
  AND deterministic_facts[0]->>'fact' = 'template_changed'
GROUP BY section
ORDER BY template_events DESC;
```

## DuckDB setup

```bash
# Install DuckDB
brew install duckdb
# Or: curl -O https://github.com/duckdb/duckdb/releases/download/v1.5.2/duckdb_cli-osx-universal.zip
```

## Validation

```bash
# Count events — should match refract analyze output
duckdb -c "SELECT count(*) FROM 'events.jsonl'"
```

All queries work on native DuckDB with no extensions. Refract NDJSON follows the standard format that DuckDB's JSON scanner reads out of the box.
