# Machina

- **Type:** API (seller; not a facilitator)
- **Description:** Machine-to-machine utility APIs for AI agents, pay-per-call in USDC on Base via x402. No API key required. Eight REST endpoints cover Python code security scanning ($0.020), email validation — single ($0.007) and bulk ($0.50 per 100 addresses), domain intelligence ($0.050), PDF-to-Markdown — standard ($0.050) and large-document ($0.180), a free PDF pre-check ($0.000), and URL-to-Markdown extraction ($0.005).
- **Website (HTTPS):** https://mach402.dev
- **Logo:** https://mach402.dev/favicon.ico
- **Documentation:** https://mach402.dev/docs
- **Public support channel:** ceo@difftechstore.com (email)

## Networks and activity

- **Networks and chain identifiers:** Base mainnet (eip155:8453)
- **Public explorer link:** https://basescan.org/address/0x14B21d80bf5776437dD4f1043BDAf31e60B366AD
- **App receiving address (Base, USDC):** `0x14B21d80bf5776437dD4f1043BDAf31e60B366AD`
- **Proof that we represent the project:** https://mach402.dev/.well-known/x402 — publicly accessible, lists all paid endpoints and the seller address, hosted on the same domain as the official website

## Payments

- **Paid endpoint and HTTP method:** `POST https://mach402.dev/url/extract` (URL-to-Markdown, $0.005 USDC). Full endpoint list: https://mach402.dev/.well-known/x402
- **Facilitator used:** Coinbase CDP — https://api.cdp.coinbase.com/platform/v2/x402
- **Payment scheme / x402 version:** exact, x402Version 2
- **Token contract and decimals:** USDC on Base — `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`, 6 decimals
- **Successful payment transaction:** Buyer wallet `0x289e52C8677125444430BD32D21D9a8b107Bc184` paid $0.005 USDC to seller wallet `0x14B21d80bf5776437dD4f1043BDAf31e60B366AD` on 2026-10-19 (Base mainnet). CDP Facilitator — TX hash truncated in receipt (known prefix: `0xf4c36f303d06b103deae6f075834e951ed4704ec9c4f9ca4807431dd9c1…`). Explorer: https://basescan.org/address/0x289e52C8677125444430BD32D21D9a8b107Bc184
- **Facilitator signer / relayer addresses:** Not ours (Coinbase CDP facilitator)

## Website application

- **Website application reference:** N/A
- **Related issue or previous pull request:** None
