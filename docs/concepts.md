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

## L2 — Model Adapter Providers

Varia supports multiple model providers without coupling to any one:
- **OpenAI** — GPT-4, GPT-4o
- **Anthropic** — Claude 3.5 Sonnet, Claude 3 Opus
- **DeepSeek** — DeepSeek V2
- **Ollama (local)** — llama.cpp-based models running on your own machine
- **BYOK** — Bring your own key to any OpenAI-compatible endpoint

## Confidence Scores

Every L2 interpretation carries a `confidence` score (0.0–1.0). The system supports calibration correction to map raw model confidence to empirical accuracy via bin-based calibration.

## Cascading & Consensus

- **CascadingRouter** — tries a primary adapter, falls back to cheaper/secondary if confidence is below threshold
- **ConsensusAdapter** — runs multiple adapters, picks the majority interpretation
- **CalibratedAdapter** — wraps any adapter with calibration bin correction

## L3 — Ground Truth Sources

Independent ground truth can come from:
- Talk page RFC closures
- Arbitration Committee (ArbCom) decisions
- Page protection events logged by administrators
- Published corrections and retractions
- External reliable source assessments

## Design Principles

1. **No single accuracy score** conflates layers. L1 accuracy is 100% by construction. L2 is measured per-adapter. L3 is independently sourced.
2. **Every interpretation** carries a confidence score. No unconfident output escapes L2.
3. **Deterministic facts** are always presented before interpretations. Consumers see the observable evidence before the model's reading of it.
4. **Layers are independently testable.** You can evaluate L2 without L3, and L3 without L2.

## Event Model

All L1 output is structured as typed events:

- **Claim events**: claim_first_seen, claim_removed, claim_softened, claim_strengthened, claim_reworded, claim_moved, claim_reintroduced
- **Citation events**: citation_added, citation_removed, citation_replaced
- **Template events**: template_added, template_removed, template_parameter_changed
- **Revert/conflict events**: revert_detected, edit_cluster_detected
- **Section/page events**: section_reorganized, lead_promotion, lead_demotion, page_moved
- **Link/category events**: wikilink_added, wikilink_removed, category_added, category_removed
- **Protection events**: protection_changed
- **Content conflict events**: revert_detected, edit_cluster_detected
- **Talk page events**: talk_page_correlated, talk_thread_opened, talk_thread_archived, talk_reply_added, talk_activity_spike

See [schema.md](schema.md) for the full reference.
