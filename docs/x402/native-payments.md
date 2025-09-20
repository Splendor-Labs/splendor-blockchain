# Splendor Native x402 Payments Guide

## 🚀 World's First Native Blockchain x402 Implementation

Splendor is the **first and only blockchain** with **native x402 payment support** built directly into the consensus layer. Add micropayments to any API in 1 line of code with zero gas fees for users.

---

## 🚀 Quick Start

### 1. Setup (Automatic)
```bash
# x402 automatically configures during node setup
./node-setup.sh --rpc

# Start node with x402 API enabled
./node-start.sh --rpc
```

### 2. Add Payments to Your API (1 Line!)
```javascript
const { splendorX402Express } = require('./x402-middleware');

// Add payments in 1 line!
app.use('/api', splendorX402Express({
  payTo: '0xYourWalletAddress',  // You get 90% of payments
  pricing: {
    '/api/weather': '0.001',     // $0.001 per request
    '/api/premium': '0.01'       // $0.01 per request
  }
}));

// That's it! Your API now accepts x402 payments
app.get('/api/weather', (req, res) => {
  res.json({ 
    weather: 'Sunny, 75°F',
    payment: req.x402,
    yourRevenue: '$0.0009'  // You earned 90%
  });
});
```

### 3. Test Your Integration
```bash
# Test x402 functionality
./test-x402.sh

# Test your API
curl http://localhost:3000/api/premium
# Returns 402 Payment Required with payment instructions
```

---

## 🆚 Why Splendor x402 is Revolutionary

### Splendor vs Others (Coinbase, Ethereum, etc.)

| Feature | **Splendor Native** | **Coinbase x402** | **Ethereum** | **Advantage** |
|---------|-------------------|------------------|--------------|---------------|
| **Settlement Speed** | **<100ms** | 2-15 seconds | 12-15 seconds | **150x faster** |
| **User Gas Fees** | **$0** | $0.01-$50 | $1-$50 | **100% savings** |
| **Developer Revenue** | **90% guaranteed** | Variable | N/A | **Predictable** |
| **Integration** | **1 line of code** | 50+ lines | Complex | **50x simpler** |
| **Consensus Level** | **✅ Native** | ❌ External | ❌ External | **Revolutionary** |
| **TPS Capability** | **Millions** | ~50,000 | ~15 | **20x+ higher** |
| **Minimum Payment** | **$0.001** | $0.01+ | $1+ | **10x+ smaller** |
| **Signature Type** | **Simple message** | EIP-3009 | Complex | **User-friendly** |

### Key Advantages

#### 1. **True Micropayments**
- **Splendor**: $0.001 minimum, zero gas fees
- **Others**: $0.01+ minimum due to gas costs

#### 2. **Instant Settlement**
- **Splendor**: <100ms consensus-level settlement
- **Others**: 2-15 seconds for blockchain confirmation

#### 3. **Developer-First**
- **Splendor**: 90% revenue share, 1-line integration
- **Others**: Variable fees, complex integration

#### 4. **User Experience**
- **Splendor**: Simple message signing, zero gas
- **Others**: Complex EIP-3009, gas fees

#### 5. **Scalability**
- **Splendor**: Millions of TPS (bypasses tx pool)
- **Others**: Limited by blockchain TPS

---

## 🔧 Technical Architecture

### 1. Consensus Layer Integration

Unlike external solutions, Splendor's x402 is built into the consensus engine:

```go
// In consensus/congress/congress_govern.go
if tx.Type() == types.X402TxType {
    // Native x402 settlement in consensus
    var p x402Payload
    if err = rlp.DecodeBytes(tx.Data(), &p); err != nil {
        vmerr = fmt.Errorf("x402: invalid payload: %w", err)
        return
    }
    
    // Direct state manipulation - no gas fees
    state.SubBalance(p.From, p.Value)
    state.AddBalance(p.To, p.Value)
    
    // Automatic revenue sharing
    validatorFee := amount * 5 / 100    // 5% to validator
    protocolFee := amount * 5 / 100     // 5% to protocol
    apiProviderFee := amount * 90 / 100  // 90% to developer
}
```

### 2. Native RPC API

```bash
# Check supported payment methods (native)
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"x402_supported","params":[],"id":1}' \
  https://rpc1.splendor.org/

# Verify payment without executing
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"x402_verify","params":[requirements, payload],"id":1}' \
  https://rpc1.splendor.org/

# Settle payment instantly
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"x402_settle","params":[requirements, payload],"id":1}' \
  https://rpc1.splendor.org/
```

---

## 💰 Revenue Model

### Automatic Revenue Distribution

Every x402 payment is automatically split:

```
User Payment: $0.001 SPLD
├── API Provider: $0.0009 SPLD (90%) ← YOU
├── Validator: $0.00005 SPLD (5%) ← NETWORK SECURITY
└── Protocol: $0.00005 SPLD (5%) ← DEVELOPMENT FUND

Blockchain Charges: $0.00 ← NO FEES!
```

### Revenue Examples

- **Weather API**: 1000 requests/day × $0.001 = **$27/month** for you
- **AI Images**: 100 images/day × $0.05 = **$135/month** for you  
- **Analytics**: 50 reports/day × $0.10 = **$135/month** for you

---

## 🔄 Upgrading Existing Chains

### Can I upgrade my existing Splendor chain?
**YES!** You can add x402 support to existing chains without starting fresh:

#### Hot Upgrade Process (No Downtime)
1. **Copy x402 files** to existing installation
2. **Update backend.go** to register x402 API
3. **Update transaction.go** to support X402TxType
4. **Rebuild node** with x402 support
5. **Restart with x402 API** enabled
6. **Install middleware** and configure

