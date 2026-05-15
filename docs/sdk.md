# SDK / package reference

## Overview

Refract's SDK is a set of packages that compose into a pipeline: ingest → analyze → persist.

Packages are published on npm under the `@refract-org` scope. All packages are ESM-only and written in TypeScript.

## Basic pipeline

```typescript
import { MediaWikiClient } from "@refract-org/ingestion";
import { sectionDiffer, citationTracker } from "@refract-org/analyzers";
import type { EvidenceEvent } from "@refract-org/evidence-graph";

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
import { Persistence } from "@refract-org/persistence";

const db = new Persistence({ dbPath: "refract.db" });
await db.insertEvents(events);
const saved = await db.getEvents({ pageTitle: "Earth" });
```

## Package reference

### `@refract-org/evidence-graph`

Core types, event schemas, and utilities. Zero runtime dependencies.

```typescript
import type { EvidenceEvent, EventType, Revision } from "@refract-org/evidence-graph";
import { createClaimIdentity, createEventIdentity } from "@refract-org/evidence-graph";
```

Key exports:
- Interfaces: `EvidenceEvent`, `Revision`, `DeterministicFact`, `ModelInterpretation`
- Types: `EventType`, `EvidenceLayer`, `PolicyDimension`, `Depth`
- Utilities: `createClaimIdentity`, `createEventIdentity`
- Merkle tree: `createReplayManifest`, `buildMerkleTree`, `getMerkleProof`, `verifyMerkleProof`
- BYOI (bring your own inference):
  - `buildInterpretationPrompt(events, pageTitle)` — format events into a structured prompt for any LLM
  - `parseInterpretationResponse(text)` — parse LLM output back into typed `ModelInterpretation[]`
  - `ModelInterpretationSchema` — JSON Schema for `response_format: json_schema`

### `@refract-org/ingestion`

Wikimedia API adapters. Fetches revision history and parses wikitext.

```typescript
import { MediaWikiClient } from "@refract-org/ingestion";
import type { RevisionFetcher, AuthConfig } from "@refract-org/ingestion";

const client = new MediaWikiClient({ apiUrl: "https://en.wikipedia.org/w/api.php" });
const revisions = await client.fetchRevisions("Earth");
```

Key exports: `MediaWikiClient` (class), `RevisionFetcher` (interface), `AuthConfig`

**Wikidata entity mapping** (new):

```typescript
import { fetchWikidataId, mapPageToEntity, mapPagesToEntities } from "@refract-org/ingestion";
import type { PageToEntityMap, WikidataEntity, WikidataClaim } from "@refract-org/ingestion";

const qid = await fetchWikidataId("Douglas_Adams"); // "Q42"
const mapping = await mapPageToEntity("Douglas_Adams"); // { pageTitle, qid, entity }
```

Key exports: `fetchWikidataId`, `fetchWikidataEntity`, `mapPageToEntity`, `mapPagesToEntities`, `wikidataEntityToEvents`

### `@refract-org/analyzers`

Deterministic analyzers for section diffs, citation tracking, revert detection, and template analysis. Exported as lowercase singleton instances — no construction needed.

```typescript
import { sectionDiffer, citationTracker, revertDetector, templateTracker } from "@refract-org/analyzers";
import type { SectionDiffer, CitationTracker, RevertDetector, TemplateTracker } from "@refract-org/analyzers";
```

All L1 analyzers share a common pattern — extract from wikitext, diff across revisions, produce `EvidenceEvent` arrays.

Key exports:
- Instances: `sectionDiffer`, `citationTracker`, `revertDetector`, `templateTracker`, `protectionTracker`
- Builders: `buildSectionLineage`, `buildSourceLineage`, `buildClaimLineage`, `buildWikilinkEvents`, `buildPageMoveEvents`, `buildTalkThreadEvents`, `buildCategoryEvents`, `buildParamChangeEvents`
- Classifiers: `classifyHeuristic`
- Parsers: `sanitizeWikitext`, `extractHeadingMap`, `extractWikilinks`, `extractCategories`, `countCitations`, `countKeywordMentions`, `deriveSectionHeading`
- Cross-revision: `correlateTalkRevisions`, `diffObservations`, `parseTalkThreads`, `diffTalkThreads`, `diffTemplateParams`, `diffCategories`, `diffWikilinks`
- Clusters & activity: `detectEditClusters`, `detectTalkActivitySpikes`

### `@refract-org/cli`

The `refract` / `wikihistory` CLI tool (10 commands: analyze, claim, cron, diff, eval, explore, export, mcp, visualize, watch). See [CLI reference](./cli).

### `@refract-org/persistence`

SQLite storage adapter (uses `bun:sqlite`).

```typescript
import { Persistence } from "@refract-org/persistence";

const db = new Persistence({ dbPath: "refract.db" });
await db.insertEvents(events);
const events = await db.getEvents({ pageTitle: "Earth" });
```

Key exports: `Persistence` (class), `PersistenceAdapter` (interface), `PersistenceConfig`

### `@refract-org/observable`

Observable Framework data loader for embedding Refract queries in Observable dashboards.

```typescript
import { RefractDataLoader } from "@refract-org/observable";
```

This package is designed for use with [Observable Framework](https://observablehq.com/framework/) data loaders. It is not published to npm — use from source or copy the loader pattern directly.

---

### `@refract-org/eval`

Evaluation harness for measuring analyzer accuracy against ground truth labels.

```typescript
import { createEvalHarness, validateAgainstGroundTruth } from "@refract-org/eval";
```

Key exports: `createEvalHarness`, `validateAgainstGroundTruth`, `EvalHarness`, `GROUND_TRUTH_LABELS`, `getGroundTruthById`, `getGroundTruthForPage`
