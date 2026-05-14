# Concepts: Three-Knowledge-Split

Varia separates knowledge into three layers that never mix.

## L1 — Deterministic (observable facts)

Byte-for-byte reproducible analysis of revision histories. No model is ever called at L1. Every output is a function of the input alone.

**What L1 produces:**
- Section additions, removals, renames
- Citation additions and removals
- Revert events (exact, partial)
- Template changes

**Invariant:** L1 never calls a model.

## L2 — Model-Assisted (interpretation)

Receives only L1 evidence (never raw wikitext). Produces structured interpretations with confidence scores.

**What L2 produces:**
- Claim classifications
- Edit summaries
- Intent categorizations

**Invariant:** L2 never sees raw Wikipedia text.

## L3 — Independent Ground Truth

Talk page consensus, RFC closures, ArbCom decisions, published corrections. These are sourced independently and never redefined by L1 or L2.

**Invariant:** L3 is never redefined by L1 or L2.

## Design principles

1. **No single accuracy score** conflates layers.
2. **Every interpretation** carries a confidence score.
3. **Deterministic facts** are always presented before interpretations.
4. **Layers are independently testable.**

## Event model

All L1 output is structured as typed events:

- `section_added`, `section_removed`, `section_renamed`
- `citation_added`, `citation_removed`
- `revert_detected`
- `template_added`, `template_removed`, `template_changed`

See [schema.md](schema.md) for the full reference.
