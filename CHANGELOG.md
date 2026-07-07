# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.8.0] - 2026-07-07

### Added

- **Mezo testnet support**: full tool parity with Mezo mainnet, exposed as a separate `mezo_testnet_*` tool set — 9 new `/basic` tools and 6 new `/advanced` tools, following the same mainnet/testnet split as Bitcoin
- Total tool count now 65 across the two endpoints: `/basic` (46 tools, up from 37) and `/advanced` (19 tools, up from 13)

## [0.7.0] - 2026-03-31

Initial public release of Boar blockchain MCP — a free, keyless, read-only MCP server for blockchain data.

### Added

- **50 tools** across 2 endpoints (`/basic` and `/advanced`)
- **3 chains**: Bitcoin (mainnet + testnet3), Ethereum, and Mezo
- **Bitcoin tools**: balance, blocks, transactions, UTXOs, fee estimates, address history, mempool info
- **EVM tools**: balance, blocks, transactions, receipts, logs, gas price, gas estimation, contract code
- **ERC-20 token tools**: token balance queries for Ethereum and Mezo
- **ENS resolution**: forward and reverse name lookups on Ethereum
- **ABI utilities**: Sourcify verified ABI fetch, function selector lookups via 4byte.directory
- **Contract interaction**: `eth_call` and `mezo_call` with automatic ABI resolution
- **Revert decoding**: human-readable revert reason extraction for failed calls
- **ABI decoders**: decode calldata, return data, and event logs using verified ABIs
- **Calldata encoding**: encode function calls from human-readable parameters
- **Multicall3**: batch multiple read calls in a single request
- Hosted on Cloudflare Workers at `https://mcp.boar.network`
- No API keys required, no authentication, no user data collection
