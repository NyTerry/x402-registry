# Horizon Pulse
- Type: API (seller; not a facilitator)
- Description: Pay-per-call APIs for AI agents over x402, no API key: web search, web fetch to markdown, HTTP proxy, structured page extract, page screenshots, PDF to text, x402 endpoint and CDP Bazaar index checks, crypto market data, and a discourse split (how public Hacker News and Bluesky posts split on a question; a sample, not a poll; $0.03), and transaction status (whether a transaction on Base, Polygon, Solana or the XRP Ledger has landed and is final; $0.002), and a tokenized stock status check (on-chain token state of the Coinbase tokenized stocks on Base: paused flags, supply, supply cap, multiplier, issuer; no price; not investment advice; $0.002), and a page change watch (whether a public web page or endpoint changed since your last check, with a short diff of added lines; $0.002), and a payer balance check (whether a wallet can pay right now on each rail: stablecoin balance, gas/fees incl. the XRPL reserve and RLUSD trust line, calls left at live prices; no risk score, no USD valuation; $0.002), and a pre-transfer recipient check (mechanical allow/warn/block: lookalike vs an expected address, OFAC SDN crypto-address snapshot with source URL and date, account age, ScamSniffer public list; not advice; $0.002). 22 paid routes. Also available as a remote MCP server at https://horizonpulse.dev/mcp (23 tools: 22 paid plus free money_market_rates).
- Website (HTTPS): https://horizonpulse.dev
- Logo (HTTPS): https://horizonpulse.dev/icon.png
- Documentation: https://horizonpulse.dev/llms.txt
- Public repository: https://github.com/horizon-pulse/horizon-pulse
- Networks and chain identifiers: HTTP API: Base mainnet (eip155:8453, via Coinbase CDP) and Solana mainnet (solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp, via PayAI); the 402 may also list XRPL and Polygon entries, which are not part of this listing. Hosted MCP (https://horizonpulse.dev/mcp) and the local MCP client: Base only (eip155:8453).
- Paid resource URLs and HTTP methods: the live list, with methods, is at https://horizonpulse.dev/.well-known/x402 (host: horizonpulse.dev only)
- x402 version and payment scheme: x402 v2, exact
- Token contract or mint, decimals: USDC on Base 0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913, 6 decimals; USDC on Solana EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v, 6 decimals
- Seller payment recipient addresses: Base 0x5b32c973596078a967562ca652761404f19be0e9; Solana BjY98A6dS3GGLZdz2zHy8wK7XAwnQgNhCc66mfmBTRPz
- Facilitator URL and supported endpoint (we are not the facilitator):

  | | Coinbase CDP (Base) | PayAI (Solana) |
  |---|---|---|
  | Base URL | `https://api.cdp.coinbase.com/platform/v2/x402` | `https://facilitator.payai.network` |
  | Supported | `GET https://api.cdp.coinbase.com/platform/v2/x402/supported` | `GET https://facilitator.payai.network/supported` |
  | Verify | `POST https://api.cdp.coinbase.com/platform/v2/x402/verify` | `POST https://facilitator.payai.network/verify` |
  | Settle | `POST https://api.cdp.coinbase.com/platform/v2/x402/settle` | `POST https://facilitator.payai.network/settle` |
  | Auth | CDP API key (Bearer JWT); 401 without it | none for `exact`; GET endpoints public |
- Facilitator signer / relayer addresses (not Horizon Pulse addresses; listed separately from the payTo addresses above): PayAI Solana fee payer CjNFTjvBhbJJd2B5ePPMHRLx1ELZpa8dwQgGL727eKww (in PayAI /supported signers["solana:*"]). Base: the 2026-10-13 test transaction was submitted on-chain by 0x97acce27d5069544480bde0f04d9f47d7422a016 (facilitator-side sender; not a Horizon Pulse address).
- Example successful settlement hashes and explorer links (operator test payments, not customer activity; GET https://horizonpulse.dev/api/pulse, 0.005 USDC each, 2026-10-13):
  - Base: https://basescan.org/tx/0xe0fdf8231544f4da2162f8b4c2eb1299f338ab88486096ba54b77d7ac5cfbcd5 (block 52553538)
  - Solana: https://solscan.io/tx/2hUu7qauJuEXcLfzYnXZ7p4berVA49Mvd9hmtkWWRznsFQg9oWNMP1zfU1RrENTaTNLUX13oNm6gioKhFwokA1p7 (slot 456450921)
- Ownership evidence (domain-hosted proof or public project reference): the public repository github.com/horizon-pulse/horizon-pulse is the public source for horizonpulse.dev, and this PR is opened from the horizon-pulse GitHub account. Both payTo addresses appear in the unpaid 402 challenge served by horizonpulse.dev.
- Public support channel: https://github.com/horizon-pulse/horizon-pulse/issues
