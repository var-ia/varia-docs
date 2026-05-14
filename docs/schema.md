# Event schema reference

## Core type: `EvidenceEvent`

All events share a common envelope:

```typescript
interface EvidenceEvent {
  id: string                          // deterministic content hash
  type: EventType                     // discriminator
  page: string                        // wiki page title
  wiki: string                        // wiki identifier (e.g. "en.wikipedia.org")
  timestamp: string                   // ISO 8601
  revisionId: number                  // source revision
  previousRevisionId: number          // parent revision
  confidence: number                  // 1.0 for L1, 0-1 for L2
  metadata: Record<string, unknown>   // type-specific payload
}
```

## `EventType`

```typescript
type EventType =
  | "section_added"
  | "section_removed"
  | "section_renamed"
  | "citation_added"
  | "citation_removed"
  | "revert_detected"
  | "claim_added"
  | "claim_removed"
  | "claim_modified"
  | "template_added"
  | "template_removed"
  | "template_changed"
```

## Claim events

```typescript
interface ClaimAddedEvent extends EvidenceEvent {
  type: "claim_added";
  metadata: {
    claimText: string;
    sourceCitation?: string;
    sectionTitle: string;
  };
}
```

## Citation events

```typescript
interface CitationAddedEvent extends EvidenceEvent {
  type: "citation_added";
  metadata: {
    citationKey: string;
    citationText: string;
    sectionTitle: string;
    sourceType?: string;
  };
}
```

## Revert events

```typescript
interface RevertDetectedEvent extends EvidenceEvent {
  type: "revert_detected";
  metadata: {
    revertedRevisionId: number;
    revertType: "exact" | "partial";
    matchScore: number;
  };
}
```

## Deterministic identity

Every `EvidenceEvent.id` is a SHA-256 content hash of its type + page + timestamp + metadata. This ensures:

- Same input always produces the same event ID
- Duplicate detection is trivial
- Events can be compared across systems by ID alone
