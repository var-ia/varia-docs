# Event schema reference

## `EventType` — all 25 event types

```typescript
export type EventType =
  // Claim lifecycle
  | "sentence_first_seen"
  | "sentence_removed"
  | "sentence_reintroduced"

  // Citation changes
  | "citation_added"
  | "citation_removed"
  | "citation_replaced"

  // Template changes
  | "template_added"
  | "template_removed"
  | "template_parameter_changed"

  // Section & page structure
  | "section_reorganized"
  | "lead_promotion"
  | "lead_demotion"
  | "page_moved"

  // Links & categories
  | "wikilink_added"
  | "wikilink_removed"
  | "category_added"
  | "category_removed"

  // Access control
  | "protection_changed"

  // Content conflict
  | "revert_detected"
  | "edit_cluster_detected"

  // Talk page correlation
  | "talk_page_correlated"
  | "talk_thread_opened"
  | "talk_thread_archived"
  | "talk_reply_added"
  | "talk_activity_spike";
```

## Core interface: `EvidenceEvent`

All events share a common envelope with `before`/`after` snapshots and
deterministic facts. There are no discriminated subtypes — all fields are
optional at the envelope level:

```typescript
export interface EvidenceEvent {
  eventId?: string;                              // deterministic content hash (see below)
  eventType: EventType;                          // discriminator
  claimId?: string;                              // claim identity hash, when applicable
  fromRevisionId: number;                        // parent revision
  toRevisionId: number;                          // source revision
  section: string;                               // section title where change occurred
  before: string;                                // text / state before the change
  after: string;                                 // text / state after the change
  deterministicFacts: DeterministicFact[];       // facts backing this event
  modelInterpretation?: ModelInterpretation;     // set by downstream consumers
  layer: EvidenceLayer;                          // provenance layer
  timestamp: string;                             // ISO 8601
}
```

## Supporting types

```typescript
export type EvidenceLayer =
  | "observed"              // directly observed from the diff
  | "policy_coded"          // coded against a policy rule
  | "model_interpretation"  // set by downstream consumers (e.g., NextConsensus)
  | "speculative"           // inferred but without a hard rule
  | "unknown";              // layer not yet classified

export interface DeterministicFact {
  fact: string;
  detail?: string;
  provenance?: FactProvenance;
}

export interface FactProvenance {
  analyzer: string;        // analyzer that produced this fact
  version: string;         // analyzer version
  inputHashes: string[];   // hashes of input data used
}

export interface ModelInterpretation {
  semanticChange: string;
  confidence: number;
  policyDimension?: PolicyDimension;
  discussionType?:
    | "notability_challenge"
    | "sourcing_dispute"
    | "neutrality_concern"
    | "content_deletion"
    | "content_addition"
    | "naming_dispute"
    | "procedural"
    | "other";
}

export type PolicyDimension =
  | "verifiability"
  | "npov"
  | "blp"
  | "due_weight"
  | "protection"
  | "edit_warring"
  | "notability"
  | "copyright"
  | "civility";
```

`ModelInterpretation` and `modelInterpretation` on `EvidenceEvent` are never set by Varia's deterministic pipeline — they exist for downstream consumers (e.g., NextConsensus) to attach semantic analysis without modifying the deterministic event.

## Deterministic identity

`eventId` is derived deterministically from the event content so the same
observation always produces the same ID:

`modelInterpretation` is excluded from the hash so downstream consumers can add it without changing the event's deterministic identity.

```typescript
import { createHash } from "node:crypto";

export function createEventIdentity(
  event: Omit<EvidenceEvent, "eventId" | "modelInterpretation">,
): string {
  const factsStr = event.deterministicFacts
    .map((f) => `${f.fact}:${f.detail ?? ""}`)
    .join("|");
  const identityKey =
    `${event.eventType}|${event.fromRevisionId}|${event.toRevisionId}|` +
    `${event.section}|${event.before}|${event.after}|${event.timestamp}|${factsStr}`;
  return createHash("sha256").update(identityKey).digest("hex").slice(0, 16);
}
```

The hash is truncated to 16 hex characters (64 bits of collision resistance).
