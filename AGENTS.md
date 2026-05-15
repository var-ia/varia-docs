# Agent instructions for varia-docs

Varia-docs is the public documentation site for the Varia project — an open-source deterministic observation engine for public revision histories.

## Repository structure

```
README.md                     # Landing page
AGENTS.md                     # This file
docs/
  index.md                    # Homepage
  quickstart.md               # Quick start guide
  concepts.md                 # Architecture concepts
  schema.md                   # Event schema reference
  cli.md                      # CLI command reference
  sdk.md                      # SDK / package reference
  boundary.md                 # What Varia is and is not
  tutorials/
    wikipedia-history.md      # Tutorial: Track Wikipedia changes
    fandom-canon.md           # Tutorial: Track Fandom canon changes
    citation-churn.md         # Tutorial: Monitor citation churn
    dispute-timeline.md       # Tutorial: Build a dispute timeline
```

## Documentation principles

- **Tagline**: "Varia answers: 'What changed?'"
- **Boundary statement**: "Varia does not decide what is true. Varia makes visible how public knowledge changes."
- **Open source framing**: "What changed?" for what Varia does. "Not included: Domain-specific decision judgment." for what Varia excludes.
- **Tone**: technical, neutral, precise. No marketing language.
- **Tutorials**: markdown with TODO notes for incomplete sections.
- **CLI examples**: always use `wikihistory` as the command name.

## Copy conventions

- Use `Varia` (capital V) as the project name.
- Reference packages as `@var-ia/<name>`.
- Reference the CLI as `wikihistory`.
- Describe Varia as a deterministic observation engine.
- L1/L2/L3 references are legacy — prefer "deterministic" / "ground truth".

## Build

```bash
node build.mjs      # builds static HTML from docs/ → dist/
```
