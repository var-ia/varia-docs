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
