# Boar blockchain MCP

**Free, keyless MCP server with 65 read-only blockchain tools across Bitcoin, Ethereum, and Mezo. No local installation required.**

![Remote MCP](https://img.shields.io/badge/Remote_MCP-blue)
![No API Key](https://img.shields.io/badge/No_API_Key-green)
![Read-Only](https://img.shields.io/badge/Read--Only-brightgreen)
![65 Tools](https://img.shields.io/badge/65_Tools-orange)
![3 Chains](https://img.shields.io/badge/Bitcoin_·_Ethereum_·_Mezo-purple)

| Registry | Basic | Advanced |
|----------|-------|----------|
| Glama.ai | [boar-blockchain-mcp-basic](https://glama.ai/mcp/connectors/network.boar.mcp/boar-blockchain-mcp-basic) | [boar-blockchain-mcp-advanced](https://glama.ai/mcp/connectors/network.boar.mcp/boar-blockchain-mcp-advanced) |
| Smithery.ai | [![smithery badge](https://smithery.ai/badge/boar-network/blockchain-basic)](https://smithery.ai/servers/boar-network/blockchain-basic) | [![smithery badge](https://smithery.ai/badge/boar-network/blockchain-advanced)](https://smithery.ai/servers/boar-network/blockchain-advanced) |
| mcp.so | [basic](https://mcp.so/server/boar-blockchain-mcp/boar-network) | [advanced](https://mcp.so/server/boar-blockchain-mcp/boar-network) |
| Cursor Directory | [boar-blockchain-mcp](https://cursor.directory/plugins/boar-blockchain-mcp) | [boar-blockchain-mcp](https://cursor.directory/plugins/boar-blockchain-mcp) |

## 🚀 Quick Start

No install. No API key. Just add the URL to your MCP client.

### Claude Desktop

Add to your config file (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

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

### Claude Code

```
/plugin marketplace add boar-network/blockchain-mcp
/plugin install boar-blockchain-mcp@boar-network/blockchain-mcp
```

Or via CLI:

```bash
claude mcp add boar-blockchain-mcp-basic --transport http --scope project https://mcp.boar.network/basic
claude mcp add boar-blockchain-mcp-advanced --transport http --scope project https://mcp.boar.network/advanced
```

### Cursor

Add to `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "boar-blockchain-mcp-basic": {
      "url": "https://mcp.boar.network/basic"
    },
    "boar-blockchain-mcp-advanced": {
      "url": "https://mcp.boar.network/advanced"
    }
  }
}
```

### OpenCode

Add to `opencode.json` in your project root:

```json
{
  "mcp": {
    "boar-blockchain-mcp-basic": {
      "type": "remote",
      "url": "https://mcp.boar.network/basic"
    },
    "boar-blockchain-mcp-advanced": {
      "type": "remote",
      "url": "https://mcp.boar.network/advanced"
    }
  }
}
```

More clients: [VS Code](clients/vscode-copilot.md) · [Windsurf](clients/windsurf.md) · [Gemini CLI](clients/gemini-cli.md) · [OpenCode](clients/opencode.md) · [All setup guides](clients/)

## 💡 What Can You Do?

Once connected, try these prompts:

- **"What is vitalik.eth's ETH balance?"** — resolves ENS name, then fetches balance
- **"Show me the UTXOs for 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa"** — Satoshi's address, Bitcoin UTXO set
- **"Why did transaction 0x... fail?"** — fetches receipt, decodes revert reason
- **"What ERC-20 tokens does 0x... hold?"** — gets token balance with symbol and decimals
- **"Batch-read 5 contract storage values in one call"** — Multicall3 batched reads

See the full [Prompt Cookbook](docs/prompt-cookbook.md) for 6 detailed agent workflows.

## 🔗 Endpoints

| Endpoint | URL | Tools | Best For |
|----------|-----|-------|----------|
| **Basic** | `https://mcp.boar.network/basic` | 46 | Balance checks, transaction lookups, block data, token info, ENS, ABI fetch |
| **Advanced** | `https://mcp.boar.network/advanced` | 19 | Contract calls, ABI decoding, revert analysis, calldata encoding, Multicall3 |

Start with `/basic`. Add `/advanced` when you need smart contract interaction or transaction debugging. [Which should I use?](docs/basic-vs-advanced.md)

## 🛠️ Top Tools at a Glance

| Tool | Chain | What It Does |
|------|-------|--------------|
| `eth_get_balance` | Ethereum | Get ETH balance (wei + ether) |
| `eth_resolve_ens` | Ethereum | Resolve ENS name to address |
| `eth_get_token_balance` | Ethereum | ERC-20 balance with symbol and decimals |
| `eth_get_transaction_receipt` | Ethereum | Transaction receipt with status and logs |
| `eth_get_abi` | Any EVM | Fetch verified ABI from Sourcify |
| `btc_get_balance` | Bitcoin | Confirmed + unconfirmed balance (satoshis) |
| `btc_get_utxos` | Bitcoin | Unspent transaction outputs |
| `mezo_get_balance` | Mezo | Native token balance on Mezo |
| `eth_call` | Ethereum | Read-only smart contract call |
| `eth_multicall` | Ethereum | Batch contract reads via Multicall3 |

Full tool reference: [Basic tools (46)](docs/basic-tools.md) · [Advanced tools (19)](docs/advanced-tools.md)

## ⛓️ Chains Supported

| Chain | Network | Tools |
|-------|---------|-------|
| Bitcoin | Mainnet | 7 |
| Bitcoin | Testnet3 | 7 |
| Ethereum | Mainnet | 14 |
| Mezo | Mainnet | 9 |
| Mezo | Testnet | 9 |
| Cross-chain | — | 4 (selector lookup, ABI fetch, ENS forward + reverse) |
| Advanced (ETH + Mezo + Mezo testnet) | — | 19 (contract calls, decoders, encoding, multicall) |

**Mezo** is a Bitcoin-first blockchain built on Cosmos SDK with EVM compatibility. All Ethereum EVM tools have Mezo equivalents — same interface, different chain. Mezo testnet mirrors mainnet with full tool parity (`mezo_testnet_` prefix), following the same mainnet/testnet split as Bitcoin.

## ⚙️ How It Works

Boar blockchain MCP is a remote MCP server running on Cloudflare Workers' global edge network. Every request is stateless — no sessions, no stored data. All tools are strictly read-only: no transaction signing, no wallet access, no state mutation. Your AI agent connects over HTTPS using Streamable HTTP transport (with SSE fallback).

## ❓ FAQ

**Is this really free?**
Yes. No API key, no account, no payment. Fair use applies.

**Is it safe?**
All tools are read-only. The server cannot send transactions, access private keys, or modify any blockchain state. It returns publicly available data only.

**What MCP transport does it use?**
Streamable HTTP with SSE fallback. All modern MCP clients support this.

**Can I use both endpoints at the same time?**
Yes. Configure them as two separate MCP servers in your client. See [Basic vs Advanced](docs/basic-vs-advanced.md).

**Does it support Bitcoin testnet?**
Yes. All 7 Bitcoin tools have `btc_testnet_` equivalents for testnet3.

**Does it support Mezo testnet?**
Yes. All 9 basic and 6 advanced Mezo tools have `mezo_testnet_` equivalents.

**Who built this?**
[Boar Network](https://boar.network).

## 📚 Documentation

- [Basic Tools Reference (46 tools)](docs/basic-tools.md)
- [Advanced Tools Reference (19 tools)](docs/advanced-tools.md)
- [Basic vs Advanced Guide](docs/basic-vs-advanced.md)
- [Prompt Cookbook — 6 Agent Workflows](docs/prompt-cookbook.md)

## 📖 Setup Guides

- [Claude Desktop](clients/claude-desktop.md)
- [Claude Code](clients/claude-code.md)
- [Cursor](clients/cursor.md)
- [VS Code (GitHub Copilot)](clients/vscode-copilot.md)
- [Windsurf](clients/windsurf.md)
- [Gemini CLI](clients/gemini-cli.md)
- [OpenCode](clients/opencode.md)

## 📋 Policies

- [Security](SECURITY.md)
- [Privacy](PRIVACY.md)
- [Support](SUPPORT.md)
- [Changelog](CHANGELOG.md)
- [License](LICENSE) (MIT)
