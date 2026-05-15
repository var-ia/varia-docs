# Security

## Credential exposure

CLI flags containing credentials (`--api-key`, `--api-password`) are visible in process listings. Use environment variables instead.

## Local storage

When using `--cache`, revision content is persisted to `~/.wikihistory/varia.db` (SQLite). This file contains full wikitext from every revision fetched. On shared machines, configure `--cache-dir` to an encrypted volume.

## Network

Varia makes outbound HTTPS requests to the configured MediaWiki API. Authentication tokens are sent as `Authorization` or `x-api-key` headers.
