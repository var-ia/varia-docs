# Concepts

Varia is a deterministic observation engine. It extracts structured evidence from revision histories without calling any model.

## How it works

```
Wikipedia API → Fetch revisions → Run analyzers → Produce events
```

Every step is byte-for-byte reproducible. Given the same revision range, Varia always produces the same output.

## Pipeline stages

| Stage | What it does | Reproducible? |
|-------|-------------|---------------|
| **Fetch** | Retrieves revisions from the MediaWiki API, handles rate limits, pagination | Yes |
| **Analyze** | Runs deterministic analyzers — section diffs, citation tracking, revert detection, template classification, category/wikilink extraction | Yes |
| **Report** | Assembles structured events with revision, section, and timestamp provenance | Yes |
| **Validate** | Optional: compares pipeline output against ground truth labels (eval package) | Yes |

## Output model

The pipeline produces timed `EvidenceEvent` objects with 25 event types, all mechanical:

- **Sentence events**: `sentence_first_seen`, `sentence_removed`, `sentence_reintroduced`
- **Citation events**: `citation_added`, `citation_removed`, `citation_replaced`
- **Section events**: `section_reorganized`
- **Template events**: `template_added`, `template_removed`, `template_parameter_changed`
- **Revert events**: `revert_detected`
- **Cluster/activity events**: `edit_cluster_detected`, `talk_activity_spike`
- **Page events**: `lead_promotion`, `lead_demotion`, `page_moved`
- **Link/category events**: `wikilink_added`, `wikilink_removed`, `category_added`, `category_removed`
- **Protection events**: `protection_changed`
- **Talk page events**: `talk_page_correlated`, `talk_thread_opened`, `talk_thread_archived`, `talk_reply_added`

See [schema.md](schema.md) for the full reference.

## What Varia does not do

- No model interpretation — semantic analysis of what a change means is handled by downstream systems (e.g., NextConsensus)
- No truth claims — Varia reports what changed, not whether the change is accurate
- No prediction, sentiment analysis, or editor scoring

## Independent ground truth

The eval package stores outcome labels (talk page consensus, RFC closures, ArbCom decisions) independently from pipeline output. These are used for validation and benchmarking, never redefined by pipeline analysis.
