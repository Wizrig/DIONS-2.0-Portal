<h1 align="center">DIONS 2.0 Architecture</h1>

<p align="center">
IOCoin Protocol Upgrade — Full Design Architecture & Chain Comparison<br>
<code>Commit: b90ee5a</code> · Fork Height: 5,730,000 (~October 2026)
</p>

<p align="center">
<img src="https://img.shields.io/badge/RPC_Methods-150-3b82f6?style=for-the-badge&labelColor=111118" />
<img src="https://img.shields.io/badge/Tests-511_Passing-22c55e?style=for-the-badge&labelColor=111118" />
<img src="https://img.shields.io/badge/Subsystems-22-a855f7?style=for-the-badge&labelColor=111118" />
<img src="https://img.shields.io/badge/Code-52K_Lines-eab308?style=for-the-badge&labelColor=111118" />
<img src="https://img.shields.io/badge/Targets-13_Binaries-f97316?style=for-the-badge&labelColor=111118" />
<img src="https://img.shields.io/badge/Language-C++20-f0f6fc?style=for-the-badge&labelColor=111118" />
</p>

<h3 align="center">
<a href="https://wizrig.github.io/DIONS-2.0-Portal/">View Interactive Architecture Portal</a>
</h3>

---

## What Is DIONS 2.0?

A hard fork of IOCoin (launched 2014) upgrading it into a modern L1 with dual execution zones, post-quantum cryptography, and native identity/messaging — while keeping full backward compatibility.

**No ICO. No presale. Fair launch continuation since 2014.**

---

## Quick Comparison

| | Ethereum | Solana | DIONS 2.0 |
|:--|:---------|:-------|:----------|
| **L1 TPS** | 15–30 | ~400 | **88** |
| **With Zones** | Thousands (rollups) | ~4,000 | **11,000+** (EVM + SVM) |
| **Min Stake** | 32 ETH (~$96K) | ~1 SOL + extreme HW | **10,000 IOC** |
| **Post-Quantum** | None | None | **Dilithium + Falcon + Kyber** |
| **Native Identity** | No (ENS = off-chain) | No | **W3C DID Core 1.0** |
| **Native Messaging** | No | No | **AES-256 + RSA-4096** |
| **File Storage** | No (IPFS) | No | **On-chain with tiers** |
| **Light Client** | Sync committee | Limited | **Native binary, 0.5 IOC/block** |
| **Earn from Zero** | No | No | **Yes (PoP relay rewards)** |
| **RAM Required** | 16GB | 256GB | **4–8GB** |
| **Storage** | ~2TB | ~2TB | **100GB validator / 100MB light** |

---

## Architecture Overview

| Section | Highlights |
|:--------|:-----------|
| **Consensus** | Enhanced PoS + checkpoints, 30s blocks, 67% finality, validator slashing |
| **Execution** | Dual EVM + SVM zones, add new zones without hard fork |
| **Cryptography** | ECDSA (legacy compat) + Dilithium/Falcon/Kyber (post-quantum) |
| **Identity** | W3C DID Core 1.0 — `did:dions:` for aliases, agents, devices |
| **Messaging** | AES-256-CBC + RSA-4096 encrypted messages and channels |
| **Data Availability** | Reed-Solomon erasure coding (64+32 shards), 99.99% DA sampling |
| **Bridge** | Native Ethereum bridge with relayer + consensus-layer HTLC |
| **Light Client** | Headers-only sync in ~5 min, earn 0.5 IOC/block |
| **PoP** | Zero-balance nodes earn 0.001 IOC/block by relaying |
| **Tiers** | 5 stake-based tiers: Basic (1K IOC) to Unlimited (100K IOC) |

---

## Security

| Status | Count |
|:-------|:------|
| **FIXED** | 13 findings (all CRITICAL + HIGH + MEDIUM) |
| **DEFERRED** | 2 findings (LOW priority, no risk) |
| **Codex Verification** | 48/48 PASS across 7 audit rounds |
| **Unit Tests** | 511 across 8 binaries |
| **E2E Testnet** | 28/28 PASS on live testnet |

---

## Build

```bash
cd build
cmake --build . -j$(nproc)
```

**13 binaries:** `dions2d` (validator), `dions2-light` (light client), `dions-cli` (wallet), `dions-explorer`, `dions2-migrate`, `dions2-dns-seeder`, `dions-relayer`, + 7 test binaries

---

<p align="center">
<b>© 2014–2026 I/O Coin</b><br>
<a href="https://wizrig.github.io/DIONS-2.0-Portal/">Interactive Portal</a> · <a href="https://github.com/Wizrig/Dions-2.0">Source Code (Private)</a>
</p>
