<h1 align="center">DIONS 2.0 Architecture</h1>
<p align="center">IOCoin Protocol Upgrade — Full Design Architecture & Chain Comparison<br>
<code>Commit: b90ee5a</code> · March 3, 2026 · Fork Height: 5,730,000 (~October 2026)</p>

<p align="center">
<b>150</b> RPC Methods · <b>511</b> Tests Passing · <b>22</b> Subsystems · <b>52K</b> Lines of Code · <b>13</b> Build Targets · <b>C++20</b>
</p>

---

## Table of Contents

1. [Full Chain Comparison (ETH / SOL / Legacy IOC / DIONS 2.0)](#1-full-chain-comparison)
2. [Consensus & Staking](#2-consensus--staking)
3. [Block Structure](#3-block-structure)
4. [Network & P2P](#4-network--p2p)
5. [Data Availability](#5-data-availability)
6. [Execution Zones (EVM + SVM)](#6-execution-zones-evm--svm)
7. [Ethereum Bridge & HTLC](#7-ethereum-bridge--htlc)
8. [Cryptography & Post-Quantum](#8-cryptography--post-quantum)
9. [W3C DID Identity](#9-w3c-did-identity)
10. [DIONS Messaging & Storage](#10-dions-messaging--storage)
11. [Light Client](#11-light-client)
12. [Storage Architecture](#12-storage-architecture)
13. [Reward Structure](#13-reward-structure)
14. [Security Hardening](#14-security-hardening)
15. [Source Code & Build Targets](#15-source-code--build-targets)

---

## 1. Full Chain Comparison

### Consensus

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Mechanism | Gasper (Casper FFG + LMD GHOST) | Tower BFT + Proof of History | PoS v2 (Peercoin-derived) | **Enhanced PoS + DAS + checkpoints** |
| Block Time | 12s slot | 400ms slot | 16–60s adaptive | **30s fixed** |
| Min Stake (Validate) | 32 ETH (~$96K) | ~1 SOL + extreme hardware | Any (coin-age) | **10,000 IOC** |
| Finality | ~13 min (2 epochs) | ~400ms optimistic, ~13s confirmed | None (probabilistic) | ~50 min (checkpoints, 2/3 stake) |
| Slashing | Yes (inactivity + proposer) | No native | None | Yes (double-sign, downtime, Byzantine) |
| Delegation | No native (Lido pools) | Native | None | Native (7-day unbonding) |
| Earn from Zero Balance | No | No | No | **Yes — PoP relay rewards** |

### Performance

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| L1 TPS | 15–30 | ~400 real | ~3–5 | **88 (L1 settlement)** |
| With Zones / L2 | Thousands (rollups) | ~4,000 theoretical | N/A | **11,000+ (EVM + SVM zones)** |
| Finality Time | ~13 minutes | ~13 seconds | Unpredictable | 30 seconds (1 block) |
| Scaling Strategy | Rollups (L2), danksharding | Parallel execution (Sealevel) | None | Modular zones (add without fork) |

### Smart Contracts & Execution

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Virtual Machine | EVM | SVM (Sealevel / BPF) | None | **EVM + SVM zones** |
| Languages | Solidity, Vyper | Rust, C | N/A | Solidity + Rust/C (BPF) |
| EVM Compatibility | Native | None | None | Ethereum JSON-RPC layer |
| EIP Support | All | N/A | N/A | EIP-1153 + EIP-2200 + EIP-2929 |
| Dispute Resolution | N/A (all execute) | N/A | N/A | SVM DisputeModule (200-block challenge) |

### Cryptography

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| TX Signing | ECDSA secp256k1 | Ed25519 | ECDSA secp256k1 | ECDSA secp256k1 (legacy compat) |
| Consensus Signing | BLS12-381 | Ed25519 | ECDSA | ECDSA secp256k1 |
| Post-Quantum | None (ETH2030 roadmap) | None | None | **Dilithium + Falcon + SPHINCS+ + Kyber** |
| Hybrid Signatures | No | No | No | **PQC + Ed25519 dual verification** |
| Hash Functions | Keccak-256 | SHA-256 | SHA-256d | SHA-256 + Keccak-256 (OpenSSL EVP) |

### Identity & Messaging

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Native Identity | No (ENS = off-chain contract) | No (SNS = off-chain) | DIONS aliases (O(n) scan) | **W3C DID Core 1.0 (`did:dions:`)** |
| Identity Types | N/A | N/A | Alias only | Agent + Device + Alias |
| Stealth Addresses | No native | No | No | Native v1 + v2 (PQC hybrid) |
| Native Messaging | No (XMTP = off-chain) | No | Basic encrypted | Enhanced (AES-256 + RSA-4096, channels) |
| File Storage | No (IPFS = off-chain) | No | No | Native on-chain with tier quotas |

### Data Availability

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Strategy | Proto-danksharding (EIP-4844) | Full replication | Full replication | **Reed-Solomon erasure coding** |
| Erasure Coding | Roadmap (full danksharding) | No | No | Yes (64 data + 32 parity shards) |
| Per-Node Storage | ~2TB | ~2TB | 2TB+ | **1–10% of data (~50GB)** |
| Light Client DA | Sync committee | No light client | Impossible | 20-shard sampling, 99.99% detection |

### Network & Bridge

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Peer Discovery | discv5 | Gossip (Turbine) | 15 seed nodes | **Kademlia DHT (any peer bootstraps)** |
| Light Client | Altair sync committee | Limited (Tinydancer) | Impossible | Native `dions2-light` binary |
| Light Client Reward | None | None | None | 0.5 IOC/block |
| Bridge | Third-party only | Wormhole ($320M hack) | None | Native Ethereum bridge + relayer |
| HTLC | Smart contract only | Smart contract only | None | Native consensus-layer HTLC |
| WebSocket | eth_subscribe | accountSubscribe | None | Native (port 33776) |

### Hardware Requirements

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Storage | ~2TB SSD | ~2TB SSD | 2TB+ | **Validator: 100GB / Light: 100MB** |
| RAM | 16GB | 256GB (!) | 4GB | **4–8GB** |
| CPU | 8-core | 12+ core | 2-core | 4-core |
| Bandwidth | 25 Mbps | 1 Gbps (!) | Low | **Validator: 10 Mbps / Light: 1 Mbps** |

### Known Issues & Risks

| Property | Ethereum | Solana | Legacy IOCoin | DIONS 2.0 |
|----------|----------|--------|---------------|-----------|
| Outages | None major (post-merge) | 7+ major outages (2022–2024) | None reported | Pre-mainnet |
| Centralization Risk | MEV/PBS, Lido ~33% | Extreme HW requirements | Low node count (~50) | Low min stake, light client rewards |
| Quantum Vulnerability | BLS, KZG, ECDSA all vulnerable | Ed25519 vulnerable | ECDSA vulnerable | **PQC-ready (Dilithium + Falcon)** |
| Known Vulns | MEV exploitation | Outage-prone, single-client | Stake grinding, O(n) aliases | P2SH hash-only (deferred, low prio) |

---

## 2. Consensus & Staking

### PoS Parameters

| Parameter | Value |
|-----------|-------|
| Block time | `TARGET_SPACING = 30s` |
| COIN | `100,000,000` (8 decimals) |
| Stake min age | 30 seconds |
| Stake max age | 90 days |
| Stake modifier interval | 10 minutes |
| Coinbase maturity | 20 blocks (~10 min) |
| Deep reorg protection | 500-block max depth |
| Fork height | `5,730,000` |
| Kernel hash | SHA256D + time-locked modifiers |

### Validator System

| Parameter | Value |
|-----------|-------|
| Min validator stake | `10,000 IOC` |
| Checkpoint interval | 100 blocks |
| Finality threshold | 67% total stake |
| Unbonding period | 7 days |
| Jail period | 24 hours |
| Equivocation | ECDSA double-sign detection |
| Slashing | 10% stake for invalid execution |
| States | Active / Inactive / Slashed / Jailed / Banned |

### Proof-of-Participation (PoP)

Any node — human, AI, robot, IoT device — can earn IOC from **zero balance** by relaying blocks and transactions to validators. This solves the bootstrapping problem: no exchange listing needed to get started.

| Parameter | Value |
|-----------|-------|
| Reward per node | `0.001 IOC/block` |
| Max participants per block | 50 |
| Cooldown | 10 blocks (~5 min) |
| Max additional emission per block | 0.05 IOC (~3.3% of validator reward) |
| Sybil resistance | DHT PoW (8 leading zero bits on node ID) |

---

## 3. Block Structure

```
                         CBlockHeader (~200 bytes)
    +---------------------------------------------------------+
    | nVersion          (int32)                               |
    | hashPrevBlock     (uint256)   <-- chain linkage         |
    | hashMerkleRoot    (uint256)   <-- transaction root      |
    | nTime             (uint32)    <-- 30s intervals         |
    | nBits             (uint32)    <-- difficulty target      |
    | nNonce            (uint32)                              |
    +---------------------------------------------------------+
    | stakeKernel       (optional<CStakeKernel>)  <-- PoS     |
    | vchBlockSig       (vector<uint8>)  <-- NOT in hash      |
    | vchValidatorPubKey(vector<uint8>)                       |
    +---------------------------------------------------------+

                           CBlock
    +---------------------------------------------------------+
    | CBlockHeader (above)                                    |
    | vtx[]            (vector<CTransaction>)                 |
    |   vtx[0] = coinbase (validator reward)                  |
    |   vtx[1] = coinstake (if PoS block)                    |
    |   vtx[2..n] = user transactions                        |
    | nHeight          (cached, not serialized)               |
    +---------------------------------------------------------+

              Modular Commitments in Header
    +---------------------------------------------------------+
    | stateRoot   (uint256) <-- UTXO set merkle               |
    | dataRoot    (uint256) <-- DIONS DA commitment            |
    | zoneRoot    (uint256) <-- EVM + SVM state roots          |
    +---------------------------------------------------------+
```

### Transaction Types

| Type ID | Name | Description |
|---------|------|-------------|
| 0 | Standard | IOC transfer (UTXO model, Bitcoin-compatible) |
| 1 | Alias | Create/update DIONS name alias |
| 2 | Message | Encrypted DIONS message |
| 3 | File | On-chain file storage |
| 4 | Group | Multi-party channel operation |
| 5 | HTLC | Hash time-locked contract |
| 6 | Slash Evidence | Validator misbehavior proof |
| 7 | Dispute Challenge | SVM execution dispute |
| 8 | Dispute Response | Validator counter-proof |
| 9 | Participation Claim | PoP relay reward claim |

---

## 4. Network & P2P

### Ports

| Port | Service |
|------|---------|
| `33764` | Mainnet P2P |
| `33765` | Mainnet RPC |
| `33763` | Testnet P2P |
| `33766` | Testnet RPC |
| `33776` | WebSocket |
| `8377` | Chain ID (mainnet) |
| `8378` | Chain ID (testnet) |

### Protocol

| Parameter | Value |
|-----------|-------|
| Version | `70000` |
| Min peer version | `60000` |
| Max connections | 125 |
| Mainnet magic | `0xfbc0b6db` |
| Testnet magic | `0x0709110b` |
| DHT k-bucket size | 20 nodes |
| DHT refresh | 10 minutes |

### Kademlia DHT

- 160-bit node IDs (SHA-256 of pubkey), 160 k-buckets
- Messages: PING, PONG, FIND_NODE, FOUND_NODES, STORE, FIND_VALUE, FOUND_VALUE
- Sybil-resistant: 8-bit PoW on node ID validated in VERSION handshake
- Any peer can bootstrap the network — no hardcoded seed dependency

---

## 5. Data Availability

```
     Block Data (DIONS payload)
           |
     Reed-Solomon Encode
           |
    +------+------+------+--- ... ---+------+------+
    | S_0  | S_1  | S_2  |          | S_63 | P_0  | ... | P_31 |
    +------+------+------+--- ... ---+------+------+
    |<--- 64 original shards --->|<-- 32 parity -->|

    Recovery: ANY 64 of 96 shards sufficient
    Light clients: sample 20 random shards per block
    Detection rate: 99.99%+ probability
```

| Parameter | Value |
|-----------|-------|
| Original shards (k) | 64 |
| Parity shards (m) | 32 |
| Total shards | 96 |
| Reconstruction threshold | Any 64 of 96 |
| Per-node storage ratio | 1–10% configurable |
| Default retention | 10,000 blocks (~1 week) |
| Maintenance interval | 3,600 seconds |
| DA sampling (light clients) | 20 random shards + Merkle proofs |

---

## 6. Execution Zones (EVM + SVM)

### EVM Zone

- **Engine:** evmone (EVMC interface)
- **Storage:** LevelDB (`dataDir/evm`)
- **TPS:** 1,000+
- **EIP-1153:** Transient storage
- **EIP-2200:** SSTORE with original value tracking (9 statuses)
- **EIP-2929:** Warm/cold access gas metering
- **JSON-RPC:** `eth_blockNumber`, `eth_getBalance`, `eth_call`, `eth_sendTransaction`, `net_version`, `web3_clientVersion`

### SVM Zone

- **Engine:** Solana BPF interpreter
- **Account model:** Lamports, executable, owner
- **PDAs:** Program Derived Addresses
- **CPI:** Cross-Program Invocation
- **Built-ins:** System, Token, AssociatedToken, Rent
- **Crypto:** SHA-256, Keccak-256, Ed25519 (all OpenSSL EVP)
- **Dispute:** 200-block challenge, 100 IOC min bond, 10% slash

Both zones post state roots to L1 via `CBlockHeader::zoneRoot`. New zones can be added without a hard fork.

---

## 7. Ethereum Bridge & HTLC

```
    DIONS Chain                         Ethereum
    +----------+                        +-----------+
    |  Lock IOC |  ----(Relayer)---->   | Mint ERC20 |
    |  (escrow) |                       |  (IOC20)   |
    +----------+                        +-----------+
    | Unlock IOC|  <---(Relayer)----    | Burn ERC20 |
    |  (release)|                       |  (IOC20)   |
    +----------+                        +-----------+

    Operations: LOCK_DIONS (0x01) → MINT_ETH (0x03)
                BURN_ETH  (0x04) → UNLOCK_DIONS (0x02)

    Escrow: UTXO-based P2PKH (real UTXO selection from TxStore)
    Signing: ECDSA SIGHASH_ALL (Bitcoin-compatible)
    Monitor: Bridge thread polls every 15 seconds
    Pause: Multi-sig with cooldown period
```

### HTLC (Hash Time-Locked Contracts)

| Parameter | Value |
|-----------|-------|
| Mechanism | Atomic swaps with SHA-256 preimage reveals |
| Min lock amount | 100,000 sat (0.001 IOC) |
| Max lock amount | 1,000,000,000 sat (10 IOC) |
| Fee rate | 50 bps (0.5%) |
| Confirmation blocks | 6 |
| States | PENDING → CLAIMED / REFUNDED / EXPIRED |
| Index uniqueness | sender + timestamp in LevelDB key |

---

## 8. Cryptography & Post-Quantum

### Classical Cryptography

| Function | Implementation |
|----------|---------------|
| TX signing | ECDSA secp256k1 (legacy IOCoin compatible) |
| Block signing | ECDSA secp256k1 (signature excluded from block hash) |
| Hashing | SHA-256 + Keccak-256 via OpenSSL EVP |
| Encryption | AES-256-CBC + RSA-4096 |
| Key derivation | BIP32 HD wallets |
| ecrecover | Real secp256k1 key recovery (bridge + SVM) |

### Post-Quantum Cryptography (PQC)

| Algorithm | NIST Level | Key Size | Signature Size | Use Case |
|-----------|-----------|----------|----------------|----------|
| Dilithium-2 | 2 | 1,312 B | 1,387 B | General purpose |
| Dilithium-3 | 3 | 1,952 B | 2,044 B | Robot / Server |
| Dilithium-5 | 5 | 2,701 B | 3,293 B | Maximum security |
| Falcon-512 | 1 | 897 B | ~500 B | IoT (compact sigs) |
| Falcon-1024 | 5 | 1,793 B | ~800 B | High-security IoT |
| Kyber-512/768/1024 | 1/3/5 | 800–1,568 B | N/A (KEM) | Key encapsulation |

### Device Profiles

| Profile | KEM | Signature | Target |
|---------|-----|-----------|--------|
| `IOT_MINIMAL` | Kyber-512 | Falcon-512 | Constrained sensors |
| `IOT_STANDARD` | Kyber-768 | Falcon-512 | Smart devices |
| `ROBOT_STANDARD` | Kyber-768 | Dilithium-3 | Autonomous robots |
| `ROBOT_PREMIUM` | Kyber-1024 | Dilithium-5 | High-value robotics |
| `SERVER` | Kyber-1024 | Dilithium-5 | Infrastructure |

---

## 9. W3C DID Identity

DIONS 2.0 implements the **W3C DID Core 1.0** specification with a native `did:dions:` method. Every alias, agent, and device gets a resolvable DID document.

```json
{
  "@context": ["https://www.w3.org/ns/did/v1", "https://w3id.org/security/v2020"],
  "id": "did:dions:alice",
  "controller": "did:dions:alice",
  "verificationMethod": [{
    "id": "did:dions:alice#key-1",
    "type": "EcdsaSecp256k1VerificationKey2019",
    "controller": "did:dions:alice",
    "publicKeyHex": "02a1b2c3..."
  }],
  "authentication": ["did:dions:alice#key-1"],
  "service": [{
    "id": "did:dions:alice#messaging",
    "type": "DionsMessaging",
    "serviceEndpoint": "dions://alice"
  }]
}
```

| Type | Example | Description |
|------|---------|-------------|
| Agent DID | `did:dions:agent:bot1` | Autonomous entity, owns devices |
| Device DID | `did:dions:device:sensor7` | Delegated to agent |
| Alias DID | `did:dions:alice` | Human-readable identity |
| Key types | ECDSA secp256k1 + RSA-4096 + PQC | Dilithium/Falcon |

---

## 10. DIONS Messaging & Storage

### Access Tiers (Stake-Based)

| Tier | Staked IOC | Messages/Month | Max File | Aliases | Retention |
|------|-----------|----------------|----------|---------|-----------|
| Basic | 1,000 | 50 | 1 MB | 1 | 30 days |
| Standard | 5,000 | 200 | 10 MB | 3 | 30–60 days |
| Premium | 10,000 | 500 | 50 MB | 10 | 60–90 days |
| Enterprise | 50,000 | 2,000 | 100 MB | 50 | 90–180 days |
| Unlimited | 100,000 | 10,000 | 500 MB | 100 | 365 days |

*Basic IOC transfers require no minimum stake — only DIONS features are tiered.*

### Encryption

| Layer | Implementation |
|-------|---------------|
| Symmetric | AES-256-CBC (random key per message) |
| Asymmetric | RSA-4096 (encrypts symmetric key) |
| Integrity | SHA-256 message hash |
| Channels | Multi-party encrypted group messaging |

---

## 11. Light Client

| Parameter | Value |
|-----------|-------|
| Binary | `dions2-light` |
| Sync model | Headers-only (~100MB for full chain) |
| Annual bandwidth | 42 MB/year header sync |
| Sync time | ~5 minutes to full chain |
| DA sampling | 20 random shards per block |
| Reward | 0.5 IOC/block |
| Capabilities | Verify PoS proofs, sample DA, request DIONS data on-demand, DHT participation |

### Async Messages

```
GET_HEADERS, HEADERS, GET_SHARD, SHARD,
GET_DIONS_DATA, DIONS_DATA, GET_TX, TX_DATA
```

---

## 12. Storage Architecture

```
    LevelDB Schema
    +---------------------------+
    | CTransactionStore         |
    |   txid → CTransaction     |
    |   blockHash → [txids]     |
    |   (txid,vout) → CTxOut    |  <-- UTXO set
    |   addr → [(txid,vout)]    |  <-- per-address index
    |   blockHash → undo data   |
    +---------------------------+
    | CDionsDB                  |
    |   alias → AliasData       |  <-- O(1) lookup
    |   msg index               |
    |   file index              |
    |   channel index           |
    |   validator set           |
    |   rate limit counters     |
    +---------------------------+
    | CChainState               |
    |   Block index (in-memory) |
    |   Chain trust scores      |
    +---------------------------+
```

All LevelDB writes check `.ok()` status. `ExtractAddress()` handles P2PKH (25-byte), P2SH (23-byte), and P2PK (34/35/67-byte) with testnet/mainnet version bytes.

---

## 13. Reward Structure

| Role | Reward | Work Required |
|------|--------|---------------|
| Validator | 1.5 IOC/block | Full validation, block production, stake 10K IOC |
| Light Client | 0.5 IOC/block | Header sync + data availability sampling |
| PoP Node | 0.001 IOC/block | Relay blocks & transactions (zero balance OK) |

| Parameter | Value |
|-----------|-------|
| Annual inflation | ~2.5% (at 30s blocks = ~1M blocks/year) |
| Supply model | No halving, no max supply |
| Coinbase maturity | 20 blocks (~10 min) |

---

## 14. Security Hardening

| Finding | Severity | Status |
|---------|----------|--------|
| H-01 Consensus Trust (stability.cpp) | HIGH | FIXED |
| H-02 SVM ELF Parser bounds | HIGH | FIXED |
| PoS stakeKernel not set on block header | CRITICAL | FIXED |
| CheckProofOfStake missing UTXO value | HIGH | FIXED |
| M-01 JSON Parser depth/string/array limits | MEDIUM | FIXED |
| M-04 Partial Send retry loop | MEDIUM | FIXED |
| M-05 Chainstate silent reset | MEDIUM | FIXED |
| OpenSSL EVP null checks (SignMessage + TLS) | CRITICAL | FIXED |
| LevelDB unchecked writes (txstore.cpp) | MEDIUM | FIXED |
| RPC input validation (auth, gas, hex) | MEDIUM | FIXED |
| Equivocation ECDSA verification | HIGH | FIXED |
| WebSocket connection slot race | MEDIUM | FIXED |
| Bridge stub silent true return | HIGH | FIXED |
| M-02 TLS Wiring | MEDIUM | DEFERRED (localhost-only RPC) |
| P2SH hash-only verification | LOW | DEFERRED (pending legacy assessment) |

Codex (OpenAI) A–AV feature verification: **48/48 PASS** across 7 rounds.

---

## 15. Source Code & Build Targets

```
dions-2.0/
├── src/
│   ├── bridge/        Ethereum bridge, HTLC, relayer
│   ├── consensus/     PoS, validation, slashing, dispute, participation
│   ├── crypto/        Post-quantum cryptography (PQC)
│   ├── dions/         Aliases, messaging, DID, files, channels
│   ├── evm/           Ethereum Virtual Machine zone
│   ├── svm/           Solana Virtual Machine zone
│   ├── net/           P2P, DHT, WebSocket, light client
│   ├── primitives/    Block, transaction, uint256
│   ├── rpc/           JSON-RPC server (150 methods)
│   ├── storage/       LevelDB chainstate, transaction store
│   ├── wallet/        HD wallet, staking, stealth
│   └── zones/         Zone bridge interface
├── include/dions/     Public headers (37 files)
├── tests/             511 tests across 7 binaries
├── scripts/           Testnet test scripts
├── contracts/         Solidity (DionsBridge.sol, IOC20.sol, TokenFactory.sol)
├── tools/             CLI, explorer, migration, DNS seeder, stress tests
└── docs/              Auditor review package, HTML docs
```

### 13 Build Targets

| Binary | Description |
|--------|-------------|
| `dions2d` | Full validator daemon |
| `dions2-light` | Light client |
| `dions-cli` | Command-line wallet |
| `dions-explorer` | Block explorer |
| `dions2-migrate` | Legacy IOCoin migration tool |
| `dions2-dns-seeder` | DNS seed node |
| `dions-relayer` | Ethereum bridge relayer |
| `dions2-tests` | 314 core tests |
| `dions2-security-tests` | 36 security tests |
| `dions2-slashing-tests` | 19 slashing tests |
| `dions2-dispute-tests` | 31 dispute tests |
| `dions2-pqc-tests` | 54 PQC tests |
| `dions2-pop-tests` | 22 PoP tests |
| `dions2-stealth-tests` | 35 stealth tests |

---

<p align="center">
DIONS 2.0 — Built for the next decade, not just next year.<br>
Fair launch continuation since 2014. No ICO. No presale. Same IOC.<br><br>
© 2014–2026 I/O Coin
</p>
