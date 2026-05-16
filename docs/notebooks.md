# Notebook analysis

Refract events are standard NDJSON — any notebook environment can load and analyze them. [Marimo](https://marimo.io) and Jupyter both work well.

## Quick start

```bash
# Export events
refract export "Bitcoin" --format ndjson > bitcoin-events.jsonl
```

## Marimo

```python
import marimo as mo
import pandas as pd
import json

# Load events
events = []
with open("bitcoin-events.jsonl") as f:
    for line in f:
        if line.strip() and not line.startswith("#"):
            events.append(json.loads(line))

df = pd.json_normalize(events)
```

### Event type breakdown

```python
df['event_type'].value_counts().plot(kind='bar')
```

### Claim timeline

```python
claim_events = df[df['event_type'].str.startswith('sentence_')]
claim_events['timestamp'] = pd.to_datetime(claim_events['timestamp'])
claim_events.set_index('timestamp').groupby('event_type').resample('ME').size().unstack(fill_value=0).plot()
```

### Citation churn

```python
citation_df = df[df['event_type'].str.contains('citation')]
citation_df['date'] = pd.to_datetime(citation_df['timestamp']).dt.date
citation_df.groupby(['date', 'event_type']).size().unstack(fill_value=0)
```

Marimo's reactivity means changing the input file or event filter automatically updates all charts and tables.

## Jupyter

Same analysis pattern works in Jupyter notebooks. Load JSONL, normalize into a DataFrame, and analyze.

## Validation

```python
print(f"Loaded {len(events)} events")
print(f"Event types: {df['event_type'].nunique()}")
print(f"Date range: {df['timestamp'].min()} to {df['timestamp'].max()}")
```
