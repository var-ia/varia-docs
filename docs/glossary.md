# Glossary

**Claim** — A distinct statement of fact within an article, identified by its text content. Claims are tracked across revisions via `createClaimIdentity` in `@refract-org/evidence-graph`. Not to be confused with "claim" in a legal or insurance context.

**Configurable heuristics** — Every analyzer threshold (revert patterns, edit cluster window, talk spike factor, sentence similarity, section rename mode) accepts an optional override via `AnalyzerConfig`. When overridden, the effective parameters are recorded in `FactProvenance.parameters`, making the interpretive choice transparent and auditable.

**Deterministic fact** — A structured statement produced by an analyzer explaining why an event was emitted. Each fact includes the analyzer name, version, input hashes for reproducibility, and optional effective parameters (when non-default config was used).

**Deterministic observation engine** — Refract's core design: given the same revision range, the same events are always produced. No model, no randomness, no variance.

**Downstream consumer** — A system that consumes Refract's event stream without modifying it. NextConsensus is a downstream consumer that attaches model interpretation.

**Edit cluster** — A group of 3+ edits within a configurable time window, detected by `detectEditClusters()`. High cluster activity signals concentrated contestation — the same claim being edited repeatedly by one or more editors. Window size and minimum group size are configurable via `AnalyzerConfig`.

**Event** — A unit of change between two revisions. Every event has an `eventType`, revision range (`fromRevisionId` → `toRevisionId`), section context, before/after snapshots, and deterministic facts. See [schema](schema.md).

**Evidence layer** — A classification of how an event was produced: `observed` (directly from diff), `policy_coded` (matched against a rule), `model_interpretation` (set by downstream), `speculative` (inferred), or `unknown`.

**Ground truth** — Independently verified outcome labels (talk page consensus, RFC closures, ArbCom decisions) stored in the `@refract-org/eval` package. Used to validate pipeline output, never redefined by it.

**L1 / L3** — Legacy terminology for the two layers: L1 (deterministic analysis) and L3 (ground truth validation). The preferred terms are "deterministic" and "ground truth."

**MediaWiki** — The open-source wiki engine used by Wikipedia, Fandom, and thousands of other wikis. Refract works with any MediaWiki instance that exposes an `api.php` endpoint.

**ObservationReport** — A structured aggregate produced by the pipeline that groups events by claim, with a `ClaimLedger` tracking each claim's lifecycle (first seen, removed, modified, reintroduced) across revision history. Output via `--report` flag.

**`sentence_modified`** — An event type emitted when sentence text changes but remains above the similarity threshold (word-overlap ratio, default 0.8). Captures rewording, minor edits, and phrasing changes without treating the sentence as removed-and-reintroduced.

**Provenance** — Metadata attached to each deterministic fact recording which analyzer produced it, what version, and which input data was used. Enables auditability.

**Revision** — A single version of a page. Revisions are identified by numeric IDs and ordered by timestamp.

**Revision range** — A contiguous span of revisions between a `fromRevisionId` and `toRevisionId`. Refract diffs adjacent revisions within the range to produce events.
