# Security

## Credential exposure

CLI flags containing credentials (`--api-key`, `--api-password`) are visible in process listings. Use environment variables instead.

## Local storage

When using `--cache`, revision content is persisted to `~/.wikihistory/varia.db` (SQLite). This file contains full wikitext from every revision fetched. On shared machines, configure `--cache-dir` to an encrypted volume.

## L2 model data flow

When using `--model`, L1-extracted evidence snippets (before/after text from revision boundaries) are sent to the configured model provider's API. Full revision wikitext is never transmitted, but snippets may contain text from private or sensitive wikis. Do not enable L2 interpretation against compliance-sensitive wikis unless the model provider is covered by a data processing agreement.

## Network

Varia makes outbound HTTPS requests to the configured MediaWiki API and model providers. Authentication tokens are sent as `Authorization` or `x-api-key` headers.
