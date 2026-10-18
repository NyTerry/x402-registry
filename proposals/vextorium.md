# Vextorium
- Type: API (paid x402 seller); not a facilitator.
- Description: 552 pay-per-call data APIs for AI agents: on-chain data on 30+ chains, DeFi, markets, official statistics, compliance, web and developer tools. Also available as a remote MCP server.
- Website (HTTPS): https://vextorium.com
- Logo (HTTPS): https://raw.githubusercontent.com/bru1noia/vextorium-mcp/main/logo.svg
- Documentation: buyer integration guide (catalogue, request bodies, headers, receipts, failures/timeouts, MCP): https://github.com/bru1noia/vextorium-mcp/blob/main/PAYMENTS.md · https://api.vextorium.com/openapi.json · https://api.vextorium.com/catalog.json
- Public repository: https://github.com/bru1noia/vextorium-mcp
- Networks and chain identifiers: Base mainnet (eip155:8453), Solana mainnet (solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp). The same challenge also offers Polygon (eip155:137), Optimism (eip155:10), Arc (eip155:5042) and XRP Ledger (xrpl:0), outside the initial review scope.
- Paid resource URLs and HTTP methods: POST https://api.vextorium.com/v1/* for paid calls (GET only for discovery: an unsigned GET returns the same 402 terms) (full list in the OpenAPI document and https://api.vextorium.com/.well-known/x402). Example: POST https://api.vextorium.com/v1/fx-rates returns HTTP 402 with a PAYMENT-REQUIRED header. `maxTimeoutSeconds` (300 s) is the seller's settlement time limit, not the signature expiry (EIP-3009 `validBefore` on Base; recent-blockhash validity on Solana). Some routes return 503 (no charge) while under maintenance and are not listed in the catalogue.
- x402 version and payment scheme: x402 v2, scheme exact (EIP-3009 on EVM).
- Token contract or mint, decimals: USDC on Base 0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913 (6); USDC on Solana EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v (6). Typical price 3000 atomic units (0.003 USDC).
- Seller payment recipient addresses: Base 0xb9f06D2734B7E9D7b21B64C4D8b95bd774b71aCf · Solana HcjfmZLgWRZzwj4deEiS7pewc4KBYSw39mW5nTyt3pz2
- Scope: seller-only listing; Vextorium does not offer facilitator endpoints to third parties.
- Facilitator URL and supported endpoint:
  - Base: Coinbase CDP x402 facilitator, base URL https://api.cdp.coinbase.com/platform/v2/x402 (`/supported`, `/verify`, `/settle`).
    - Every request is authenticated with Vextorium's CDP API key (a JWT per request, via @coinbase/cdp-sdk). Buyers need no credentials.
    - No fallback: if CDP fails, the call is not settled and no data is delivered.
  - Solana: Coinbase CDP x402 facilitator (same base URL and authentication as Base) since 2026-10-16.
    - The fee payer is CDP's and is advertised in `accepts[].extra.feePayer`.
    - Before 2026-10-16 Solana was settled by a self-hosted reference @x402/svm facilitator (fee payer 4ZHcGaij9ktYqm9dNvMSMLsbT5D7BUrXDEzQGVH5h3Q6); the historical transaction in evidence 3 was settled that way.
  - Details: PAYMENTS.md §3.
- Settlement order: verify → run the service → settle only if it answered < 400 → return the data with PAYMENT-RESPONSE. Calls that fail before settlement are not charged. After a settlement attempt the server never issues a fresh payment request; uncertain outcomes are reconciled as described in PAYMENTS.md §4 (with a reproducible test).
- Facilitator signer / relayer addresses (if applicable): Base settlements are submitted by CDP relayers (for example 0x8cda367232d78c067116e3260da881d2da8ffa39 and 0x8f5cb67b49555e614892b7233cfddebfb746e531). Solana settlements are submitted by CDP fee payers (advertised in `accepts[].extra.feePayer`, for example BFK9TLC3edb13K6v4YyH3DwPb5DSUpkWvb7XnqCL9b4F).
- Remote MCP: https://api.vextorium.com/mcp, part of the same application. Details in PAYMENTS.md §5.
  - Transport: Streamable HTTP, stateless.
  - Tools: search_services and service_details (free), call_service (x402 over MCP).
  - Networks: same as HTTP except XRPL.
