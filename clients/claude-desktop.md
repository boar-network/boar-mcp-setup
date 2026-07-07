# Boar blockchain MCP — Claude Desktop Setup

## Config file location

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

## Basic endpoint (recommended start)

Add to your config file (create it if it doesn't exist):

```json
{
  "mcpServers": {
    "boar-blockchain-mcp-basic": {
      "type": "streamable-http",
      "url": "https://mcp.boar.network/basic"
    }
  }
}
```

## Both endpoints

```json
{
  "mcpServers": {
    "boar-blockchain-mcp-basic": {
      "type": "streamable-http",
      "url": "https://mcp.boar.network/basic"
    },
    "boar-blockchain-mcp-advanced": {
      "type": "streamable-http",
      "url": "https://mcp.boar.network/advanced"
    }
  }
}
```

## Verify

1. Restart Claude Desktop (fully quit and reopen).
2. Look for the hammer icon in the chat input area — it should show **46 tools** (basic) or **65 tools** (both).
3. Try a prompt: **"What is the ETH balance of vitalik.eth?"**

## Can't connect? (Team/Enterprise plans)

Some organizations block remote MCP servers on Claude Team or Enterprise subscriptions. If the direct setup above doesn't work, use [`mcp-remote`](https://github.com/geelen/mcp-remote) as a local proxy. It runs a local stdio server that forwards requests to the remote endpoint.

Requires [Node.js](https://nodejs.org/) installed on your machine.

### Basic endpoint via proxy

```json
{
  "mcpServers": {
    "boar-blockchain-mcp-basic": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.boar.network/basic"]
    }
  }
}
```

### Both endpoints via proxy

```json
{
  "mcpServers": {
    "boar-blockchain-mcp-basic": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.boar.network/basic"]
    },
    "boar-blockchain-mcp-advanced": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.boar.network/advanced"]
    }
  }
}
```

After saving, restart Claude Desktop and verify as described above.

## Troubleshooting

- **No hammer icon after restart** — Make sure the JSON is valid (no trailing commas). Use a JSON validator if unsure.
- **Tools appear but calls fail** — Check your internet connection. The server is at `mcp.boar.network` and requires HTTPS.
- **Config file not found** — On macOS, the `Claude` directory may not exist yet. Create it manually, then add the config file.
- **Using `mcp-remote` and tools don't appear** — Make sure Node.js is installed (`node --version`). The first run may take a moment while npx downloads the package.
