# Security

## Credential exposure

CLI flags containing credentials (`--api-key`, `--api-password`) are visible in process listings. Use environment variables instead:

| CLI flag | Environment variable |
|----------|---------------------|
| `--api-key` | `VARIA_API_KEY` |
| `--api-user` | `VARIA_API_USER` |
| `--api-password` | `VARIA_API_PASSWORD` |
| `--model-api-key` | `VARIA_MODEL_API_KEY` |

## Local storage

When using `--cache`, revision content is persisted to `~/.refract/cache.db` (SQLite). This file contains full wikitext from every revision fetched. On shared machines, configure `--cache-dir` to an encrypted volume.

The cache directory can also be set via `VARIA_CACHE_DIR` environment variable.

## Network

Refract makes outbound HTTPS requests to the configured MediaWiki API. Authentication tokens are sent as `Authorization` or `x-api-key` headers. All traffic is encrypted in transit.

Bundled evidence files (`--bundle`) are signed but not encrypted — they contain plaintext event data with a SHA-256 hash for integrity verification.

## Data retention

Cached revision data persists indefinitely in `~/.refract/cache.db`. Clear it manually:

```bash
rm -rf ~/.refract/cache.db
```

There is no automatic data retention policy. The cache only contains data you explicitly fetched.
