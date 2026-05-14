# SDK / package reference

## `@var-ia/evidence-graph`

Core types, event schemas, and utilities. Zero runtime dependencies.

```typescript
import type { EvidenceEvent, EventType } from "@var-ia/evidence-graph";
import { createClaimIdentity, hashEvent } from "@var-ia/evidence-graph";
```

**Exports:**
- Interfaces: `EvidenceEvent`, `ClaimAddedEvent`, `CitationAddedEvent`, `RevertDetectedEvent`
- Types: `EventType`
- Utilities: `createClaimIdentity`, `hashEvent`, `compareEvents`, `mergeEventStreams`

## `@var-ia/ingestion`

Wikimedia API adapters. Fetches revision history and parses wikitext.

```typescript
import { RevisionFetcher } from "@var-ia/ingestion";
const fetcher = new RevisionFetcher({ wiki: "en.wikipedia.org" });
```

Includes: `RevisionFetcher`, `WikitextParser`

## `@var-ia/analyzers`

Deterministic analyzers for section diffs, citation tracking, revert detection, and template analysis.

```typescript
import { SectionDiffer, CitationTracker, RevertDetector, TemplateAnalyzer } from "@var-ia/analyzers";
```

All L1 analyzers share a common interface — receive a pair of revisions and produce an array of `EvidenceEvent`.

## `@var-ia/interpreter`

Pluggable model adapter interface for L2.

```typescript
import type { ModelAdapter } from "@var-ia/interpreter";
import { createOpenAIAdapter } from "@var-ia/interpreter/openai";
```

## `@var-ia/cli`

The `wikihistory` CLI tool. Combines ingestion, analysis, and output.

## `@var-ia/persistence`

SQLite storage adapter (uses `bun:sqlite`).

```typescript
import { EventStore } from "@var-ia/persistence";
const store = new EventStore("varia.db");
```

## `@var-ia/eval`

Evaluation harness for measuring analyzer and model accuracy against L3 ground truth.
