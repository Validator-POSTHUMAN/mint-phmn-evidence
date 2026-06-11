# PHMN Incident: Evidence And Counter-Evidence

English version of the public evidence package. Russian original: [README.md](README.md).

## How To Use This Document

- This document combines the technical evidence ladder and the operator-provided call description with Olim.
- We are open to counter-evidence, corrections, clarifications, and rational alternative interpretations.
- If you believe any point is incorrect or weak, submit a pull request with a specific correction and links to verifiable sources.
- Do not publish seed phrases, private keys, mnemonics, passwords, access tokens, or private logs without redacting sensitive data.
- Counter-evidence should be verifiable: timestamps, transaction hashes, IP/SSH fingerprints, signed statements, contextual screenshots, log exports, or other reproducible artifacts.
- This document is not a legal conclusion and does not prove any person's identity by itself. It records facts, limitations, and the current interpretation.

## Public Artifacts In This Repository

- [Server investigation report](artifacts/REPORT.md)
- [Attacker address graph](artifacts/phmn-attacker-address-links-2026-06-10.md)
- [Noble pqard router trace](artifacts/phmn-noble-pqard-router-2026-06-10.md)
- [Osmosis to Solana CCTP route](artifacts/phmn-solana-cctp-2026-06-10.md)
- [Secret funding background](artifacts/phmn-secret-funding-background-2026-06-10.md)

Raw system logs are intentionally not published because they may contain sensitive shell history and secret-like material.

---

<!-- START: EVIDENCE_LADDER.md -->


Date: 2026-06-10  
Scope: unauthorized PHMN mint and subsequent sale/bridging activity.  
Primary server: `46.4.101.219` / `payments-2`, user `valoper`.  
Time basis: UTC unless stated otherwise.

This document orders the collected evidence from strongest technical evidence to weaker contextual/circumstantial evidence. It is not a legal conclusion and does not prove a human identity by itself.

## Source artifacts

- Server investigation report:
  [artifacts/REPORT.md](artifacts/REPORT.md)
- Raw system logs archive:
  raw system logs archive is not published; see [artifacts/REPORT.md](artifacts/REPORT.md)
- Raw logs SHA256:
  `327c7077d54edb73605f7d7bf0eed6363916d7517fdd93bf5afe5acfdb721f43`
- Attacker address graph:
  [artifacts/phmn-attacker-address-links-2026-06-10.md](artifacts/phmn-attacker-address-links-2026-06-10.md)
- Noble router analysis:
  [artifacts/phmn-noble-pqard-router-2026-06-10.md](artifacts/phmn-noble-pqard-router-2026-06-10.md)
- Solana CCTP route:
  [artifacts/phmn-solana-cctp-2026-06-10.md](artifacts/phmn-solana-cctp-2026-06-10.md)
- Daily working memory:
  not published; relevant extracted facts are included in this README_EN.md and artifacts

Important handling note: the raw server bundle includes shell history and historical logs with secrets or secret-like material. Do not publish the raw bundle.

## Executive summary

The strongest technical evidence shows:

1. The unauthorized `30000 PHMN` mint happened on-chain at `2026-06-10T09:23:50Z`.
2. The compromised minter transferred `30000 PHMN` to attacker address `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` at `2026-06-10T09:39:20Z`.
3. The mint server has artifacts matching that transfer:
   `/home/valoper/tools/send_drop/stakedrop.json` contains a transfer from
   `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw` to
   `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` for `30000000000`.
4. `153.75.83.51` authenticated to the server with an unknown SSH key and was active during the mint and the `30000 PHMN` transfer.
5. `109.75.50.14` authenticated with the `OlimJon` key and was active throughout the full mint, transfer, sale, and CCTP window.
6. On 2026-06-08, shortly after attacker addresses were prepared on-chain, `109.75.50.14` logged in and an unknown Cloudzy/RouterHosting IP `144.172.95.214` logged in concurrently with a different unknown SSH key.

The strongest technical suspicion for the actual server-side execution during the mint window points to the unknown key/IP `153.75.83.51`. The strongest technical suspicion for coordination or enabling access involves `109.75.50.14` / `OlimJon`, because it was present before and throughout the incident window and overlaps with unknown-key access on both June 8 and June 10.