- Example successful settlement hashes and explorer links: see "Settlement evidence" below.
- Ownership evidence (domain-hosted proof or public project reference): the payment recipients above are published in the live 402 challenge served by api.vextorium.com and in https://api.vextorium.com/.well-known/x402; the public repository links to the same domain.
- Public support channel: https://github.com/bru1noia/vextorium-mcp/issues

## Settlement evidence (Base and Solana)

End-to-end captures with retained responses (responses are retained since 2026-10-16, only for services that return public-source data):

### A. Base: operator test, clearly labelled

| Field | Value |
|---|---|
| Request | `POST https://api.vextorium.com/v1/fx-rates`, body `{"currencies":"EUR,JPY"}`, 2026-10-16 22:12:20 UTC |
| Amount | 4000 atomic units (0.004 USDC) |
| Response | `200`: `{"currencies":2,"types":[{"currency":"EUR","units_per_usd":0.888652,...}],"source":"Source: ECB statistics ..."}` |
| Transaction | https://basescan.org/tx/0xf4ab31807b2498de8baa0a7106b2ccbcd26dfc9284626f9d4a32e95b1a10b76c |

```json
{"success": true, "payer": "0xa8FC…868F", "transaction": "0xf4ab31807b2498de8baa0a7106b2ccbcd26dfc9284626f9d4a32e95b1a10b76c", "network": "eip155:8453"}
```

### B. Solana: customer payment (not an operator test)

| Field | Value |
|---|---|
| Request | `POST https://api.vextorium.com/v1/sec-company-ranking`, 2026-10-17 00:56:33 UTC. Request body not retained; the response matches the catalogue example body `{"metric": "revenue", "period": "CY2025", "limit": 10}`. |
| Amount | 6000 atomic units (0.006 USDC) |
| Response | `200`, 1644 bytes, sha256 `7de8b67a30835ea414de50aaa6f09bdbda67e56f8b8c78db1024e4d45b7e175a`: `{"metric":"revenue","period":"CY2025","companies":4384,"ranking":[{"company":"AMAZON.COM, INC.","ticker":"AMZN","value_usd":716924000000,"position":1},...],"source":"SEC EDGAR, XBRL frames"}` (public SEC data) |
| Settled by | CDP facilitator, slot 457844994 (fee payer `BFK9TLC3edb13K6v4YyH3DwPb5DSUpkWvb7XnqCL9b4F`) |
| On-chain | USDC `2WgR…Z23Wx` → `HcjfmZ…t3pz2`, 0.006 |
| Transaction | https://solscan.io/tx/3MFE9Fp9DM7Y5MweKA4RXM3JMukcV3QN7opTqiPvixkbooEgZbNKz2Bo9v9BUYsB1v6KYZ7nvJbrpFgVfjFTH5PV |

```json
{"success": true, "payer": "2WgR…Z23Wx", "transaction": "3MFE9Fp9DM7Y5MweKA4RXM3JMukcV3QN7opTqiPvixkbooEgZbNKz2Bo9v9BUYsB1v6KYZ7nvJbrpFgVfjFTH5PV", "network": "solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp"}
```

## Historical transaction evidence (customer payments, before responses were retained)

Each payment below can be matched three ways:
- **Settlement record**: our ledger entry, written when settlement succeeds. Its fields (`payer`, `transaction`, `network`, amount) are the fields of the decoded `PAYMENT-RESPONSE`.
- **Delivery log**: the backend access log line for the same request, with method and HTTP status.
- **On-chain**: success status, and USDC moved from payer to `payTo`.

