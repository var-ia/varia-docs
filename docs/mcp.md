# MCP — model context protocol

`wikihistory mcp` starts a JSON-RPC server over stdio that exposes the Varia engine to AI agents via the Model Context Protocol.

## Usage

```bash
wikihistory mcp
```

## Connecting an AI agent

### Claude Desktop

```json
{
  "mcpServers": {
    "varia": {
      "command": "npx",
      "args": ["wikihistory", "mcp"]
    }
  }
}
```

### Cline / VS Code

```json
{
  "mcpServers": {
    "varia": {
      "command": "bunx",
      "args": ["wikihistory", "mcp"]
    }
  }
}
```

## Available tools

### `analyze`

Analyze a MediaWiki page's full edit history.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `page` | string | required | Page title |
| `depth` | enum | | `brief`, `detailed`, `forensic` |
| `api` | string | | MediaWiki API base URL |
| `from` | string | | Start revision ID |
| `to` | string | | End revision ID |
| `since` | string | | Re-observe from ISO timestamp |

### `claim`

Track a specific claim's provenance across revisions.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `page` | string | required | Page title |
| `text` | string | required | Claim text to track |

### `export`

Export page analysis as structured JSON.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `page` | string | required | Page title |

### `cron`

Re-observe pages for scheduled monitoring.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `pagesFile` | string | required | File path with one page title per line |
| `interval` | string | | Lookback window in hours |

## Protocol

- **Transport:** stdio (stdin for requests, stdout for responses)
- **Format:** JSON-RPC 2.0
- **Initialization:** Send `tools/list` to discover tools, `tools/call` to invoke
