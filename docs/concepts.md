# Concepts

Refract is a deterministic observation engine. It extracts structured evidence from revision histories without calling any model. Every run against the same revision range produces identical output — byte-for-byte reproducible.

## How determinism works

Refract never calls an LLM, never samples randomly, and never depends on external state. Given revision text from a MediaWiki API, each analyzer applies a pure function: `(wikitext) → events`. The same wikitext always produces the same events. This holds across machines, OS versions, and time.

Deterministic identity (`eventId`) extends this guarantee: events with identical content produce identical hashes, so deduplication and cross-reference are reliable even when the same observation is run independently.

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

## Depth levels

The `--depth` flag controls how many analyzers run:

- **`brief`** — section differ only. Quick check for any change.
- **`detailed`** (default) — all standard analyzers (section, citation, revert, template).
- **`forensic`** — includes cross-revision correlation, metadata trackers, and cluster detection. Use for full audit or export.

See [depth reference](depth.md).

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

The event schema includes a `modelInterpretation` field and `model_interpretation` evidence layer — these are never set by Refract's deterministic engine. They exist for downstream consumers (e.g., NextConsensus) to attach semantic analysis without modifying the event stream.

See [schema.md](schema.md) for the full reference.

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

## Bring your own inference

Refract provides the format pipeline; you provide the model.

The deterministic engine never calls an LLM, never stores API keys, and never
depends on any inference runtime. But the event schema includes a
`modelInterpretation` field on every `EvidenceEvent` — reserved for downstream
systems to attach semantic analysis without modifying the deterministic record.

Three utilities in `@refract-org/evidence-graph` make this boundary clean:

- **`buildInterpretationPrompt(events, pageTitle)`** — formats a batch of events
  into a structured prompt for any LLM: "classify each event by semantic change,
  confidence, policy dimension, and discussion type"

- **`ModelInterpretationSchema`** — a JSON Schema that any provider supporting
  `response_format: json_schema` (OpenAI, Anthropic, DeepSeek) can use to produce
  valid structured output

- **`parseInterpretationResponse(text)`** — extracts typed `ModelInterpretation[]`
  from raw LLM output with JSON extraction fallback

The MCP server (`wikihistory mcp`) extends this pattern through **MCP sampling**:
it requests the host's LLM to interpret events without managing API keys.
Reference pipelines are in [varia-labs](https://github.com/refract-org/refract-labs) for
DeepSeek, OpenAI, Ollama, and other OpenAI-compatible providers.

## Independent ground truth

The eval package stores outcome labels independently from pipeline output. These labels are collected from talk page discussions, RFC closures, and arbitration decisions — human consensus about what happened. The evaluation harness compares pipeline events against these labels to measure analyzer accuracy without feedback loops.
