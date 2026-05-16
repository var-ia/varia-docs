///
/// # Refract Analysis — {{PAGE_TITLE}}
///
/// This notebook loads events from `{{PAGE_TITLE}}` and visualizes the
/// claim lifecycle, citation churn, and event distribution.
///
/// ## Setup
///
/// ```bash
/// pip install refract-py pandas altair
/// refract export "{{PAGE_TITLE}}" --format ndjson --flatten > events.csv
/// ```
///
/// ## Load events

```python
import pandas as pd
import json

events = []
with open("events.csv") as f:
    for line in f:
        if line.strip() and not line.startswith("#"):
            events.append(json.loads(line))

df = pd.json_normalize(events)
df["timestamp"] = pd.to_datetime(df["timestamp"])
len(df)
```

/// ## Event type distribution

```python
import altair as alt

dist = df["eventType"].value_counts().reset_index()
dist.columns = ["event_type", "count"]
alt.Chart(dist).mark_bar().encode(
    x="event_type", y="count", color="event_type"
)
```

/// ## Citation churn over time

/// Filter to citation events and count by month.

```python
citation_df = df[df["eventType"].str.contains("citation")]
citation_df["month"] = citation_df["timestamp"].dt.to_period("M")
churn = citation_df.groupby(["month", "eventType"]).size().reset_index(name="count")
alt.Chart(churn).mark_line().encode(
    x="month:T", y="count:Q", color="eventType:N"
)
```

/// ## Claim stability scores

/// Events with high revert/edit-cluster counts indicate contested claims.
/// This chart shows the relative frequency of contested vs stable events.

```python
contested = ["revert_detected", "edit_cluster_detected", "sentence_removed"]
df["is_contested"] = df["eventType"].isin(contested)
stable_count = (~df["is_contested"]).sum()
contested_count = df["is_contested"].sum()

pd.DataFrame({"category": ["Stable", "Contested"], "count": [stable_count, contested_count]})
```