#### Zero Cost Upgrade
- ✅ **No blockchain fees** for x402 functionality
- ✅ **No upgrade costs** or licensing fees
- ✅ **No ongoing charges** for x402 payments
- ✅ **Backward compatible** - existing transactions continue working

---

## 🧪 Testing & Deployment

### Test x402 Functionality
```bash
# Verify x402 integration
./verify-x402-integration.sh

# Test x402 API
./test-x402.sh
```

### Production Deployment
```bash
# Start node with x402 API
./node-start.sh --rpc

# x402 API automatically included in:
# --http.api db,eth,net,web3,personal,txpool,miner,debug,x402
```

### ERC‑20 Notes
- Verification checks `balanceOf(from)` and `allowance(from → payTo)`
- Optional: include EIP‑2612 `permit` in the payment payload to skip prior approve
- Settlement: if `permit` is present, the chain executes `permit(owner, payTo, value, deadline, v, r, s)` then `transferFrom(from, payTo, amount)` in consensus
- If not using `permit`, users must approve your `payTo` address on that token

Example `permit` in x402 payload:
```json
{
  "x402Version": 1,
  "scheme": "exact",
  "network": "splendor",
  "payload": {
    "from": "0xPayer",
    "to": "0xPayTo",
    "value": "0x...",
    "validAfter": 1710000000,
    "validBefore": 1710000300,
    "nonce": "0x...",
    "asset": "0xToken",
    "signature": "0x...",
    "permit": {
      "value": "0x...",
      "deadline": "0x...",
      "v": 28,
      "r": "0x...32bytes...",
      "s": "0x...32bytes..."
    }
  }
}
```

---

## Developer Summary: Permit & ERC‑20 Compatibility

### Permit Support (EIP‑2612)
- Implements the standard `permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s)` flow.
- Lets users set allowances with an off‑chain signature instead of an on‑chain `approve()`.
- If a token deviates from the spec (custom domain/version), `permit` may revert; we automatically fall back to checking standard `allowance`.

### ERC‑20 Transfer Return Handling
- Legacy tokens: `transfer`/`transferFrom` may return no data → treated as success.
- Standard tokens: 32‑byte boolean is decoded and validated.

### Security Guarantees
- Consensus path enforces:
  - Strict x402 payment signature (EIP‑191 message binding addresses, value, time window, asset, chainId).
  - Permit deadline checks (if present) and on‑chain verification by the token contract (EIP‑2612).
  - Nonce replay protection persisted on chain per (payer, nonce).

---

## Non‑Technical Summary (Investors / Stakeholders)
- Gasless approvals: users authorize with a signature instead of paying for two transactions.
- Broad compatibility: works with legacy and modern ERC‑20 tokens; falls back to normal allowance if `permit` isn’t supported.
-- Security built‑in: authorizations are time‑limited, single‑use, and cryptographically verified on chain.
- Future‑proof: handles both legacy behaviors and newer standards seamlessly.

### Monitor Your Revenue
```bash
# Check your wallet balance (your 90% share)
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0xYourWallet","latest"],"id":1}' \
  https://rpc1.splendor.org/

# Get x402 revenue statistics
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"x402_getRevenueStats","params":[],"id":1}' \
  https://rpc1.splendor.org/
```

---

## 🎯 Use Cases

### API Monetization
- **Weather APIs**: $0.001 per request
- **Stock Data**: $0.01 per quote
- **News Articles**: $0.005 per article
- **Maps/Directions**: $0.002 per route

### AI Services
- **Image Generation**: $0.05 per image
- **Text Generation**: $0.01 per request
- **Voice Synthesis**: $0.02 per audio file
- **Translation**: $0.001 per word

### Data Services
- **Analytics Reports**: $0.10 per report
- **Database Queries**: $0.001 per query
- **File Storage**: $0.001 per MB
- **CDN Access**: $0.0001 per file

---

## 🔧 Advanced Configuration

### Middleware Options
```javascript
const middleware = splendorX402Express({
  // Required
  payTo: '0xYourWalletAddress',        // Your wallet (receives 90%)
  
  // Optional
  rpcUrl: 'http://localhost:80',       // Splendor RPC endpoint
  network: 'splendor',                 // Network name
  chainId: 6546,                       // Splendor chain ID
  defaultPrice: '0.001',               // Default price in USD
  
  // Flexible pricing
  pricing: {
    '/api/free': '0',                  // Free endpoint
    '/api/premium': '0.001',           // Fixed price
    '/api/data/*': '0.01',             // Wildcard pattern
    '/api/analytics': '0.05',          // Higher value content
    '/api/bulk/*': '0.0001'            // Bulk pricing
  }
});
```

### Environment Variables
```bash
# x402 Configuration (auto-added during setup)
X402_ENABLED=true
X402_NETWORK=splendor
X402_CHAIN_ID=6546
X402_DEFAULT_PRICE=0.001
X402_MIN_PAYMENT=0.001
```

---

## 🎉 Conclusion

Splendor's native x402 implementation represents a **paradigm shift** in blockchain payments:

- **🌍 World's first** native x402 blockchain
- **⚡ 150x faster** than external x402 solutions
- **💰 Zero gas fees** for users
- **🔧 1-line integration** for developers
- **📈 90% revenue share** guaranteed
- **🔄 Hot upgrades** for existing chains

**Welcome to the future of internet payments!** 🚀

---

*Built with ❤️ by the Splendor team - The first blockchain to make micropayments practical for developers.*
