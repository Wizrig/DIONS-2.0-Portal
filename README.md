# DIONS 2.0 Portal

Public portal source for the DIONS 2.0 architecture and retail-readiness page.

Live page: https://iocoin.io/dions20/

Core protocol repo: https://github.com/Wizrig/Dions-2.0

## Purpose

This portal should explain where DIONS 2.0 is going for retail release without overclaiming unfinished proof. It is not just a historical architecture page. It should be kept aligned with the current retail-release contract from the DIONS 2.0 core work:

- Legacy IOCoin/DIONS Proof-of-Stake continuity remains the launch block-production base.
- Heavy transaction load must produce near-16-second confirmations through deterministic, committed-load difficulty/target behavior.
- Sustained 4 MB block pressure must be proven with real mixed feature traffic.
- Enterprise/master-wallet operation must stay fast as wallet history grows.
- Validator, Light service, and PoP rewards must be paid and visible.
- Explorer network, graph/general mempool, reward split, block fill, and feature activity must remain live and accurate.
- StampRight-style enterprise record storage is a primary product target, not an afterthought.

## Current Portal Update Needed

The live page at `https://iocoin.io/dions20/` still contains older May 2026 material and should be revised before it is treated as the public DIONS 2.0 retail page.

Stale or risky live-page claims that need updating:

- `May 2026 source material` should become a current release-readiness status.
- `Tests 741/741`, `511 tests`, and similar old numbers should be replaced with the current gate model, not a static test count.
- `RPC 150 methods` should either be updated from the current source or described as broad RPC compatibility without a stale number.
- `30s target cadence`, `fixed cadence`, and similar language should be replaced with legacy adaptive PoS plus near-16-second target behavior under load.
- `88 L1 TPS`, `11,000+ with zones`, `1,000+ TPS EVM`, and `10,000+ TPS SVM` should be removed or marked as design targets until benchmark artifacts exist.
- `67% finality`, validator slashing, and checkpoint/finality language should not describe launch consensus. Launch consensus is legacy PoS continuity with probabilistic confirmations.
- `Falcon + Kyber` should not be presented as production-active if the current core status is Dilithium3/ML-DSA active with Falcon header-declared and other PQC paths gated.
- Bridge/custody claims should stay monitor-only/fail-closed until separately activated and proven.
- `100 MB light disk` and other exact light-client resource claims need fresh measurement or should be presented as targets.
- The page should link to the current DIONS 2.0 source repo under `Wizrig`, not old or ambiguous source links.

## Retail Release Contract

DIONS 2.0 is retail-ready only when one clean, pinned SHA proves the following end-to-end:

- Full feature soak PASS across aliases, tokens, DEX, EVM, SVM, messages, private/shadow/group messaging, files/data, graph events, normal IOC sends, wallet backup/import, and key recovery.
- Sustained 4 MB pressure for at least 1 hour.
- Near-16-second confirmations under committed fill-heavy transaction load.
- Difficulty/target eases under real load and re-hardens when idle.
- `msgs_failed=0` during pressure.
- No fork/reorg churn and managed-node spread stays `<=2`.
- Validator reward `1.5 IOC`, Light service reward `0.5 IOC`, and PoP reward `0.001 IOC` are paid and visible in explorer output.
- Public explorer reports the correct target SHA, `same_sha=true`, live network state, graph/general mempool split, reward split, and block-fill/colorgram behavior.
- Bloated-wallet and service-wallet paths remain fast. Clean-wallet benchmarks are diagnostic only.
- Real bloated `wallet.dat` fixture tests pass.
- Legacy staking/change-split behavior does not create unbounded wallet bloat.
- Fresh/restarted nodes catch up and recover without manual fork repair.

Until those gates pass, the portal should describe DIONS 2.0 as an active retail-release candidate and architecture dossier, not a final released network.

## Current Readiness Matrix

