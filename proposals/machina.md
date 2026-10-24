# Machina

- **Type:** API (seller; not a facilitator)
- **Description:** Machine-to-machine utility APIs for AI agents, pay-per-call in USDC on Base via x402. No API key required. Eight REST endpoints cover Python code security scanning ($0.020), email validation — single ($0.007) and bulk ($0.50 per 100 addresses), domain intelligence ($0.050), PDF-to-Markdown — standard ($0.050) and large-document ($0.180), a free PDF pre-check ($0.000), and URL-to-Markdown extraction ($0.005).
- **Website (HTTPS):** https://mach402.dev
- **Logo:** https://mach402.dev/logo.png
- **Documentation:** https://mach402.dev/docs
- **Public support channel:** ceo@difftechstore.com (email)

## Networks and activity

- **Networks and chain identifiers:** Base mainnet (eip155:8453)
- **Public explorer link:** https://basescan.org/address/0x14B21d80bf5776437dD4f1043BDAf31e60B366AD
- **App receiving address (Base, USDC):** `0x14B21d80bf5776437dD4f1043BDAf31e60B366AD`
- **Proof that we represent the project:** https://mach402.dev/.well-known/x402 — publicly accessible, lists all paid endpoints, the seller address (`0x14B21d80bf5776437dD4f1043BDAf31e60B366AD`), network (`eip155:8453`), facilitator, and token contract. Hosted on the same domain as the official website.

## Payments

- **Paid endpoint and HTTP method:** `POST https://mach402.dev/url/extract` (URL-to-Markdown, $0.005 USDC). Full endpoint list: https://mach402.dev/.well-known/x402
- **Facilitator used:** Coinbase CDP — https://api.cdp.coinbase.com/platform/v2/x402
- **Payment scheme / x402 version:** exact, x402Version 2
- **Token contract and decimals:** USDC on Base — `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`, 6 decimals
- **Successful payment transaction (operator self-test):** Buyer wallet `0x289e52C8677125444430BD32D21D9a8b107Bc184` paid $0.005 USDC to seller wallet `0x14B21d80bf5776437dD4f1043BDAf31e60B366AD` on 2026-10-21 (Base mainnet). TX: [`0x15aa8966e73f479b81d61bd06b2e118db6f49f92adf3710c8771073be9ab7cb9`](https://basescan.org/tx/0x15aa8966e73f479b81d61bd06b2e118db6f49f92adf3710c8771073be9ab7cb9). Settlement receipt (base64): `eyJzdWNjZXNzIjogdHJ1ZSwgInBheWVyIjogIjB4Mjg5ZTUyQzg2NzcxMjU0NDQ0MzBCRDMyRDIxRDlhOGIxMDdCYzE4NCIsICJ0cmFuc2FjdGlvbiI6ICIweDE1YWE4OTY2ZTczZjQ3OWI4MWQ2MWJkMDZiMmUxMThkYjZmNDlmOTJhZGYzNzEwYzg3NzEwNzNiZTlhYjdjYjkiLCAibmV0d29yayI6ICJlaXAxNTU6ODQ1MyJ9`
- **Buyer integration guide:** https://mach402.dev/docs/buyer-guide
- **Facilitator signer / relayer addresses:** Not ours (Coinbase CDP facilitator)

## Verified response

Operator self-test — `POST /url/extract` with `{"url": "https://example.com"}`, $0.005 USDC:

```json
{
  "url": "https://example.com",
  "title": "Example Domain",
  "author": null,
  "date": null,
  "sitename": null,
  "char_count": 156,
  "word_count": 25,
  "truncated": false,
  "markdown": "This domain is for use in documentation examples without needing permission. This is not a service; avoid relying on it for testing and monitoring purposes.",
  "duration_ms": 540.0
}
```

Decoded settlement receipt:

```json
{
  "success": true,
  "payer": "0x289e52C8677125444430BD32D21D9a8b107Bc184",
  "transaction": "0x15aa8966e73f479b81d61bd06b2e118db6f49f92adf3710c8771073be9ab7cb9",
  "network": "eip155:8453"
}
```

## Website application

- **Website application reference:** N/A
- **Related issue or previous pull request:** None
