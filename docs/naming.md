# Naming conventions

## The split explained

| Name | What it is | Why |
|---|---|---|
| **Refract** | The project | Sequence of observed changes |
| **var-ia** | GitHub organization | `sequent` was taken as a GitHub org; `var-ia` was available |
| **`@refract-org/*`** | npm package scope | All lowercase by npm convention |
| **`wikihistory`** | CLI command verb | What you do: analyze wiki revision history |

## Why `wikihistory` and not `sequent`?

The CLI is named after the action it performs, not the project it belongs to. When you type `wikihistory analyze "Bitcoin"`, the meaning is self-evident: analyze the wiki history of Bitcoin. `sequent analyze` would require prior knowledge of what Refract is.

## In code

- Use `Refract` (capital S) as the project name in prose
- Use `@refract-org/<name>` when referencing packages
- Use `wikihistory` when referencing the CLI command
