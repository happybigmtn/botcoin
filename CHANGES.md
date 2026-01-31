# Botcoin: Changes from Bitcoin Core v29

This document lists every modification made to Bitcoin Core to create Botcoin.
Full transparency. No hidden changes. Verify everything yourself.

## Repository

- **Base:** Bitcoin Core v29.0
- **Fork date:** January 30, 2026
- **Commit history:** Single squashed commit for clean start

## Consensus Changes

### 1. Block Time (60 seconds instead of 10 minutes)

**File:** `src/kernel/chainparams.cpp`
```cpp
// Before (Bitcoin)
consensus.nPowTargetSpacing = 10 * 60; // 10 minutes

// After (Botcoin)
consensus.nPowTargetSpacing = 60; // 60 seconds
```

### 2. Difficulty Adjustment Window (1 hour instead of 2 weeks)

**File:** `src/kernel/chainparams.cpp`
```cpp
// Before (Bitcoin)
consensus.nPowTargetTimespan = 14 * 24 * 60 * 60; // 2 weeks

// After (Botcoin)
consensus.nPowTargetTimespan = 60 * 60; // 1 hour (60 blocks)
```

### 3. Proof of Work Algorithm (RandomX instead of SHA256)

**Files:** 
- `src/crypto/randomx_hash.cpp` (new)
- `src/crypto/randomx_hash.h` (new)
- `src/pow.cpp` (modified)
- `cmake/randomx.cmake` (new)

RandomX is CPU-friendly, ASIC-resistant. Same algorithm used by Monero.
Source: https://github.com/tevador/RandomX (MIT license)

### 4. Genesis Block

**File:** `src/kernel/chainparams.cpp`

- **Timestamp:** 1738195200 (January 30, 2026)
- **Message:** "01100110 01110010 01100101 01100101"
- **Reward:** 50 BTC (same as Bitcoin)

### 5. Network Magic Bytes

**File:** `src/kernel/chainparams.cpp`
```cpp
// Mainnet
pchMessageStart[0] = 0xb0;
pchMessageStart[1] = 0x7c;
pchMessageStart[2] = 0x01;
pchMessageStart[3] = 0x0e;
```

### 6. Address Prefixes

**File:** `src/kernel/chainparams.cpp`
- Mainnet: `bot1` (bech32)
- Testnet: `tbot1` (bech32)
- Regtest: `bcrt1` (unchanged)

## Branding Changes (Non-Consensus)

| File | Change |
|------|--------|
| `CMakeLists.txt` | CLIENT_NAME: "Bitcoin Core" → "Botcoin Core" |
| `src/clientversion.cpp` | UA_NAME: "Satoshi" → "Botcoin" |
| `src/qt/bitcoinunits.cpp` | Unit names: BTC → BOT |

## What Was NOT Changed

- **Cryptography:** Same secp256k1, same ECDSA signatures
- **Transaction format:** Identical to Bitcoin
- **Script system:** Same opcodes, same limits
- **Block structure:** Same merkle tree, same header format
- **Wallet format:** Same BIP32/39/44 derivation
- **P2P protocol:** Same messages, different magic bytes
- **RPC interface:** Same commands, same parameters

## Verification

```bash
# Clone and verify yourself
git clone https://github.com/happybigmtn/botcoin
cd botcoin

# Compare against Bitcoin Core v29
git remote add bitcoin https://github.com/bitcoin/bitcoin
git fetch bitcoin v29.0

# See exact differences
git diff bitcoin/v29.0..HEAD --stat
```

## License

Same as Bitcoin Core: MIT License

## Security

If you find a vulnerability, please report responsibly.
See SECURITY.md for details.
