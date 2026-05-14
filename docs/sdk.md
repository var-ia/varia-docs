# SDK / package reference

## Overview

Varia's SDK is a set of 7 packages that compose into a pipeline: ingest → analyze → persist → interpret.

Packages are published on npm under the `@var-ia` scope. All packages are ESM-only and written in TypeScript.

## Basic pipeline

```typescript
import { MediaWikiClient } from "@var-ia/ingestion";
import { sectionDiffer, citationTracker } from "@var-ia/analyzers";
import type { EvidenceEvent } from "@var-ia/evidence-graph";

const client = new MediaWikiClient({ apiUrl: "https://en.wikipedia.org/w/api.php" });
const revisions = await client.fetchRevisions("Earth");

const events: EvidenceEvent[] = [];
for (let i = 1; i < revisions.length; i++) {
  events.push(...sectionDiffer.diffSections(
    sectionDiffer.extractSections(revisions[i - 1].content),
    sectionDiffer.extractSections(revisions[i].content),
  ));
  events.push(...citationTracker.diffCitations(
    citationTracker.extractCitations(revisions[i - 1].content),
    citationTracker.extractCitations(revisions[i].content),
  ));
}
```

## Storage

```typescript
import { Persistence } from "@var-ia/persistence";

const db = new Persistence({ dbPath: "varia.db" });
await db.insertEvents(events);
const saved = await db.getEvents({ pageTitle: "Earth" });
```

## L2 interpretation

```typescript
import { createAdapter } from "@var-ia/interpreter";
import type { ModelAdapter, InterpretedEvent } from "@var-ia/interpreter";

const adapter: ModelAdapter = createAdapter({
  provider: "openai",
  model: "gpt-4o",
  apiKey: process.env.OPENAI_API_KEY,
});
const interpreted: InterpretedEvent[] = await adapter.interpret(events);
```

Available providers: `openai`, `anthropic`, `deepseek`, `local`, `byok`. Use `local` with `--router` for Ollama-based open-weight models.

## Package reference

### `@var-ia/evidence-graph`

Core types, event schemas, and utilities. Zero runtime dependencies.

```typescript
import type { EvidenceEvent, EventType, Revision } from "@var-ia/evidence-graph";
import { createClaimIdentity, createEventIdentity } from "@var-ia/evidence-graph";
```

Key exports:
- Interfaces: `EvidenceEvent`, `Revision`, `DeterministicFact`, `ModelInterpretation`
- Types: `EventType`, `EvidenceLayer`, `PolicyDimension`, `Depth`
- Utilities: `createClaimIdentity`, `createEventIdentity`
- Merkle tree: `createReplayManifest`, `buildMerkleTree`, `getMerkleProof`, `verifyMerkleProof`

### `@var-ia/ingestion`

Wikimedia API adapters. Fetches revision history and parses wikitext.

```typescript
import { MediaWikiClient } from "@var-ia/ingestion";
import type { RevisionFetcher, AuthConfig } from "@var-ia/ingestion";

const client = new MediaWikiClient({ apiUrl: "https://en.wikipedia.org/w/api.php" });
const revisions = await client.fetchRevisions("Earth");
```

Key exports: `MediaWikiClient` (class), `RevisionFetcher` (interface), `AuthConfig`

**Wikidata entity mapping** (new):

```typescript
import { fetchWikidataId, mapPageToEntity, mapPagesToEntities } from "@var-ia/ingestion";
import type { PageToEntityMap, WikidataEntity, WikidataClaim } from "@var-ia/ingestion";

const qid = await fetchWikidataId("Douglas_Adams"); // "Q42"
const mapping = await mapPageToEntity("Douglas_Adams"); // { pageTitle, qid, entity }
```

Key exports: `fetchWikidataId`, `fetchWikidataEntity`, `mapPageToEntity`, `mapPagesToEntities`, `wikidataEntityToEvents`

### `@var-ia/analyzers`

Deterministic analyzers for section diffs, citation tracking, revert detection, and template analysis. Exported as lowercase singleton instances — no construction needed.

```typescript
import { sectionDiffer, citationTracker, revertDetector, templateTracker } from "@var-ia/analyzers";
import type { SectionDiffer, CitationTracker, RevertDetector, TemplateTracker } from "@var-ia/analyzers";
```

All L1 analyzers share a common pattern — extract from wikitext, diff across revisions, produce `EvidenceEvent` arrays.

Key exports:
- Instances: `sectionDiffer`, `citationTracker`, `revertDetector`, `templateTracker`, `protectionTracker`
- Builders: `buildSectionLineage`, `buildSourceLineage`, `buildClaimLineage`, `buildWikilinkEvents`, `buildPageMoveEvents`, `buildTalkThreadEvents`, `buildCategoryEvents`, `buildParamChangeEvents`
- Classifiers: `classifyHeuristic`, `classifyClaimChange`
- Parsers: `sanitizeWikitext`, `extractHeadingMap`, `extractWikilinks`, `extractCategories`, `countCitations`, `countKeywordMentions`, `deriveSectionHeading`
- Cross-revision: `correlateTalkRevisions`, `diffObservations`, `parseTalkThreads`, `diffTalkThreads`, `diffTemplateParams`, `diffCategories`, `diffWikilinks`
- Clusters & activity: `detectEditClusters`, `detectTalkActivitySpikes`

### `@var-ia/interpreter`

Pluggable model adapter interface for L2. Provider-agnostic factory pattern.

```typescript
import { createAdapter } from "@var-ia/interpreter";
import type { ModelAdapter, ModelConfig, InterpretedEvent, LineageContext } from "@var-ia/interpreter";

const adapter = createAdapter({ provider: "openai", apiKey: "...", model: "gpt-4o" });
```

Key exports: `createAdapter`, `ModelAdapter` (interface), `ModelConfig`, `InterpretedEvent`, `LineageContext`, `CalibratedAdapter`, `CascadingRouter`, `ConsensusAdapter`, `ModelRouter`

### `@var-ia/cli`

The `wikihistory` CLI tool (9 commands: analyze, claim, export, visualize, watch, cron, diff, eval, mcp). See [CLI reference](./cli).

### `@var-ia/persistence`

SQLite storage adapter (uses `bun:sqlite`).

```typescript
import { Persistence } from "@var-ia/persistence";

const db = new Persistence({ dbPath: "varia.db" });
await db.insertEvents(events);
const events = await db.getEvents({ pageTitle: "Earth" });
```

Key exports: `Persistence` (class), `PersistenceAdapter` (interface), `PersistenceConfig`

### `@var-ia/eval`

Evaluation harness for measuring analyzer and model accuracy against L3 ground truth labels.

```typescript
import { createEvalHarness, runL2Benchmark, validateAgainstGroundTruth } from "@var-ia/eval";
```

Key exports: `createEvalHarness`, `runL2Benchmark`, `printBenchmarkResult`, `validateAgainstGroundTruth`, `buildL2Dataset`, `computeCalibration`, `GROUND_TRUTH_LABELS`, `getGroundTruthById`, `getGroundTruthForPage`
