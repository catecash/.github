# Catecash project plan

**Updated:** 2026-11-26 Pacific time; incorporates incident evidence recorded on 2026-11-27 UTC.
**Release status:** The mainnet candidate is mining with public peers, explorer, and wallet gateways. The formal reviewed public release decision remains **NO-GO**. Running services and merged code do not by themselves close release gates.

This is the cross-repository plan. Detailed gates remain in the [mainnet checklist](https://github.com/catecash/zebra/blob/main/docs/catecash/mainnet-checklist.md), [wallet plan](https://github.com/catecash/zebra/blob/main/docs/catecash/wallet-plan.md), [pool plan](https://github.com/catecash/catecash-gpu-miner/blob/main/POOL_PLAN.md), and [swap steps](https://github.com/catecash/catecash-bridge/blob/main/docs/solana-swap-implementation-steps.md). Some older plans still describe the state before mining began; reconcile their status text against dated evidence without marking unproven gates complete.

## Outcomes

1. A verifiable independent Equihash chain with frozen Catecash identity, 5,000 CATE subsidy, 25-second target, 240-block coinbase maturity, and reviewed supply accounting.
2. Recoverable, chain-checked wallets that pass funded transparent and shielded send, restore, wrong-chain, and reorg tests on each released platform.
3. Open independent mining with observable node agreement and a pool whose shares, allocations, and payouts can be independently reconciled.
4. Explorer, indexer, gateways, website, and monitoring that avoid key exposure and privileged public RPC. Hosted services run in containers; server secrets are read-only mounts outside build contexts.
5. A separately reviewed native CATE ↔ Solana USDC market. No production swap or bridge is live.

*Done* below means the bounded result has evidence. *In progress* means code or a rehearsal exists while the release gate remains open. There is no committed launch date while critical gates remain open.

## Completed work and current gaps

| Workstream | Status | Evidence of work completed | Next acceptance milestone |
| --- | --- | --- | --- |
| Chain and peering | In progress | [Zebra network support](https://github.com/catecash/zebra/commit/d2eb0577f), [public bootstrap peers](https://github.com/catecash/zebra/commit/9edd1939b), [verified seeds](https://github.com/catecash/zebra/commit/005c3c1b4), and a [settled height-8000 checkpoint](https://github.com/catecash/zebra/commit/03e621bd5) are committed. Private mining began 2026-11-24 UTC; public peers and mining followed. | Frozen consensus contract, high-supply live checks, independent review, reproducible release and signed GO. |
| Network incident and health | In progress | The local 2026-11-26/27 UTC incident report records a competing branch near height 8985, delayed convergence, checkpoint deployment to five RPC nodes and three seeds, and concentrated payouts from the operator's own GPU miner. An incorrect temporary P2P block was reversed. | Diagnose propagation delay and concentration; test reorg and outage response; keep independent tip/supply monitoring. |
| Indexer and gateway | In progress | [Zaino Catecash indexing](https://github.com/catecash/zaino/commit/2ac8f4c5) and [Docker secret exclusion](https://github.com/catecash/zaino/commit/2879e888) are committed. Candidate mainnet chain identity, wide-zero balance, TLS wallet access, and blocked `SubmitBlock` were checked in the [hosted candidate record](https://github.com/catecash/zebra/blob/main/docs/catecash/mainnet-hosted-candidate.md). | Sustained sync, wrong-chain rejection, restart/reindex/reorg and exact large-value comparisons on the current chain. |
| Wallet engine and iOS | In progress | [Zingolib Catecash support](https://github.com/catecash/zingolib/commit/ed64e1189) and [maturity correction](https://github.com/catecash/zingolib/commit/a04206112) are committed. [Mobile integration](https://github.com/catecash/zingo-mobile/commit/df8841663) exists. An operator iPhone received a confirmed 4,999.99985000 CATE sweep; a separate Beta passed focused tests. | Clean-device restore, funded send, wrong-chain and reorg tests; signed release and independently recorded App Store outcome. |
| Browser wallets | In progress | Chrome, Brave, Firefox, and Safari alpha forks exist. [Chrome Ironwood support](https://github.com/catecash/catecash-chrome-wallet/commit/6fb7f32) is committed; [external reward shielding is held](https://github.com/catecash/catecash-chrome-wallet/commit/3a77707) until backup is ready. [Website downloads](https://github.com/catecash/website) identify alpha packages. | Clean-profile recovery of both accounts, funded mainnet sends and shielding, reorg behavior, platform packaging, review. |
| Desktop wallet | In progress | [Zingo PC Catecash branding](https://github.com/catecash/zingo-pc/commit/52f5e7e9) is committed. | Pin and rebuild the Catecash engine; pass funded wallet and release tests on selected desktop targets. |
| Explorer and web | In progress | CateScan has [supply/cadence](https://github.com/catecash/catescan/commit/f339173), a [hash-rate chart](https://github.com/catecash/catescan/commit/2dbb621), and [shielded pool display](https://github.com/catecash/catescan/commit/a3cf078). The website has network and wallet pages and a [pool development page](https://github.com/catecash/website/commit/4020c4e). | Compare explorer figures with independent nodes; align public claims with candidate/release status; promote website changes through dev → test → main checks. |
| Solo miners and pool | In progress | [Apple Silicon miner](https://github.com/catecash/catecash-gpu-miner/commit/d08e0d0), [Windows OpenCL miner](https://github.com/catecash/catecash-pc-gpu-miner/commit/a61cb59), and a [closed coordinator with encrypted operator wallet setup](https://github.com/catecash/catecash-gpu-miner/commit/3493f5d) exist. The prototype ledger does not prove public payouts. | Share verification, per-plan accounting, reconciliation, isolated signer, receipts, multiple-worker and reorg/outage rehearsal. |
| Monitoring | In progress | The [integrity auditor](https://github.com/catecash/catecash-integrity-auditor/commit/995afdf) records observed forks and disagreement; [miner concentration monitoring](https://github.com/catecash/catecash-integrity-auditor/commit/af727c8) is containerized. | Independent host, alert/retention policy, tested outage/divergence/recovery. The auditor is not a consensus validator. |
| Solana swap | In progress | [Design and local HTLC proof](https://github.com/catecash/catecash-bridge/commit/3680bee) are committed. Local devnet CATE claim/refund and Solana local-validator mock-USDC escrow were rehearsed. | Adversarial program tests, wallet Trade and recovery flows, measured timeout margin, independent review, limited public test. |

## Critical path and evidence gates

### 1. Stabilize the running candidate

- [ ] Publish a sanitized dated report with exact node versions, genesis/checkpoint hashes, peer observations, tip agreement, supply and incident follow-up. Keep host credentials and private operational details out of Git.
- [ ] Investigate the height-8985 propagation delay and 91–94% recent block payout concentration. Payout-address share is not a direct hash-power measurement. Document an evidence-based response threshold.
- [ ] Preserve auditor history and rehearse alerts for fork, stale node, lost peer and recovery. Test image rollback on a nonproduction network.

### 2. Freeze and verify the protocol

- [ ] Publish one reviewed manifest and independent vectors for genesis, magic/ports, address and viewing-key formats, branch ID, Equihash, DigiShield, fees, shielded activation, replay separation and upgrade policy. Reconcile the older [protocol plan](https://github.com/catecash/zebra/blob/main/docs/catecash/protocol-plan.md) with the implementation.
- [ ] Capture blocks 4,200 and 4,201 on a controlled chain; compare exact issuance, pool and address totals across independent Zebra nodes, Zaino and Zingolib. If payout rotation spreads rewards, use a separate fixed-address rehearsal for the old single-address limit.
- [ ] Test values above JavaScript's exact-integer range and signed/unsigned 64-bit boundaries, plus live maturity, shielded transfers, restart, reindex and deliberate reorg. Publish expected and observed values.
- [ ] Run an independently operated public testnet with final rules and an issue/fix ledger. Complete independent consensus, supply, wallet key and cross-chain replay review; resolve critical findings.

### 3. Release recoverable wallets

- [ ] Automate node → Zaino → Zingolib → app checks for fresh wallet, backup/restore, transparent and supported shielded sends, wrong chain, maturity, reorg, failover and sustained sync.
- [ ] Complete iOS signing/distribution evidence and store outcome; select Android and desktop targets and publish pinned Catecash bindings with hashes.
- [ ] Complete browser backup migration and clean-profile restore before lifting the external shielding hold or treating alpha packages as production releases.

### 4. Open participation and services

- [ ] Publish node and miner releases with hashes, source provenance, peer diversity and independent sync instructions.
- [ ] Complete pool Equihash share validation, proportional/PPLNS rules, accounting replay, chain reconciliation, signer isolation, payout receipts, outage/reorg tests and independent ledger audit. Publish fees and payout floors before public miners join.
- [ ] Check explorer, gateway, website, faucet and monitoring externally. Use the website [deployment guide](https://github.com/catecash/website/blob/main/docs/deployment.md) for each dev → test → main promotion. Keep the faucet closed until its signer and abuse controls are tested.

### 5. Develop the optional market

- [ ] Finish Solana exact-mint escrow and claim/refund tests; freeze program ID, upgrade authority, token mint, fees and deadlines.
- [ ] Add browser Trade and durable CATE HTLC recovery, with client verification of both chains. Rehearse stopped services, delayed confirmations, reorgs and both claim/refund outcomes.
- [ ] Complete independent program, wallet and recovery reviews and a limited public test before real funds. The older EVM bridge prototype is not this market's settlement path.

## Open decisions

| Decision | Current default | Closing evidence |
| --- | --- | --- |
| Candidate mainnet public release | NO-GO | Launch checklist, independent review, signed artifacts and operator sign-off. |
| Final network/address parameters | Candidate values | Versioned contract and cross-component positive/negative vectors. |
| Public pool fee, payout floor and cadence | Pool closed | Published rules, payout reconciliation and approved signer design. |
| Browser mainnet spending/external shielding | Alpha restrictions | Funded recovery/send tests and security review. |
| Production Solana program and deadlines | No public swaps | Audit, measured timing margin and outage/recovery report. |

For each milestone, link its commit or release, dated test report, observed result and unresolved findings. Update this plan after incidents and before public release claims. Publish sanitized evidence and hashes; keep private records out of the repository.
