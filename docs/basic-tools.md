# Basic Tools Reference

**Endpoint:** `https://mcp.boar.network/basic` — 46 tools

All tools are read-only. No authentication required.

---

## Ethereum (14 tools)

### `eth_get_balance`

Get native token balance for an address. Returns balance in wei (hex and decimal) and in ether.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Ethereum address (0x-prefixed) |

### `eth_get_block`

Get block data by number or tag. Returns full block object with transaction hashes.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `block` | string | Yes | Block number as hex (e.g. `"0x1"`) or tag (`"latest"`, `"earliest"`, `"pending"`) |

### `eth_get_transaction`

Get transaction details by hash. Returns full transaction object including from, to, value, input data, and status.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hash` | string | Yes | Transaction hash (0x-prefixed, 32 bytes) |

### `eth_get_transaction_receipt`

Get transaction receipt by hash. Returns status (`0x1` = success, `0x0` = fail), gasUsed, contractAddress (for deployments), and event logs.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hash` | string | Yes | Transaction hash (0x-prefixed, 32 bytes) |

### `eth_get_logs`

Get event logs matching a filter. Requires at least fromBlock and toBlock.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromBlock` | string | Yes | Start block (hex or tag) |
| `toBlock` | string | Yes | End block (hex or tag) |
| `address` | string | No | Contract address to filter logs |
| `topics` | array | No | Topic filters. Each element: topic hash, null (wildcard), or array of hashes (OR match) |

### `eth_get_code`

Get bytecode at an address. Returns `"0x"` for EOAs (wallets), non-empty hex for contracts.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Ethereum address (0x-prefixed) |
| `block` | string | No | Block to query (default: `"latest"`) |

### `eth_gas_price`

Get current gas price in wei. No parameters.

### `eth_estimate_gas`

Estimate gas required for a transaction.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | Yes | Destination address (0x-prefixed) |
| `from` | string | No | Sender address |
| `value` | string | No | Value in wei (hex) |
| `data` | string | No | ABI-encoded call data |

### `eth_get_token_balance`

Get ERC-20 token balance for a wallet. Returns raw balance, formatted balance with decimals, and token symbol.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | Yes | ERC-20 token contract address |
| `wallet` | string | Yes | Wallet address to check |

### `eth_get_token_info`

Get ERC-20 token metadata: name, symbol, decimals, and total supply. Degrades gracefully — any individual field failure returns null.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | Yes | ERC-20 token contract address |

### `eth_lookup_selector`

Look up a 4-byte function or error selector against 4byte.directory. Returns matching function/error signatures.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `selector` | string | Yes | 4-byte selector (0x-prefixed, e.g. `0x70a08231`) |

### `eth_get_abi`

Fetch a verified contract's ABI from Sourcify. Returns full ABI array and match type (`full`, `partial`, or `none`). Defaults to Ethereum mainnet.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Contract address |
| `chain_id` | number | No | EVM chain ID (default: 1 for Ethereum mainnet) |

### `eth_resolve_ens`

Resolve an ENS name to its Ethereum address. Returns null if not registered.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | ENS name (e.g. `"vitalik.eth"`) |

### `eth_lookup_address`

Reverse-resolve an Ethereum address to its primary ENS name. Returns null if no primary name is set.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Ethereum address (0x-prefixed) |

---

## Mezo (9 tools)

Mezo is a Bitcoin-first blockchain built on Cosmos SDK with EVM compatibility. These tools mirror the Ethereum tools with a `mezo_` prefix.

### `mezo_get_balance`

Get native token balance for an address on Mezo. Returns balance in wei (hex and decimal) and in ether.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Address (0x-prefixed) |

### `mezo_get_block`

Get block data by number or tag on Mezo.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `block` | string | Yes | Block number as hex or tag |

### `mezo_get_transaction`

Get transaction details by hash on Mezo.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hash` | string | Yes | Transaction hash (0x-prefixed, 32 bytes) |

### `mezo_get_transaction_receipt`

