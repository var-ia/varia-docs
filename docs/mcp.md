# MCP — model context protocol

`wikihistory mcp` starts a JSON-RPC server over stdio that exposes the Refract engine to AI agents via the Model Context Protocol.

The MCP server supports **sampling** — it can request the host's LLM to interpret events without managing API keys or model SDKs internally.

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

Track a specific sentence across revisions.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `page` | string | required | Page title |
| `text` | string | required | Sentence text to track (partial match) |

### `export`

Export page analysis as structured JSON.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `page` | string | required | Page title |
| `depth` | enum | | `brief`, `detailed`, `forensic` |
| `api` | string | | MediaWiki API base URL |
| `since` | string | | Re-observe from ISO timestamp |

### `cron`

Re-observe pages for scheduled monitoring.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `pagesFile` | string | required | File path with one page title per line |
| `interval` | string | | Lookback window in hours |
| `api` | string | | MediaWiki API base URL |

### `interpret`

Analyze a page and return structured semantic interpretations for every event. Each event gets a semantic change description, confidence score, Wikipedia policy dimension (if applicable), and talk page discussion type.

When the host supports MCP sampling, the server constructs a prompt from the events and requests the host's LLM to fill in the interpretations — no API key needed. If sampling is unavailable, returns the prompt text so the host can pass it to any external LLM.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `page` | string | required | Page title |
| `depth` | enum | | `brief`, `detailed`, `forensic` |
| `api` | string | | MediaWiki API base URL |
| `from` | string | | Start revision ID |
| `to` | string | | End revision ID |
| `since` | string | | Re-observe from ISO timestamp |
| `mode` | enum | | `auto` — try host LLM, fall back to prompt; `prompt` — return prompt only; `interpret` — require host LLM |

## Protocol

- **Transport:** stdio (stdin for requests, stdout for responses)
- **Format:** JSON-RPC 2.0 (protocol version `2025-06-18`)
- **Capabilities:** `tools`, `sampling`
- **Initialization:** Send `tools/list` to discover tools, `tools/call` to invoke

## Bring Your Own Inference

Refract never calls a model. The MCP server's `interpret` tool uses
host-provided sampling — the AI application's own LLM fills in the
interpretation. This means:

- No API key configuration needed in Refract
- The same Refract server works with any model (Claude, GPT, local Ollama)
- Interpretations are bounded by Refract's schema (never modify deterministic events)
- MCP `sampling/createMessage` requests are handled asynchronously so the server
  can continue processing while waiting for the host's LLM response
