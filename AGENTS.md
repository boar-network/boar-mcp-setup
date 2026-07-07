# Boar Blockchain MCP

This repository is the source of truth for **Boar blockchain MCP** - a free, keyless remote MCP server exposing 65 read-only blockchain tools across Bitcoin, Ethereum, and Mezo (mainnet and testnet). No local installation required.

The server runs on Cloudflare Workers and is available at:
- Basic endpoint (46 tools): `https://mcp.boar.network/basic`
- Advanced endpoint (19 tools): `https://mcp.boar.network/advanced`

All tools are strictly read-only. No transaction signing, no wallet access, no state mutation.

## Repository Structure

- `clients/` - setup guides for Claude Desktop, Claude Code, Cursor, VS Code, Windsurf, Gemini CLI, OpenCode
- `docs/` - tool references, basic vs advanced guide, prompt cookbook
- `.mcp.json` - MCP server configuration for Claude Code (project-level auto-detection)
- `opencode.json` - MCP server configuration for OpenCode (project-level auto-detection)
- `.claude-plugin/` - Claude plugin standard files (see below)
- `.plugin/` - Open Plugin Standard files (see below)

## Plugin Standards

### Claude Plugin Standard

The `.claude-plugin/` directory follows the Claude plugin standard for [Claude Code](https://claude.ai/code) and Claude Desktop.

Relevant files:
- `.claude-plugin/plugin.json` - plugin manifest referencing the MCP server config
- `.claude-plugin/mcp.json` - MCP server configuration (referenced by `plugin.json` via `"mcpServers": "./mcp.json"`)
- `.claude-plugin/marketplace.json` - marketplace manifest enabling installation via `/plugin install`

To install via Claude Code:
```
/plugin install boar-blockchain-mcp@boar-network/blockchain-mcp
```

### OpenCode

The `opencode.json` file in the repo root is auto-detected by [OpenCode](https://opencode.ai) as a project-level MCP config.

Relevant files:
- `opencode.json` - MCP server configuration using `"type": "remote"` and top-level `"mcp"` key (OpenCode's format)
- `clients/opencode.md` - setup guide

### Open Plugin Standard

The `.plugin/` directory follows the [Open Plugin Standard](https://open-plugins.com/).

Relevant files:
- `.plugin/plugin.json` - plugin manifest

## Working with This Repository

- Documentation changes go in `clients/` or `docs/` depending on scope
- When bumping the version, update it consistently across `.plugin/plugin.json`, `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`, and `CHANGELOG.md`
- The MCP server itself is not hosted in this repo - this repo is documentation and plugin configuration only