Get transaction receipt on Mezo. Returns status, gasUsed, contractAddress, and logs.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hash` | string | Yes | Transaction hash (0x-prefixed, 32 bytes) |

### `mezo_get_logs`

Get event logs matching a filter on Mezo.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fromBlock` | string | Yes | Start block (hex or tag) |
| `toBlock` | string | Yes | End block (hex or tag) |
| `address` | string | No | Contract address to filter logs |
| `topics` | array | No | Topic filters |

### `mezo_get_code`

Get bytecode at an address on Mezo. Returns `"0x"` for EOAs.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Address (0x-prefixed) |
| `block` | string | No | Block to query (default: `"latest"`) |

### `mezo_gas_price`

Get current gas price on Mezo in wei. No parameters.

### `mezo_get_token_balance`

Get ERC-20 token balance on Mezo. Same interface as `eth_get_token_balance`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | Yes | ERC-20 token contract address |
| `wallet` | string | Yes | Wallet address to check |

### `mezo_get_token_info`

Get ERC-20 token metadata on Mezo. Same interface as `eth_get_token_info`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | Yes | ERC-20 token contract address |

---

## Mezo Testnet (9 tools)

All Mezo tools have testnet equivalents with a `mezo_testnet_` prefix. Same parameters and behavior, different network. Follows the same mainnet/testnet split as Bitcoin.

| Mainnet Tool | Testnet Tool |
|-------------|-------------|
| `mezo_get_balance` | `mezo_testnet_get_balance` |
| `mezo_get_block` | `mezo_testnet_get_block` |
| `mezo_get_transaction` | `mezo_testnet_get_transaction` |
| `mezo_get_transaction_receipt` | `mezo_testnet_get_transaction_receipt` |
| `mezo_get_logs` | `mezo_testnet_get_logs` |
| `mezo_get_code` | `mezo_testnet_get_code` |
| `mezo_gas_price` | `mezo_testnet_gas_price` |
| `mezo_get_token_balance` | `mezo_testnet_get_token_balance` |
| `mezo_get_token_info` | `mezo_testnet_get_token_info` |

---

## Bitcoin Mainnet (7 tools)

### `btc_get_balance`

Get confirmed and unconfirmed balance for a Bitcoin address. Returns balance in satoshis.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Bitcoin address (P2PKH, P2SH, P2WPKH, P2WSH, or P2TR) |

### `btc_get_history`

Get transaction history for a Bitcoin address. Returns list of transactions with heights and tx hashes.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Bitcoin address |

### `btc_get_utxos`

Get unspent transaction outputs (UTXOs) for a Bitcoin address. Returns tx hash, position, height, and value in satoshis.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `address` | string | Yes | Bitcoin address |

### `btc_get_transaction`

Get full transaction details by txid. Returns verbose data including inputs, outputs, confirmations, and block hash.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `txid` | string | Yes | Transaction ID (64 hex characters) |

### `btc_get_block`

Get block header data by height. Returns hash, version, previous block hash, merkle root, timestamp, bits, and nonce.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `height` | number | Yes | Block height (integer, >= 0) |

### `btc_get_fee_estimate`

Get fee rate estimate for a target number of blocks. Returns BTC/kB and sat/byte. Returns -1 if no estimate available.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `target` | number | No | Target blocks for confirmation (default: 6). Use 1 for next-block, 6 for ~1 hour. |

### `btc_get_mempool_info`

Get mempool fee histogram. Returns `[fee_rate, cumulative_vsize]` pairs showing transaction distribution by fee rate (sat/vB). No parameters.

---

## Bitcoin Testnet3 (7 tools)

All Bitcoin mainnet tools have testnet3 equivalents with a `btc_testnet_` prefix. Same parameters and behavior, different network.

| Mainnet Tool | Testnet Tool |
|-------------|-------------|
| `btc_get_balance` | `btc_testnet_get_balance` |
| `btc_get_history` | `btc_testnet_get_history` |
| `btc_get_utxos` | `btc_testnet_get_utxos` |
| `btc_get_transaction` | `btc_testnet_get_transaction` |
| `btc_get_block` | `btc_testnet_get_block` |
| `btc_get_fee_estimate` | `btc_testnet_get_fee_estimate` |
| `btc_get_mempool_info` | `btc_testnet_get_mempool_info` |
