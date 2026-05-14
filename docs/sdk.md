# SDK / package reference

## Overview

Varia's SDK is a set of 7 packages that compose into a pipeline: ingest → analyze → persist → interpret.

Packages are published on npm under the `@var-ia` scope. All packages are ESM-only and written in TypeScript.

## Basic pipeline

```typescript
import { MediaWikiClient } from "@var-ia/ingestion";
import { SectionDiffer, CitationTracker, RevertDetector } from "@var-ia/analyzers";
import type { EvidenceEvent } from "@var-ia/evidence-graph";

const client = new MediaWikiClient({ api: "https://en.wikipedia.org/w/api.php" });
const differ = new SectionDiffer();
const citations = new CitationTracker();

// Fetch revisions
const revisions = await client.fetchRevisions({ page: "Earth", limit: 10 });

// Run analysis
const events: EvidenceEvent[] = [];
for (const rev of revisions) {
  events.push(...differ.diff(rev.previous, rev.current));
  events.push(...citations.diff(rev.previous, rev.current));
}
```

## Storage

```typescript
import { EventStore } from "@var-ia/persistence";

const store = new EventStore("varia.db");
await store.insertEvents(events);
const saved = await store.getEvents({ page: "Earth" });
```

## L2 interpretation

```typescript
import { createOpenAIAdapter } from "@var-ia/interpreter/openai";

const adapter = createOpenAIAdapter({
  model: "gpt-4",
  apiKey: process.env.OPENAI_API_KEY,
});
const interpreted = await adapter.interpret(events);
```

## Package reference

### `@var-ia/evidence-graph`

Core types, event schemas, and utilities. Zero runtime dependencies.

```typescript
import type { EvidenceEvent, EventType } from "@var-ia/evidence-graph";
import { createClaimIdentity, hashEvent } from "@var-ia/evidence-graph";
```

Key exports:
- Interface: `EvidenceEvent`
- Types: `EventType`, `EvidenceLayer`, `PolicyDimension`
- Utilities: `createClaimIdentity`, `createEventIdentity`, `compareEvents`

### `@var-ia/ingestion`

Wikimedia API adapters. Fetches revision history and parses wikitext.

```typescript
import { RevisionFetcher } from "@var-ia/ingestion";
const fetcher = new RevisionFetcher({ wiki: "en.wikipedia.org" });
```

Key exports: `RevisionFetcher`, `WikitextParser`, `MediaWikiClient`

### `@var-ia/analyzers`

Deterministic analyzers for section diffs, citation tracking, revert detection, and template analysis.

```typescript
import { SectionDiffer, CitationTracker, RevertDetector, TemplateAnalyzer } from "@var-ia/analyzers";
```

All L1 analyzers share a common interface — receive a pair of revisions and produce an array of `EvidenceEvent`.

Key exports: `SectionDiffer`, `CitationTracker`, `RevertDetector`, `TemplateAnalyzer`

### `@var-ia/interpreter`

Pluggable model adapter interface for L2.

```typescript
import type { ModelAdapter } from "@var-ia/interpreter";
import { createOpenAIAdapter } from "@var-ia/interpreter/openai";
```

Key exports: `ModelAdapter` (interface), `createOpenAIAdapter`

### `@var-ia/cli`

The `wikihistory` CLI tool. Combines ingestion, analysis, and output.

### `@var-ia/persistence`

SQLite storage adapter (uses `bun:sqlite`).

```typescript
import { EventStore } from "@var-ia/persistence";
const store = new EventStore("varia.db");
```

Key exports: `EventStore`

### `@var-ia/eval`

Evaluation harness for measuring analyzer and model accuracy against L3 ground truth.
