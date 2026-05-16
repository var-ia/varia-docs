# Downstream integration

Refract produces a deterministic event stream. Downstream systems consume that stream to interpret what the changes mean for their domain.

## Integration surfaces

### 1. Structured events via CLI

```bash
# Export events as NDJSON
refract export "Bitcoin" --format ndjson > bitcoin-events.jsonl

# Export as ObservationReport with Merkle root
refract analyze "Bitcoin" --report > bitcoin-report.json
```

Each event carries `schemaVersion`, `FactProvenance` (analyzer, version, parameters), and an `eventId` (deterministic content hash).

### 2. SDK events via adapter

```typescript
import { buildStructuredEvents, EVENT_SCHEMA_VERSION } from "@refract-org/evidence-graph";
import { sectionDiffer, citationTracker, detectEditClusters } from "@refract-org/analyzers";

const events = buildStructuredEvents(revisions);
// Each event has schemaVersion, FactProvenance with version, parameters
```

### 3. FactProvenance for auditability

Every event's `deterministicFacts[0].provenance` includes:

```json
{
  "analyzer": "section-differ",
  "version": "0.4.0",
  "parameters": { "similarityThreshold": 0.8 }
}
```

When a consumer overrides a threshold, the effective value is in `parameters`.

### 4. Schema versioning

Every event carries `schemaVersion` matching `EVENT_SCHEMA_VERSION`. This prevents silent invalidation of historical observations when `EventType` gains new members.

### 5. Config version pinning

`AnalyzerConfig.$version` is pinned from the CLI version. Downstream systems can prove which configuration was used for a given run.

```bash
refract analyze "Bitcoin" --similarity 0.85
# config.$version = "0.5.1"
# config.section.similarityThreshold = 0.85
```

### 6. ObservationReport for chain-of-custody

```json
{
  "pageTitle": "Bitcoin",
  "observedAt": "2026-05-15T10:00:00Z",
  "revisionRange": { "from": 100, "to": 200 },
  "merkleRoot": "a1b2c3d4...",
  "eventCount": 47,
  "analyzerVersion": "0.5.1"
}
```

The Merkle root is from the replay manifest. A downstream system can verify events match the observation.

## Baseline superiority validation

When validating an integrated signal, supply a Refract event summary to `computeBaselineSuperiority()`. The function adds Refract-derived baselines (revert count, citation flux, edit clusters) that the signal must outperform.

```typescript
const result = computeBaselineSuperiority({
  integratedLeadTimeDays: signal.leadTimeDays,
  mentionCount: snapshot.metrics.mentionCount,
  revertCount: snapshot.metrics.revertCount,
  refractEventSummary: { totalEvents: 47, revertCount: 12 },
});
```

## Principle

Refract's event stream is purely mechanical. All interpretation happens downstream. Refract provides the deterministic record; the consumer provides the judgment.

## What downstream systems build

| Consumer | Builds on Refract |
|----------|-------------------|
| **Healthcare decision intelligence** | Feed structured events into a measurement pipeline that scores claims by clinical truth, ratification, economic stake, and feasibility. Each event carries the exact analyzer thresholds used. |
| **AI training data curation** | Score each claim by revert count, citation churn, talk page correlation, and template dispute history. Include only stable, well-sourced claims in training data. |
| **Provenance-aware RAG** | Enrich each retrieved chunk with its claim history — stable, recently changed, source-fragile, contested. Use the signal to weight or filter results. |
| **Regulatory monitoring** | Run `refract cron` on drug pages, guidelines, and regulatory topics. Alert on citation removal, template disputes, or section reorganization. |
| **Competitive intelligence** | Use `refract diff` to compare how the same topic is framed across wikis (English vs German Wikipedia, Fandom vs independent wiki). Track divergence over time. |
| **Fact-checking** | Given a claim, query its lifecycle — first appearance, source additions, revert history, talk page activity. Return a verifiable provenance timeline. |
| **Academic research** | Export `ObservationReport` with Merkle-verifiable claim histories. Analyze claim stability across topics, time periods, and editorial environments. |
| **Journalism forensics** | Track how a specific claim about a person evolved. Detect coordinated editing, source softening, or removal without replacement. |
| **Fan wiki canon tracking** | Compare the same fictional universe across competing wikis. Detect retcon divergence and measure by how much. |
| **Knowledge graph engineering** | Use `--depth forensic` to capture category and wikilink change events. Build an entity graph that evolves with the public record. |

## Complementary technologies

Refract pairs naturally with these modern tools. The event stream is standard JSON/NDJSON — anything that reads JSON or speaks HTTP can consume it.

| Category | Technology | How they fit |
|----------|-----------|-------------|
| **Vector databases** | Pinecone, Weaviate, pgvector, Chroma | Store claim embeddings alongside stability metadata. Query: "find claims similar to X that are stable and well-sourced." |
| **RAG frameworks** | LangChain, LlamaIndex, Vercel AI SDK | Use Refract's stability/contestation signals as retrieval filters or reranking features. LangChain document transformers can attach claim provenance to each chunk. |
| **Data lakes & query** | DuckDB, Apache Parquet, ClickHouse | Query `refract export --format ndjson` output with SQL. DuckDB can query JSONL files directly: `SELECT event_type, count(*) FROM 'events.jsonl' GROUP BY event_type;` |
| **Streaming** | Apache Kafka, Redpanda, Cloudflare Queues | Feed event streams into real-time claim monitoring pipelines. Each `EvidenceEvent` is a Kafka message with key by claimId for stateful processing. |
| **Visualization** | Observable Framework, Mermaid, D3 | `refract visualize --format mermaid` already produces Mermaid diagrams. Observable Framework has a dedicated `@refract-org/observable` data loader. D3 can read event JSONL directly. |
| **Knowledge graphs** | RDF, SPARQL, Neo4j | Convert `wikilink_added`/`category_added` events into triple statements. Build an evolving entity graph where each edge has a revision timestamp. |
| **Version control** | Git, DVC, LakeFS | The `ObservationReport` and JSONL event files are plain text — commit them to track how claims evolve over observation runs. DVC can manage large event corpora with data versioning. |
| **Model serving** | OpenAI API, DeepSeek API, Ollama, vLLM | Plug any OpenAI-compatible endpoint into `refract classify` at each BYO-inference boundary (revert detection, sentence similarity, edit classification, template signal, activity spike). |
| **Notebooks** | Jupyter, Observable notebooks, Marimo | Load event JSONL into a DataFrame: `pd.read_json("events.jsonl", lines=True)`. Analyze claim stability, citation churn, and edit cluster patterns interactively. |
| **Serverless** | Cloudflare Workers, D1, R2, Queues | Run `refract` via `npx` in a Worker, store structured events in D1, export to R2, queue re-observations. The entire infrastructure is edge-deployable. |
