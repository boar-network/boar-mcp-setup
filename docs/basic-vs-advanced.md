# Choosing Between /basic and /advanced

Boar blockchain MCP exposes two endpoints with **disjoint tool sets** — no overlap between them.

## /basic — 46 tools

Single-RPC tools for straightforward blockchain queries:

- **Balances** — native and ERC-20 token balances
- **Blocks** — block data by number or hash
- **Transactions** — transaction details and receipts
- **Logs** — event log queries with topic filters
- **Gas** — gas price and estimation
- **Contract code** — bytecode retrieval
- **ENS** — name resolution and reverse lookups
- **ABI fetch** — Sourcify verified ABI retrieval, function selector lookups

Covers Bitcoin (mainnet + testnet3), Ethereum, and Mezo (mainnet + testnet).

**Best for:** most users. Handles the majority of blockchain data queries.

## /advanced — 19 tools

Multi-step workflow tools for contract interaction and debugging:

- **Contract calls** — `eth_call` / `mezo_call` / `mezo_testnet_call` with automatic ABI resolution
- **Revert decoding** — human-readable error extraction from failed calls
- **ABI decoders** — decode calldata, return data, and event logs
- **Calldata encoding** — build calldata from function name and parameters
- **Multicall3** — batch multiple read calls into a single request

**Best for:** developers debugging contracts, decoding transactions, or making complex read calls.

## Using Both

The endpoints can be configured as **two separate MCP servers** in your client. There is no conflict since they have completely disjoint tool sets.

## Context Window Considerations

Each endpoint adds its tool descriptions to the LLM context window:

| Endpoint | Tools | Context Impact |
|----------|-------|----------------|
| `/basic` | 46 | Larger footprint |
| `/advanced` | 19 | Smaller footprint |

**Recommendation:** Start with `/basic` to cover common queries. Add `/advanced` as a second server when you need contract interaction or debugging tools. This keeps context usage minimal until the advanced capabilities are needed.
