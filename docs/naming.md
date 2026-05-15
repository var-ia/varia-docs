# Naming conventions

## The split explained

| Name | What it is | Why |
|---|---|---|
| **Varia** | The project | Latin for "change" — the engine observes change |
| **var-ia** | GitHub organization | `varia` was taken; `var-ia` was available |
| **`@var-ia/*`** | npm package scope | All lowercase by npm convention |
| **`wikihistory`** | CLI command verb | What you do: analyze wiki revision history |

## Why `wikihistory` and not `varia`?

The CLI is named after the action it performs, not the project it belongs to. When you type `wikihistory analyze "Bitcoin"`, the meaning is self-evident: analyze the wiki history of Bitcoin. `varia analyze` would require prior knowledge of what Varia is.

## In code

- Use `Varia` (capital V) as the project name in prose
- Use `@var-ia/<name>` when referencing packages
- Use `wikihistory` when referencing the CLI command
