# Splendor x402 Documentation

## 🚀 Stripe for Web3

X402 is **instant wallet-based payment verification** with **flexible settlement** - essentially **Stripe for Web3**. Enable pay-per-use APIs, content paywalls, and micropayments with just wallet signatures.

**✅ Production Ready** | **✅ Wallet Compatible** | **✅ Developer Friendly**

> **[📖 Developer Pitch](DEVELOPER_PITCH.md)** - Why X402 is a game-changer for Web3 payments

- developer-integration.md — End‑to‑end integration guide (RPC, payloads, flows, ERC‑20, permit)
- native-payments.md — Technical details of on‑chain settlement and architecture
- payment-guide.md — Pricing and payload formats (quick reference)
- examples/
  - See `Core-Blockchain/examples/x402-middleware-server.js` for an Express example using the middleware

Quick RPC checks

- Supported kinds:
  curl -s -X POST -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"x402_supported","params":[],"id":1}' \
    https://rpc1.splendor.org/

- Verify / Settle:
  See payload examples in `developer-integration.md` and `payment-guide.md`.