Times are UTC. Response bodies of these customer calls were not retained, so the delivery proof is the logged `200 OK`.

### 1. Base: `/v1` route at the typical price (0.003 USDC)

| Field | Value |
|---|---|
| Request | `POST https://api.vextorium.com/v1/ecb-rates-history`, 2026-10-15 17:14:58 |
| Delivery log | `"POST /v1/ecb-rates-history HTTP/1.1" 200 OK` |
| Amount | 3000 atomic units (0.003 USDC) |
| Settled by | CDP facilitator, block 52647577 (relayer `0x8cda367232d78c067116e3260da881d2da8ffa39`) |
| On-chain | USDC Transfer `0x2773…62aa` → `0xb9f06D…71aCf`, 0.003 |
| Transaction | https://basescan.org/tx/0x3238f55cfeddf0ecb2ec9b16f832130f80d78411e7b7607ba7292f4d004a0cf3 |

```json
{"success": true, "transaction": "0x3238f55cfeddf0ecb2ec9b16f832130f80d78411e7b7607ba7292f4d004a0cf3", "network": "eip155:8453", "payer": "0x2773c5b227fc2c824e9d08d1cb320197b94662aa", "amount": "3000"}
```

### 2. Base: the transaction originally supplied (0.005 USDC)

| Field | Value |
|---|---|
| Request | `POST https://api.vextorium.com/clasificar-pago`, 2026-10-14 12:01:02. This is the legacy alias of `/v1/classify-text`, which is priced at 0.005 USDC. |
| Delivery log | `"POST /clasificar-pago HTTP/1.1" 200 OK` |
| Amount | 5000 atomic units (0.005 USDC) |
| Settled by | CDP facilitator, block 52594959 (relayer `0x8f5cb67b49555e614892b7233cfddebfb746e531`) |
| On-chain | USDC Transfer `0xc9c7…1670` → `0xb9f06D…71aCf`, 0.005 |
| Transaction | https://basescan.org/tx/0xb3cdc7752938b5cf1c4ca475bb0bb755f3f63f3ca59d2926d4b75ee2b8f33a4a |

```json
{"success": true, "transaction": "0xb3cdc7752938b5cf1c4ca475bb0bb755f3f63f3ca59d2926d4b75ee2b8f33a4a", "network": "eip155:8453", "payer": "0xc9c7b38C0942914fC8EA12063BC92dcd3b581670"}
```

### 3. Solana: the transaction originally supplied (0.005 USDC)

| Field | Value |
|---|---|
| Request | `POST https://api.vextorium.com/revisar-firma-pago`, 2026-10-14 17:00:18. This is the legacy alias of `/v1/eip712-review`, which is priced at 0.005 USDC; that is why the amount is 0.005 and not 0.003. |
| Delivery log | `"POST /revisar-firma-pago HTTP/1.1" 200 OK` |
| Amount | 5000 atomic units (0.005 USDC) |
| Settled by | Our self-hosted facilitator, slot 456913564 (fee payer `4ZHcGaij9ktYqm9dNvMSMLsbT5D7BUrXDEzQGVH5h3Q6`) |
| On-chain | USDC `34CMQ…5v2z` → `HcjfmZ…t3pz2`, 0.005 |
| Transaction | https://solscan.io/tx/rPZcUEk6r8MoQthXyrH8edKbk1ZtGs9eWFouufsp9vzC2XQcx93f81J4rM6yYZje9BcgdHeNyYcx9QuVaMqP3aV |

```json
{"success": true, "transaction": "rPZcUEk6r8MoQthXyrH8edKbk1ZtGs9eWFouufsp9vzC2XQcx93f81J4rM6yYZje9BcgdHeNyYcx9QuVaMqP3aV", "network": "solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp", "payer": "34CMQ3HB3aDWPxwAbLSbQPrBtrFhi354zAsfbb3z5v2z", "amount": "5000"}
```