## Evidence tier 1: strongest direct technical evidence

### 1. Unauthorized mint happened on-chain

Event:

- Time: `2026-06-10T09:23:50Z`
- Tx:
  [91470D322C8C8ABE427C6E0A397880C6A61424832799F3994363A555B4C15E98](https://ping.pub/juno/tx/91470D322C8C8ABE427C6E0A397880C6A61424832799F3994363A555B4C15E98)
- Effect: `30000 PHMN` minted.

Why this is strong:

- This is direct chain evidence.
- It defines the incident time anchor.

Limit:

- On-chain data proves the mint happened, but not which human typed the command.

### 2. Compromised minter transferred 30000 PHMN to attacker Juno address

Event:

- Time: `2026-06-10T09:39:20Z`
- Tx:
  [5AA8C1274E2FC823DCD4DACDC8B951220929D5BA338BFAB57E731CAEBAB737FA](https://ping.pub/juno/tx/5AA8C1274E2FC823DCD4DACDC8B951220929D5BA338BFAB57E731CAEBAB737FA)
- From:
  `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw`
- To:
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
- Amount: `30000 PHMN`.

Why this is strong:

- Direct on-chain movement from the compromised minter to the attacker address.
- This links the unauthorized mint to the attacker-controlled `pqard...` address cluster.

Limit:

- This does not identify the human operator by itself.

### 3. Server-side stakedrop artifact matches the stolen transfer

Event/artifact:

- File:
  `/home/valoper/tools/send_drop/stakedrop.json`
- mtime:
  `2026-06-10 09:40:18Z`
- Content observed in report:
  transfer from
  `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw`
  to
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
  for amount `30000000000`.

Why this is strong:

- The server contains an artifact matching the exact post-mint transfer.
- The timestamp is approximately one minute after the on-chain transfer.

Limit:

- File metadata and current contents do not prove which SSH session created or edited the file.

### 4. Incident-window files and deleted log names align with on-chain timing

Server artifacts:

- Deleted log names in shell history include:
  - `magic-drop-2026-06-10T09:24:53+0000.log`
  - `magic-drop-2026-06-10T09:26:41+0000.log`
  - `magic-drop-2026-06-10T09:27:47+0000.log`
  - `magic-drop-2026-06-10T09:39:21+0000.log`
  - `magic-drop-2026-06-10T09:40:18+0000.log`
- File metadata:
  - `holders.json`: `2026-06-10 09:29:59Z`
  - `stakedrop.json`: `2026-06-10 09:40:18Z`
  - `signed.json`: `2026-06-10 09:40:19Z`
  - `send_drop` directory mtime: `2026-06-10 09:43:26Z`

Why this is strong:

- Server file activity is tightly aligned with the mint and transfer window.
- Deleted log filenames align with critical on-chain moments:
  - mint at `09:23:50Z`
  - attacker gas funding at `09:26:16Z`
  - transfer of `30000 PHMN` at `09:39:20Z`

Limit:

- Bash history has no reliable per-command timestamps.
- Deletion evidence is strong for cleanup behavior, but not for exact human attribution.

### 5. Shell history contains PHMN mint command shape and exact amount

Artifact:

- Public artifact description: [artifacts/REPORT.md](artifacts/REPORT.md)
- File on investigated server:
  `/home/valoper/.bash_history`
- File was created/rewritten:
  `2026-06-10 20:18:09Z`
- Contains:
  - navigation to `/home/valoper/tools/send_drop`
  - `junod tx wasm execute ... mint ...`
  - PHMN contract:
    `juno1rws84uz7969aaa7pej303udhlkt3j9ca0l3egpcae98jwak9quzq8szn2l`
  - explicit amount:
    `30000000000`
  - deletion of incident-window `magic-drop` logs.

Why this is strong:

- The command history matches the technical method needed to mint PHMN.
- It contains the exact amount corresponding to `30000 PHMN`.

Limit:

- Bash history is not timestamped per command and can be edited.
- It cannot reliably attribute the command to a specific SSH session.

## Evidence tier 2: strong SSH/session evidence

### 6. Unknown IP 153.75.83.51 was logged in during mint and transfer

Successful SSH logins:

- IP:
  `153.75.83.51`
- User:
  `valoper`
- Fingerprint:
  `ED25519 SHA256:QrqSOi47hGAMA7GTF/OsZgqFpcK42VxGBpHdTxYGqgM`
- Sessions:
  - `2026-06-10 09:00:38Z -> 09:44:54Z`
  - `2026-06-10 09:09:42Z -> 09:32:30Z`
  - `2026-06-10 09:33:03Z -> 09:43:50Z`
  - `2026-06-10 09:43:58Z -> 09:44:38Z`

Overlap:

- Mint:
  `09:23:50Z`
- Transfer of `30000 PHMN`:
  `09:39:20Z`

Why this is strong:

- This unknown key/IP was active during the exact mint and transfer window.
- The key was not found in current `authorized_keys`, `authorized_keys.bak`, or `/root/.ssh/authorized_keys`.
- This makes it the strongest technical candidate for direct interactive execution during the mint/transfer window.

Limit:

- Logs do not show which commands this session ran.
- The human behind the IP/key is not proven by the server logs alone.

### 7. OlimJon key was logged in throughout the full incident and sale/CCTP window

Successful SSH logins:

- IP:
  `109.75.50.14`
- User:
  `valoper`
- Fingerprint:
  `ED25519 SHA256:G9y8wFIoq5Bm364loZREqKF8tb4uQRasZQD/tCTE+QM`
- Key inventory comment:
  `OlimJon`
- Sessions:
  - `2026-06-10 08:55:02Z -> 11:07:51Z`
  - `2026-06-10 08:59:03Z -> 11:14:06Z`

Overlap:

- `09:23:50Z`: mint
- `09:39:20Z`: transfer to attacker
- `09:48:35Z`: PHMN moved to Osmosis
- `09:50:33Z` and `09:51:23Z`: PHMN moved to Neutron
- `09:54:51Z -> 10:03:22Z`: Osmosis sale
- `09:54:59Z -> 09:56:47Z`: Neutron sale
- `10:06:25Z`: Osmosis USDC sent through Noble/CCTP toward Solana

Why this is strong:

- `OlimJon` was present before, during, and after the entire theft/sale/bridge flow.
- `109.75.50.14` entered 1 minute 35 seconds before `153.75.83.51`.
- Both were concurrently active during the mint/transfer window.

Limit:

- Presence is not command attribution.
- Logs do not prove that `OlimJon` typed the mint/sale commands.
- The strongest direct execution evidence still points to the unknown `153.75.83.51` session by timing.

### 8. 153.75.83.51 used a key absent from later key inventory

Observed fingerprint:

- `ED25519 SHA256:QrqSOi47hGAMA7GTF/OsZgqFpcK42VxGBpHdTxYGqgM`

Search results:

- Not found in current `/home/valoper/.ssh/authorized_keys`.
- Not found in `/root/.ssh/authorized_keys`.
- Not found in `/home/valoper/.ssh/authorized_keys.bak`.

Why this is strong:

- It suggests temporary access, removed access, or a not-yet-found authorization path.

Limit:

- It does not prove who added the key.
- `authorized_keys.bak` has metadata caveats and may not represent exact state at `09:44Z`.

## Evidence tier 3: strong on-chain flow evidence

### 9. Same raw key controls attacker addresses across multiple chains

Same raw key forms:

- Juno:
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
- Osmosis:
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`
- Neutron:
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`
- Noble:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
- Cosmos Hub:
  `cosmos1pqardc39d558mr58nx5m2wgmy448pv9409wz6n`
- AtomOne:
  `atone1pqardc39d558mr58nx5m2wgmy448pv94p9j9vt`
- Akash:
  `akash1pqardc39d558mr58nx5m2wgmy448pv94z7r9rf`
- Stride:
  `stride1pqardc39d558mr58nx5m2wgmy448pv94vww7wl`

Why this is strong:

- Same raw key mapping ties Juno, Osmosis, Neutron, and Noble attacker actions together.
- It shows one coherent attacker-controlled key cluster, not unrelated addresses.

Limit:

- Same-key linkage proves wallet control, not human identity.

### 10. The attacker addresses were funded and prepared before the mint

First confirmed on-chain activity:

- `2026-06-08T08:25:00Z`:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
  received `3.286220 USDC`.
- `2026-06-08T08:40:48Z`:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
  sent USDC through CCTP toward Solana token account
  `AhT5WaWAAoMF48nPssaSFEoygCfsq1N2qq7BTUJqEer`,
  owner
  `39CAqAK85xu24YbTw7LbqnphG1QqReL4PHkcmTX3v1HC`.
- `2026-06-08T09:28:42Z`:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
  funded
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`.
- `2026-06-08T09:29:59Z`:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
  funded
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`.
- `2026-06-08T09:30:21Z`:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
  funded
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`.

Why this is strong:

- Shows preparation of the attacker cluster two days before the mint.
- Noble acted as a router/funding account for Juno, Osmosis, Neutron, and Solana route.

Limit:

- The first confirmed on-chain funding happened before the first observed `OlimJon` server login on 2026-06-08.
- Therefore this does not prove the addresses were created from the minter server during that `OlimJon` session.

### 11. The sale and bridge path is coherent and fast

Flow:

- `2026-06-10T09:48:35Z`:
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
  sent `12000 PHMN` to
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`.
- `2026-06-10T09:50:33Z`:
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
  sent `12000 PHMN` to
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`.
- `2026-06-10T09:51:23Z`:
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
  sent `3000 PHMN` to
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`.
- `2026-06-10T09:54:51Z -> 10:03:22Z`:
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`
  sold `12000 PHMN` in chunks into `8953.791909 USDC`.
- `2026-06-10T09:54:59Z -> 09:56:47Z`:
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`
  sold `2900 PHMN` into `3546.992876 USDC`.
- `2026-06-10T10:04:40Z`:
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`
  returned `12100 PHMN` to
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`.
- `2026-06-10T10:06:25Z`:
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`
  sent `8953.791909 USDC` through Noble/CCTP toward Solana token account
  `AhT5WaWAAoMF48nPssaSFEoygCfsq1N2qq7BTUJqEer`,
  owned by
  `39CAqAK85xu24YbTw7LbqnphG1QqReL4PHkcmTX3v1HC`.

Why this is strong:

- Demonstrates a fast, coordinated liquidation/bridge path after mint.
- This is consistent with prior preparation of gas and routes.

Limit:

- On-chain flow shows wallet control and behavior, not server command attribution.

### 12. Current remaining PHMN balances

Live balance recheck during investigation:

- `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`:
  `15100.000000 PHMN`
- `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`:
  `0 PHMN`
- `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`:
  `0 PHMN`

Separate compromised minter balance:

- `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw`:
  `2032.149313 PHMN`

Interpretation:

- Of the stolen `30000 PHMN`, current known remaining balance on the attacker same-key cluster is `15100 PHMN` on Juno.
- `14900 PHMN` was sold:
  - `12000 PHMN` on Osmosis.
  - `2900 PHMN` on Neutron.
- The `2032.149313 PHMN` on the compromised minter is separate and should not be counted as the remaining balance of the stolen `30000 PHMN`.

## Evidence tier 4: strong circumstantial server evidence

### 13. Unknown Cloudzy IP logged in during OlimJon session on 2026-06-08

Event:

- IP:
  `144.172.95.214`
- Public metadata:
  `214.95.172.144.static.cloudzy.com`, Cloudzy / RouterHosting LLC, AS14956, Utah, US.
- Login:
  `2026-06-08 10:09:22Z -> 10:10:49Z`
- Fingerprint:
  `ED25519 SHA256:cRG3qa72oM91d5bbxZcnjyH+6sfFWMPrzFzxxl6BVWU`
- The key was not found in current key files or `authorized_keys.bak`.
- Local ownership check found no evidence that this IP belongs to POSTHUMAN infrastructure, service hosts, OpenClaw hosts, or known AI-agent hosts.

Why this matters:

- It was a successful unknown-key login, not just a failed scanner attempt.
- It happened while `109.75.50.14` / `OlimJon` was also online.
- It occurred shortly after attacker address preparation on-chain.

Limit:

- The server logs do not show what commands this session ran.
- It happened after the first confirmed on-chain funding of `pqard...` addresses, so it does not prove this session created those addresses.

### 14. 109.75.50.14 appears repeatedly in historical access

Observed:

- `109.75.50.14` / `OlimJon` appears on 2026-06-08 and 2026-06-10.
- Earlier entries in May/June also exist.

Why this matters:

- Shows this was not a one-off accidental login.
- Makes the presence on both the preparation day and incident day more relevant.

Limit:

- Regular legitimate access could also produce repeated historical entries.
- This is suspicious only in combination with the incident timing and unknown-key overlaps.

### 15. 153.75.83.51 and 144.172.95.214 are the strongest unknown-key events

Ranked technical suspicion by timing and key state:

1. `153.75.83.51`:
   - Active during mint and `30000 PHMN` transfer.
   - Unknown key `Qrq...`.
   - Key absent from inventory.
2. `144.172.95.214`:
   - Unknown Cloudzy key `cRG3...`.
   - Concurrent with `OlimJon` on 2026-06-08.
   - IP not found in POSTHUMAN infrastructure inventory.
3. `109.75.50.14` / `OlimJon`:
   - Present on 2026-06-08.
   - Present throughout the full 2026-06-10 incident and sale window.
   - Identified key in inventory.

Limit:

- This is a risk ranking, not proof of human identity or guilt.

## Evidence tier 5: weaker but relevant contextual evidence

### 16. Witness observation: attacker address appeared in browser history

Operator-reported fact:

- Multiple team members reportedly saw an attacker-related address in Olim's browser history during screen sharing.
- Address reported in the conversation:
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`
- The screen share was reportedly stopped immediately after this was noticed.
- There was no recording.

Why this matters:

- If documented by witness statements, it is relevant non-technical evidence connecting Olim's local browser activity to an attacker address.

Limit:

- Not independently verified by logs collected here.
- No recording was preserved.
- Should be treated as witness testimony, not server/on-chain proof.

### 17. Operator assertion: 109.75.50.14 corresponds to Olim

Operator-reported fact:

- The team states that IP `109.75.50.14` corresponds to Olim.
- Server logs independently prove that `109.75.50.14` authenticated with a key labeled `OlimJon`.

Why this matters:

- The SSH key label supports the operator attribution.
- The IP attribution strengthens context if independently documented.

Limit:

- The server logs prove IP and key label, not the physical person at keyboard.
- IP ownership/location should be supported with ISP/account/VPN/device evidence if used outside internal analysis.

### 18. Operator assertion: related addresses belong to Olim

Operator-reported addresses:

- `juno1eltl6qu6y538vhux3mk3pjpn7redx8najm4u3e`
- `CjHc2hJUxHmYrg9wTttkCekMgQCCa3L3Mhgq8dbrDNPb`
- `cosmos187jy6cderlfwwttds33e6cdzntx0z0gh9qrhee`
- `osmo187jy6cderlfwwttds33e6cdzntx0z0ghdms80t`

Why this matters:

- These addresses may connect to earlier PHMN/DAS and cross-chain activity.
- Some investigated flows show shared funding leads, for example via
  `osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q`.

Limit:

- Address ownership claims are not proven by chain data alone.
- This is useful context only if tied to stronger evidence such as exchange KYC, device/browser artifacts, signed messages, or direct account records.

### 19. Common funding hub osmo1m8wg4... links attacker cluster to earlier investigated activity

Address:

- `osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q`

Observed:

- Funded `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`.
- Funded `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`.
- Funded `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`.
- Also funded previously investigated `juno187jy6cderlfwwttds33e6cdzntx0z0ghnjqv79` on 2026-02-14.

Why this matters:

- This is a strong on-chain ownership lead.
- It links the attacker cluster to earlier address activity that may have independent identity context.

Limit:

- Common funding is not the same as proof of ownership by the same person.
- It can indicate a funding hub, service, or wallet controlled by another actor.

### 20. Secret Network funding-chain context

Observed:

- Secret tx:
  [344DDD42B2C83977AFB746E1EF5CF296C1E002A7FF702093937F6618EC09E016](https://www.mintscan.io/secret/transactions/344DDD42B2C83977AFB746E1EF5CF296C1E002A7FF702093937F6618EC09E016)
- Time:
  `2026-06-08T08:22:59Z`
- From:
  `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k`
- To:
  `secret1j2n0znmwyy95jc8auvdr7ydk05ff97sgd8t7rn`
- Amount:
  `56.102115 SCRT`
- Historical Secret txs from `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k`
  contain memo:
  `Relayed POSTHUMAN∞DVS https://posthuman.digital | hermes 1.10.5`
- Local file `posthuman-source-data/agoric/ibc-relayers.md` lists
  `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k` as a POSTHUMAN Secret
  Network relayer address.
- Artifact:
  [artifacts/phmn-secret-funding-background-2026-06-10.md](artifacts/phmn-secret-funding-background-2026-06-10.md)

Why this matters:

- This links an early identified attacker gas funding chain with an address
  that internal POSTHUMAN data attributes to Secret Hermes relayer inventory.
- This is useful context for the origin of the funding route, but it is not
  used as the main line of personal attribution.

Limit:

- This does not prove that Olim controlled the Secret key or initiated the
  transfer.
- The team does not have confirmed evidence of Olim's access to the Secret
  relayer host or key material.
- Therefore this point should not be used as evidence of Olim's involvement.
  At most, it is background on the funding chain.

## Evidence tier 6: facts that reduce alternative explanations

### 21. No other successful SSH logins were found in the incident window

Successful 2026-06-10 logins before/during incident:

- `109.75.50.14` / `OlimJon`
- `153.75.83.51` / unknown key

Later 2026-06-10 logins:

- `185.114.176.59`, known Albert RSA key:
  `SHA256:QAmw2m59Z1D0bV5XTinfbjIEMaICQigNX4BdxTtIENQ`
  after the sale/CCTP window.
- `85.253.152.70`, same known Albert RSA key, later that evening.
- `142.132.158.158`, `posthuman-bot` key, used for read-only investigation.

Why this matters:

- It narrows successful server access during the key incident window to two IP/key sets.

Limit:

- Does not exclude local processes, already-running tmux/screen sessions, stolen tokens/keys, or actions from already-open sessions.
- Does not prove that no other authorization path existed earlier and was cleaned.

### 22. Failed/invalid SSH attempts are noisy but not successful access

Observed:

- Many failed/invalid SSH attempts from unrelated internet scanners.
- No `Accepted publickey` for those scanner attempts in the relevant window.

Why this matters:

- Reduces likelihood that a random scanner from logs is the actor.

Limit:

- Failed attempts are not useful for attribution unless paired with successful access or other evidence.

## Key limitations and gaps

1. No command accounting:
   - `auditd`, `acct`, and `psacct` were inactive.
   - `lastcomm` was unavailable or empty.
2. Bash history is not enough for attribution:
   - no per-command timestamps;
   - file was rewritten at `2026-06-10 20:18:09Z`;
   - can be edited.
3. SSH logs prove IP/key/session, not the human at keyboard.
4. On-chain data proves wallet control and fund flow, not human identity.
5. Browser-history observation currently depends on witness testimony because no screen recording was preserved.
6. `authorized_keys.bak` is useful but not definitive:
   - mtime and birth/change metadata conflict;
   - it should not be treated as an exact snapshot of key state at `09:44Z`.
7. The moment of private-key generation for attacker wallets is not visible on-chain.
   - The earliest provable event is first funding/first use.

## Objective conclusion

Based only on collected technical evidence:

- The theft was executed through the PHMN minter workflow on `payments-2` or with artifacts matching that workflow.
- The attacker wallet cluster is clearly identified:
  - `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
  - `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`
  - `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`
  - `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`
- The strongest direct server-side suspect session for the mint/transfer window is `153.75.83.51` using unknown fingerprint `ED25519 SHA256:QrqSOi47hGAMA7GTF/OsZgqFpcK42VxGBpHdTxYGqgM`.
- `109.75.50.14` / `OlimJon` is strongly suspicious by timing, overlap, and repeated presence:
  - present on 2026-06-08 shortly after attacker address preparation;
  - present during the full 2026-06-10 incident and sale/bridge window;
  - concurrent with unknown-key access on June 8 and June 10.
- The evidence supports a hypothesis of coordinated access or temporary key use.
- The evidence does not yet prove beyond doubt which human typed the mint command.

## Recommended evidence preservation and next steps

1. Preserve the server state:
   - provider snapshot;
   - raw log archive hash already recorded;
   - avoid further interactive edits where possible.
2. Export formal witness statements from everyone who saw the browser history observation:
   - date/time;
   - exact address observed;
   - who was present;
   - what happened immediately after.
3. Preserve external identity evidence for `109.75.50.14`:
   - ISP records, VPN records, device logs, or account ownership evidence if available.
4. Ask Cloudzy/RouterHosting for logs related to `144.172.95.214` if there is a legal/abuse route.
5. Investigate how `Qrq...` and `cRG3...` keys were authorized:
   - filesystem snapshot;
   - deleted file recovery if available;
   - shell/editor histories;
   - `.ssh` metadata;
   - provider disk snapshot before more changes.
6. Rotate/revoke compromised PHMN minter credentials and all unneeded SSH keys.

<!-- END: EVIDENCE_LADDER.md -->

---


## Call With Olim And Counter-Evidence Request

Source: operator-provided description of the call. This is witness/context material, not a server log and not on-chain evidence.

Olim joined the call. Other participants included @cryptoq11, @Albert_OpenTech, @web34ever, @s_orion, and @antropocosmist.

We asked Olim to explain what he was doing on the PHMN mint server on June 8 and June 10. He said that he regularly logs into different servers because he needs to look at scripts and reproduce similar setups for Cheater in Cosmos.

We asked why, after he logged into the PHMN mint server, another SSH key was added and an unknown IP address logged in with that key. This happened 1 minute 37 seconds after Olim logged in, and during the PHMN mint window both Olim's IP and the unknown IP were on the server.

Olim answered that he had logged into the server, but did not remember whether he had logged out. He said he then went hiking and had only just returned, finding the incident already underway.

We asked whether he was ready to share his screen and show information that could prove it was not him.

Olim said that first we should say exactly what information we wanted, and then he would think about whether he could show it.

We said that if we listed the exact data in advance, it could be deleted. We also said we were not interested in his private life and would only look at information related to the connection between his addresses/actions and the attacker's addresses/actions. Olim would decide what to show; we would not be able to see anything he did not choose to show.

Olim continued to insist that we first specify exactly what to show, but we kept our proposal.

Olim shared his screen and asked what to show.

First, we asked him to show his Keplr wallet and all accounts he had there. He showed them briefly, and we asked to inspect some accounts in more detail.

Then we asked him to show what was in his Phantom wallet and how many accounts he had there.

Then we wanted to check his browser history, but he refused.

We then asked him to place the attacker address into the browser address bar.

Olim pasted an address that belonged to the attacker, and a list of links from browser history appeared where that address was present in URLs.

As soon as this appeared, Olim quickly switched to Mintscan and started trying to enter the address there, apparently so Mintscan would create a fresh browser-history entry showing that he had opened the address only now.

We said not to enter it into Mintscan, but to use the browser address bar so the browser history would be shown.

Olim stopped screen sharing, said that we were attacking him, and said he would not show anything else.

We said that five of us had seen links with that address in his browser history. Olim replied that we had not seen anything.

Then we asked him to share his screen, open a terminal, and run this command:

(date -u; whoami; hostname; grep -Eai 'ssh|juno|junod|phmn|mint|wasm|execute' ~/.zsh_history ~/.bash_history 2>/dev/null | grep -Evai 'seed|mnemonic|private|password' | tail -200)

This command would have shown when he had run token mint-related commands from that laptop.

If the command had shown that he had not run PHMN mint commands from that laptop, it would have been strong evidence that he did nothing from that laptop.

Olim refused to run it, said he would not type anything into the terminal, and did not share the screen.

After that, we said that he himself did not want to prove that he was not guilty. Olim said that we had attacked him and left the group call.

We do not understand why Olim did not want to investigate together. If he was innocent, we would have expected him to be willing to show any confirmation of his innocence, rather than refusing to show anything under the argument of private information.

We understand that commands typed into a terminal can be private, but providing command records by time would have been the best evidence of innocence if there were no commands related to the PHMN mint.

---

What speaks in Olim's favor:

- Olim had been with the team for a long time and had never done anything bad.
- Why would Olim damage his own reputation?
- We all know Olim as a decent person, and we all want this to be one large misunderstanding.

---

We would be very glad to receive counter-evidence or at least explanations of what happened.

If all direct and circumstantial evidence receives counter-evidence or a rational alternative interpretation, we will all be glad that Olim is an honest person and that the evidence we provided has no meaning in light of that counter-evidence.

We also separately considered the scenario where this was not done by Olim.

If we assume that Olim is an honest person and had no motivation to harm the team, the following rational explanations are possible:

- The direct executor may have been the unknown session from IP 153.75.83.51, which was active during the mint and token-transfer window. In that scenario, the OlimJon key may have been only a parallel session that did not execute mint commands.
- The attacker addresses may have been prepared in advance by a separate external actor. In that scenario, the Secret/Noble/Osmosis funding route shows how attacker wallets were prepared, but does not itself prove which human was behind that preparation.
- The OlimJon SSH key may have been compromised, copied, reused, or used by someone other than Olim. Server logs show the fact of login with a specific key and IP, but they do not by themselves prove which human physically performed the actions.
- Someone else may have had access to the minter server, minter key material, scripts, or saved commands. In that scenario, the attacker did not need access to the Secret relayer key; access to the server or credential that allowed the mint/transfer workflow would be enough.
- The presence of the attacker address in Olim's browser history may have an innocent explanation if the address was opened only after the team had started investigating and discussing the addresses. Without a preserved screen recording or signed witness statements, this remains weak evidence.

What Olim could provide as counter-evidence or explanations of non-involvement:

- A written timeline of his actions on June 8 and June 10, including timezone: when he was at the computer, when he connected to servers, when he left, and when he returned.
- A list of his devices, networks, VPNs, and IP addresses used on June 8 and June 10. It is especially important to explain whether 109.75.50.14 belongs to him, his VPN, ISP, router, or a shared/NAT network.
- Information about the OlimJon SSH key: where the private key was stored, who else could access it, whether it could have been copied, whether it was used from other devices, when it was created, and where it was used.
- Browser-history export for June 8-10 with timestamps, especially around attacker addresses. This would help determine whether those addresses were opened before the incident, during the incident, or only after the team began investigating.
- Local SSH/client/system log exports from his machine: shell history, terminal history, tmux/screen logs, SSH client logs, known_hosts, ssh-agent usage information, and OS login/session timeline.
- Logs or information about VPN/RDP/AnyDesk/TeamViewer/Chrome Remote Desktop and other remote-access tools, to check whether someone could have controlled his device or used his keys.
- Technical time alibi: device sleep/offline logs, router/DHCP/Wi-Fi logs, mobile/location timeline, call records, or screen-sharing timestamps if they contradict interactive server work during the critical window.
- Explanation of address secret1f68m5x8qkyn4wrj3kh452xw2lvupcrfn555tjp if it is his Secret Network address: why it was used, what transactions it had, and why it is not connected to the attacker cluster.
- A list of his known wallets on Juno, Osmosis, Neutron, Noble, Secret, and Solana. If needed, ownership of benign addresses can be proven with signed messages, but seed phrases, private keys, or mnemonics must never be shared.
- Any information that helps identify the unknown executor: who could have used 153.75.83.51, who could have added the unknown SSH key, where 144.172.95.214 came from, and who else had access to the minter server or minter key material.

The strongest counter-evidence for Olim would be not just saying "it was not me", but helping explain the unknown key/IP 153.75.83.51 and showing that his own key, device, or browser history is not connected to the mint execution and token transfer.

We have not received such counter-evidence yet. If it exists, please provide it as soon as possible, so Olim's good name is not harmed unnecessarily.

