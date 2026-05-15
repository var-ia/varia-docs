# Troubleshooting / FAQ

## Rate limits

Refract respects the MediaWiki API's `maxlag` parameter and backs off automatically. If you see `429 Too Many Requests`, wait a few minutes before retrying. Use `-c` / `--cache` to avoid re-fetching pages you've already analyzed.

## "Page too large" errors

Wikipedia pages with thousands of revisions may exceed the default fetch limit. Use `--from <revId> --to <revId>` to scope to a specific range, or increase depth gradually:

```bash
# Start with the last 50 revisions
refract analyze "Earth" --from <recent-rev-id>
```

## Authentication errors

| Error | Likely cause | Fix |
|-------|-------------|-----|
| `401 Unauthorized` | Missing or invalid `--api-key` | Check the API token is correct |
| `403 Forbidden` | Token lacks permission | Verify token scope with wiki admin |
| Connection refused | Wrong API URL | Ensure URL ends in `/api.php` |

## Private wikis

For private or authenticated MediaWiki instances, provide credentials:

```bash
refract analyze "Page" \
  --api https://internal.wiki/api.php \
  --api-key <token>
```

Supported auth methods: bearer token (`--api-key`), basic auth (`--api-user` + `--api-password`), OAuth2.

## Cache issues

If results seem stale, clear the cache:

```bash
rm -rf ~/.refract/cache.db
```

Or use `--cache-dir` to point at a fresh location.

## No events produced

If `refract analyze` returns no events, the page may not have changed in the requested revision range. Try expanding the range or removing `--from`/`--to` to fetch the most recent revisions.

## Cross-wiki diff returns no results

`refract diff` compares a topic across two MediaWiki instances. Each wiki must have a page with the given title. Verify:

```bash
refract analyze "Topic" --api <wiki-a-url>
refract analyze "Topic" --api <wiki-b-url>
```