| Area | Target | Current status | Required before retail PASS |
|------|--------|----------------|------------------------------|
| Legacy PoS continuity | Stable IOCoin/DIONS-style PoS liveness | Implemented and repeatedly soak-tested on RC lines | Keep proving on final pinned SHA through feature soak, pressure, restart, and wind-down |
| 16s under load | Heavy committed transaction load produces near-16-second confirmations | In progress; fast-floor design exists | Must pass sustained fill-heavy 4 MB run with cadence artifacts and idle re-hardening |
| 4 MB blocks | 1-hour sustained 4 MB-class pressure | In progress; previous runs exposed wallet bottlenecks | Final mixed 4 MB/16s PASS with artifacts |
| Wallet/master-wallet scale | Exchange/StampRight-style master wallets stay fast over years | In active fix cycle; WAL/delta persistence and UTXO-cache fixes being validated | Real bloated wallet.dat fixture, no full Save/RebuildUTXOCache per send, 4 MB proof |
| Staking/change split | Legacy staking behavior without unbounded change fragmentation | Added as explicit gate | Long staking-history regression: reward/change correctness, bounded UTXO growth, stake eligibility preserved |
| Validator rewards | 1.5 IOC validator reward | Implemented and visible on RC explorer | Keep proof on final SHA |
| Light rewards | 0.5 IOC Light service reward | Implemented/service-gated; needs final mixed-run proof | Capture fresh reward and explorer display proof |
| PoP rewards | 0.001 IOC PoP reward | Implemented/service-gated; needs final mixed-run proof | Capture fresh reward and explorer display proof |
| Graph layer | Graph/general mempool split, graph events, DA/shard observability | Implemented as gated/observability surfaces | Include graph pressure in final mixed fuzz and verify explorer output |
| Messaging | Plain, encrypted/private, shadow, and group/channel-style traffic | Passing feature soaks on RC lines | Include under concurrent 4 MB pressure |
| Files/data | On-chain records/data/file metadata paths | Implemented in feature surface | Include file/data pressure in final retail fuzz |
| Aliases/domains/identity | O(1) aliases, DID/agent identity, customer/domain/contact workflows | Implemented surfaces; enterprise scenario proof pending | Multi-user alias/domain/contact fuzz, including StampRight-style records |
| Tokens/DEX | Create, mint, transfer, burn, settlement/DEX paths | Passing feature soaks on RC lines | Include under mixed 4 MB pressure |
| EVM/SVM | DIONS-native contract/program execution | Passing feature soaks on RC lines | Concurrent EVM/SVM load during final mixed fuzz |
| Explorer | Accurate network, reward, graph/general, block-fill UI | Improving; stale writer issues have occurred | Verify after every deploy and final pressure run |
| Sync/IBD/recovery | Restart/new-node catch-up without fork repair | Implemented hardening; final SHA proof pending | Fresh/restart-catchup proof on final SHA |
| Enterprise records / StampRight | High-volume record storage for business workflows | Strategic target | Dedicated enterprise fuzz: many customers, records, aliases, domains, service contacts, node-failure recovery |

## What Has Improved Since The Older Portal Copy

The older portal copy emphasized broad architecture targets. The current release work has improved or clarified several areas:

- Launch consensus is legacy PoS continuity, not the retired quorum/finality experiment.
- 16-second behavior is being treated as a proof gate under real committed load, not a marketing claim.
- 4 MB block capacity is tied to sustained pressure artifacts, not a theoretical size limit.
- Wallet scaling is now a first-class retail blocker. DIONS 2.0 must support bloated master/service wallets, not just clean test wallets.
- Explorer reward display now needs to show latest block reward split accurately: validator, Light service, and PoP service.
- Graph/general mempool split is an explorer observability feature; daemon mempool remains shared.
- Bridge/custody and broad Ethereum/Solana wallet parity remain separately gated until proven.
- StampRight-scale enterprise record storage is part of the retail-readiness definition.

## Portal Content Direction

Recommended live-page update structure:

1. **Hero:** DIONS 2.0 retail-release candidate, fair-launch IOC continuation, legacy PoS continuity, enterprise service rails.
2. **Retail gate:** show what must pass before launch: feature soak, 4 MB/16s, wallet scale, rewards, explorer, sync/recovery.
3. **Architecture:** preserve consensus, graph, identity, messaging, tokens, DEX, EVM/SVM, PQC, bridge, light/PoP sections, but label activation-gated surfaces clearly.
4. **Explorer proof:** include live explorer links for network, graph/general mempool, rewards, and block fill.
5. **StampRight lane:** explain enterprise records, aliases/domains, service contacts, proof receipts, and data storage as a flagship use case.
6. **Status matrix:** Completed / implemented but pending proof / blocked / future-gated.
7. **No fake TPS:** remove old TPS numbers unless backed by current reproducible artifacts.

## Development Notes

This repo currently contains a static portal page plus embed variants:

- `index.html` - main portal page
- `index-legacy.html` - older saved page
- `wix-embed.html`, `wix-embed-stable.html`, `wix-embed-scroll.html` - embed variants
- `tetnet-update-1.html` - historical testnet update file, filename appears misspelled and should be reviewed before public linking

Before pushing public copy updates, verify that no private node IPs, wallet secrets, internal artifact paths, or unpublished operational details are included.

## Build / Publish

This portal is static HTML. Update the HTML files directly, then deploy through the hosting path used by `iocoin.io`.

Basic local check:

```bash
python3 -m http.server 8080
# open http://127.0.0.1:8080/
```

## Release Language Rule

Use precise status language:

- **Implemented** means code exists.
- **Soak-tested** means the feature passed an isolated or full feature soak.
- **Retail-proven** means it passed the final pinned-SHA mixed pressure/recovery gate.
- **Activation-gated** means the surface exists but must not be marketed as live production behavior.
- **Design target** means planned or intended, not yet proven.

Do not call DIONS 2.0 retail-ready until the final feature soak plus bloated-wallet 4 MB/16s proof passes on one pinned SHA.

---

© 2014-2026 I/O Coin. Fair-launch continuation.
