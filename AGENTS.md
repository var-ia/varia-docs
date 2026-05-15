# Agent instructions for sequent-docs

Sequent-docs is the public documentation site for the Sequent project — an open-source deterministic observation engine for public revision histories.

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
  boundary.md                 # What Sequent is and is not
  tutorials/
    wikipedia-history.md      # Tutorial: Track Wikipedia changes
    fandom-canon.md           # Tutorial: Track Fandom canon changes
    citation-churn.md         # Tutorial: Monitor citation churn
    dispute-timeline.md       # Tutorial: Build a dispute timeline
```

## Documentation principles

- **Tagline**: "Sequent answers: 'What changed?'"
- **Boundary statement**: "Sequent does not decide what is true. Sequent makes visible how public knowledge changes."
- **Open source framing**: "What changed?" for what Sequent does. "Not included: Domain-specific decision judgment." for what Sequent excludes.
- **Tone**: technical, neutral, precise. No marketing language.
- **Tutorials**: markdown with TODO notes for incomplete sections.
- **CLI examples**: always use `wikihistory` as the command name.

## Copy conventions

- Use `Sequent` (capital V) as the project name.
- Reference packages as `@var-ia/<name>`.
- Reference the CLI as `wikihistory`.
- Describe Sequent as a deterministic observation engine.
- L1/L2/L3 references are legacy — prefer "deterministic" / "ground truth".

## Build

```bash
node build.mjs      # builds static HTML from docs/ → dist/
```
