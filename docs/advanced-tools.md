# Advanced Tools Reference

**Endpoint:** `https://mcp.boar.network/advanced` — 19 tools

All tools are read-only. No authentication required. These tools handle complex workflows: contract calls, ABI decoding, calldata encoding, and batch reads.

---

## Contract Calls

### `eth_call`

Execute a read-only smart contract call on Ethereum mainnet. Returns hex-encoded return data. Use this to read contract state without sending a transaction.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | Yes | Contract address (0x-prefixed) |
| `data` | string | Yes | ABI-encoded function call data (0x-prefixed) |
| `from` | string | No | Sender address (optional, for context) |
| `block` | string | No | Block to query (default: `"latest"`) |

### `mezo_call`

Execute a read-only smart contract call on Mezo. Same interface as `eth_call`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | Yes | Contract address (0x-prefixed) |
| `data` | string | Yes | ABI-encoded function call data (0x-prefixed) |
| `from` | string | No | Sender address (optional, for context) |
| `block` | string | No | Block to query (default: `"latest"`) |

### `mezo_testnet_call`

Execute a read-only smart contract call on Mezo testnet. Same interface as `eth_call`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | Yes | Contract address (0x-prefixed) |
| `data` | string | Yes | ABI-encoded function call data (0x-prefixed) |
| `from` | string | No | Sender address (optional, for context) |
| `block` | string | No | Block to query (default: `"latest"`) |

---

## Revert Decoding

### `eth_decode_revert`

Decode raw EVM revert data from a failed transaction or eth_call on Ethereum. Handles:
- `Error(string)` reverts
- `Panic(uint256)` assertions
- Custom Solidity errors (requires ABI)
- Silent reverts (empty data)

Pure computation — no RPC call needed.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `revert_data` | string | Yes | Raw revert data as hex (0x-prefixed). Use `"0x"` or empty string for silent reverts. |
| `abi` | string or array | No | Contract ABI for decoding custom errors. Only needed beyond built-in `Error(string)` and `Panic(uint256)`. |

### `mezo_decode_revert`

Decode raw EVM revert data on Mezo. Same interface as `eth_decode_revert`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `revert_data` | string | Yes | Raw revert data (0x-prefixed hex) |
| `abi` | string or array | No | Contract ABI for custom errors |

### `mezo_testnet_decode_revert`

Decode raw EVM revert data on Mezo testnet. Same interface as `eth_decode_revert`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `revert_data` | string | Yes | Raw revert data (0x-prefixed hex) |
| `abi` | string or array | No | Contract ABI for custom errors |

---

## ABI Decoding

### `eth_decode_calldata`

Decode raw calldata into function name and typed arguments using a provided ABI on Ethereum. Pure computation — no RPC call needed.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calldata` | string | Yes | Raw calldata (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI as JSON array or JSON-encoded string |

### `mezo_decode_calldata`

Decode raw calldata on Mezo. Same interface as `eth_decode_calldata`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calldata` | string | Yes | Raw calldata (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |

### `mezo_testnet_decode_calldata`

Decode raw calldata on Mezo testnet. Same interface as `eth_decode_calldata`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calldata` | string | Yes | Raw calldata (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |

### `eth_decode_return`

Decode raw return data from an eth_call into typed values. Pure computation — no RPC call needed.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `data` | string | Yes | Raw return data (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |
| `function_name` | string | Yes | Function name to decode against |

### `mezo_decode_return`

Decode raw return data on Mezo. Same interface as `eth_decode_return`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `data` | string | Yes | Raw return data (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |
| `function_name` | string | Yes | Function name to decode against |

### `mezo_testnet_decode_return`

Decode raw return data on Mezo testnet. Same interface as `eth_decode_return`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `data` | string | Yes | Raw return data (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |
| `function_name` | string | Yes | Function name to decode against |

### `eth_decode_log`

Decode a raw event log (topics + data) into named fields using a provided ABI on Ethereum. Pure computation — no RPC call needed.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topics` | array | Yes | Log topics array. `topics[0]` is the event signature hash. |
| `data` | string | Yes | Log data field (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |

### `mezo_decode_log`

Decode a raw event log on Mezo. Same interface as `eth_decode_log`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topics` | array | Yes | Log topics array |
| `data` | string | Yes | Log data field (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |

### `mezo_testnet_decode_log`

Decode a raw event log on Mezo testnet. Same interface as `eth_decode_log`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `topics` | array | Yes | Log topics array |
| `data` | string | Yes | Log data field (0x-prefixed hex) |
| `abi` | string or array | Yes | Contract ABI |

---

## Calldata Encoding

### `eth_encode_calldata`

Encode a function call into ABI-encoded calldata hex. Accepts either a human-readable function signature or a full ABI + function name. Pure computation — no RPC call needed.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `signature` | string | No | Human-readable signature, e.g. `"transfer(address to, uint256 amount)"`. Use instead of `abi` + `function_name`. |
| `abi` | string or array | No | Full ABI as JSON array. Use with `function_name`. |
| `function_name` | string | No | Function name — required when using `abi` instead of `signature` |
| `args` | array | No | Arguments array. Pass uint/int values as decimal or hex strings for precision. |

Provide either `signature` or both `abi` + `function_name`.

---

## Batch Reads

### `eth_multicall`

Batch multiple read-only contract calls into a single RPC round trip via Multicall3 on Ethereum (`0xcA11bde05977b3631167028862bE2a173976CA11`). Returns success status and raw return data for each call.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calls` | array | Yes | Array of call objects (at least 1) |

Each call object:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `target` | string | Yes | Contract address (0x-prefixed) |
| `callData` | string | Yes | ABI-encoded call data (0x-prefixed) |
| `allowFailure` | boolean | No | Allow this call to fail without reverting the batch (default: true) |

### `mezo_multicall`

Batch reads on Mezo via Multicall3. Same interface as `eth_multicall`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calls` | array | Yes | Array of call objects (at least 1) |

### `mezo_testnet_multicall`

Batch reads on Mezo testnet via Multicall3. Same interface as `eth_multicall`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `calls` | array | Yes | Array of call objects (at least 1) |
