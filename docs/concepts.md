# Concepts

Refract is the open claim-history layer for public knowledge. It ingests revision histories
from MediaWiki instances and produces a deterministic event stream showing how claims
enter, change, stabilize, and exit the public record — where they came from, what supported
them, what challenged them, and when context altered their meaning.

Every run against the same revision range produces identical output: byte-for-byte reproducible.

## How determinism works

Refract never calls an LLM, never samples randomly, and never depends on external state. Given revision text from a MediaWiki API, each analyzer applies a pure function: `(wikitext) → events`. The same wikitext always produces the same events. This holds across machines, OS versions, and time.

Deterministic identity (`eventId`) extends this guarantee: events with identical content produce identical hashes, so deduplication and cross-reference are reliable even when the same observation is run independently.

## Architecture

![Concept overview](concept-overview.svg)

The diagram shows how Refract ingests revision histories from MediaWiki APIs, runs deterministic analyzers, and produces a structured event stream with provenance tags.

## Pipeline stages

```
Wikipedia API → Fetch → Analyze → Report → (optional) Validate
```

| Stage | What it does | Reproducible? |
|-------|-------------|---------------|
| **Fetch** | Retrieves revisions from the MediaWiki API, handles rate limits, pagination | Yes — same API + same page → same revisions |
| **Analyze** | Runs deterministic analyzers — section diffs, citation tracking, revert detection, template classification, category/wikilink extraction | Yes — pure functions, no external state |
| **Report** | Assembles structured events with revision, section, and timestamp provenance | Yes — deterministic ID via SHA-256 content hash |
| **Validate** | Optional: compares pipeline output against ground truth labels (eval package) | Yes — deterministic comparison |

![Architecture data flow](architecture-flow.svg)

## Depth levels

The `--depth` flag controls how many analyzers run:

- **`brief`** — section differ only. Quick check for any change.
- **`detailed`** (default) — all standard analyzers (section, citation, revert, template).
- **`forensic`** — includes cross-revision correlation, metadata trackers, and cluster detection. Use for full audit or export.

See [depth reference](depth.md).

## Output model

The pipeline produces timed `EvidenceEvent` objects with 26 event types, all mechanical:

- **Sentence events**: `sentence_first_seen`, `sentence_removed`, `sentence_modified`, `sentence_reintroduced`
- **Citation events**: `citation_added`, `citation_removed`, `citation_replaced`
- **Section events**: `section_reorganized`
- **Template events**: `template_added`, `template_removed`, `template_parameter_changed`
- **Revert events**: `revert_detected`
- **Cluster/activity events**: `edit_cluster_detected`, `talk_activity_spike`
- **Page events**: `lead_promotion`, `lead_demotion`, `page_moved`
- **Link/category events**: `wikilink_added`, `wikilink_removed`, `category_added`, `category_removed`
- **Protection events**: `protection_changed`
- **Talk page events**: `talk_page_correlated`, `talk_thread_opened`, `talk_thread_archived`, `talk_reply_added`

The event schema includes a `modelInterpretation` field and `model_interpretation` evidence layer — these are never set by Refract's deterministic engine. They exist for downstream consumers to attach semantic analysis without modifying the event stream.

Analyzers accept configurable parameters (similarity thresholds, time windows, revert patterns) via `AnalyzerConfig`, passed from the CLI or SDK. This allows users to tune detection sensitivity without modifying analyzer code.

The pipeline can also produce an `ObservationReport` — a structured aggregate that groups events by claim with a `ClaimLedger` tracking each claim's lifecycle across revisions.

See [schema.md](schema.md) for the full reference.

![Evidence labels diagram](evidence-labels.svg)

## Configurable heuristics / BYO-inference boundaries

Every analyzer threshold encodes an interpretive judgment — what counts as a revert, how close edits must be to form a cluster, what sentence similarity means "modified" vs "removed and added." These thresholds accept optional overrides via `AnalyzerConfig`, and the effective parameters are recorded in `FactProvenance.parameters` when non-default values are used.

Each threshold is a **BYO-inference boundary** — a typed function signature where a model can replace the default heuristic:

| Boundary | Default (mechanical) | Pluggable (model) |
|----------|----------------------|--------------------|
| Sentence similarity | Word-overlap ratio (0.8) | "Are these two sentences the same claim?" |
| Revert detection | 6 regex patterns | "Is this edit comment a revert?" |
| Template classification | Name-to-type lookup | "What policy signal does this template represent?" |
| Edit cluster detection | Time window + min size | "Are these edits semantically related?" |
| Heuristic classification | Size thresholds + comment patterns | "What kind of edit is this?" |

The defaults work offline with no configuration required. Consumers supply a model at any boundary, and the event's `FactProvenance.parameters` records the path taken — `similaritySource: "model"` or `"default"` — keeping the audit trail transparent regardless.

This is the architectural spine: Refract stays purely mechanical, but the boundaries where judgment enters are explicit, typed, and versioned. Foundation model improvements automatically benefit consumers without Refract changing.

## Two-knowledge split

Refract separates mechanical observation from human judgment:

1. **Deterministic layer** — what changed, byte-for-byte reproducible. All analyzers, all events.
2. **Ground truth layer** — independently verified outcome labels (talk page consensus, RFC closures, ArbCom decisions) stored in `@refract-org/eval`. Used for validation, never redefined by pipeline output.

Downstream model interpretation is a separate concern — it consumes Refract's event stream without modifying it.

## What Refract does not do

- No model interpretation — semantic analysis of what a change means is handled by downstream systems
- No truth claims — Refract reports what changed, not whether the change is accurate
- No prediction, sentiment analysis, or editor scoring
- No claims about compliance, policy violations, or decision relevance

## Independent ground truth

The eval package stores outcome labels independently from pipeline output. These labels are collected from talk page discussions, RFC closures, and arbitration decisions — human consensus about what happened. The evaluation harness compares pipeline events against these labels to measure analyzer accuracy without feedback loops.
