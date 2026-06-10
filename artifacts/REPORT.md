# PHMN minter host investigation, 2026-06-10

Host: `46.4.101.219` / `payments-2`  
User checked: `valoper`  
Collection mode: read-only SSH collection from BrightWave.

## Artifact inventory

Directory:

`data/phmn-server-investigation-2026-06-10/`

Raw system logs:

`raw-system-logs.tar.gz`

SHA256:

`327c7077d54edb73605f7d7bf0eed6363916d7517fdd93bf5afe5acfdb721f43`

Important note: collected shell history and historical drop logs contain secrets
or secret-like material. Do not publish the raw bundle. Use this report for
sharing and preserve raw files for forensic review only.

## Time basis

Server timezone is UTC in the relevant logs.

Known on-chain incident times from local snapshot artifacts:

- PHMN mint tx:
  `91470D322C8C8ABE427C6E0A397880C6A61424832799F3994363A555B4C15E98`
  at `2026-06-10T09:23:50Z`.
- Compromised minter transferred `30000 PHMN` to attacker:
  `5AA8C1274E2FC823DCD4DACDC8B951220929D5BA338BFAB57E731CAEBAB737FA`
  at `2026-06-10T09:39:20Z`.
- Attacker started IBC movement from Juno to Osmosis at
  `2026-06-10T09:48:35Z`.

## SSH evidence

### 2026-06-08 focused review

`109.75.50.14` authenticated as `valoper` with the same `OlimJon`
fingerprint:

`ED25519 SHA256:G9y8wFIoq5Bm364loZREqKF8tb4uQRasZQD/tCTE+QM`

Observed sessions:

- `10:07:18Z -> 10:28:49Z` on pts/0 and pts/3.
- `10:11:15Z -> 10:28:49Z` on pts/4 and pts/5.
- `16:19:18Z -> 16:19:34Z` on pts/0 and pts/3.

During the first `109.75.50.14` session window, a separate IP also
authenticated as `valoper`:

- `144.172.95.214`
- `10:09:22Z -> 10:10:49Z`
- key fingerprint:
  `ED25519 SHA256:cRG3qa72oM91d5bbxZcnjyH+6sfFWMPrzFzxxl6BVWU`

The `cRG...` key was not found in the current key search output or in
`authorized_keys.bak`.

Local ownership check for `144.172.95.214`:

- Not found in the POSTHUMAN knowledge base, memory, OpenClaw configs,
  session references, SSH known hosts, or local SSH configuration except as
  part of this PHMN incident investigation.
- Not listed as a known validator node, service host, BrightWave/OpenClaw
  host, or AI-agent host.
- Public IP metadata identifies it as
  `214.95.172.144.static.cloudzy.com`, Cloudzy / RouterHosting LLC,
  AS14956, Utah, US.
- Assessment: no evidence currently links `144.172.95.214` to documented
  POSTHUMAN infrastructure. It appears to be an external VPS/hosting IP unless
  it was rented ad hoc outside the documented inventory.

No sudo commands from the `109.75.50.14` windows were found in
`auth.log`/journal. File metadata checks found no files modified under
`/home/valoper` during:

- `2026-06-08 10:02Z -> 10:34Z`
- `2026-06-08 16:14Z -> 16:25Z`

Only one unrelated-looking file under `/home/valoper` was modified during the
full 2026-06-08 UTC day:

- `/home/valoper/.npm/_logs/2026-06-08T00_00_01_706Z-debug-0.log`

Audit/process accounting was not available:

- `auditd`: inactive
- `acct`: inactive
- `psacct`: inactive
- `lastcomm`: unavailable or empty

Assessment for 2026-06-08:

- Logs prove `109.75.50.14` / `OlimJon` accessed the server.
- Logs also show a short concurrent login from `144.172.95.214` with a
  separate key not found later in key inventory.
- The available system logs do not prove what commands were typed during those
  sessions, because command accounting/audit was not enabled.
- No sudo activity or file modification evidence was found for those windows.

Correlation with attacker address first funding on 2026-06-08:

- `08:25:00Z`: `noble1pqard...` first confirmed funding,
  `3.286220 USDC`.
- `08:40:48Z`: `noble1pqard...` sent USDC via CCTP toward the Solana
  route `AhT5...` / owner `39CA...`.
- `09:28:42Z`: `noble1pqard...` funded same-key `osmo1pqard...` with
  usable Osmosis gas.
- `09:29:59Z`: `noble1pqard...` funded same-key `neutron1pqard...`.
- `09:30:21Z`: `noble1pqard...` funded same-key `juno1pqard...`.
- `10:07:18Z`: first observed `109.75.50.14` / `OlimJon` SSH login.
- `10:09:22Z`: `144.172.95.214` SSH login while `OlimJon` was online.

Interpretation: the first confirmed on-chain funding/activation of the
attacker `pqard...` accounts happened before the first observed
`OlimJon` SSH login on 2026-06-08: Noble by about 1h42m, and the
Osmosis/Neutron/Juno gas setup by about 37-39 minutes. The server logs
therefore do not prove that those addresses were created from this server
during the `OlimJon` session. They do prove that `OlimJon` accessed the
server shortly after the attacker accounts were prepared, and that an unknown
Cloudzy/RouterHosting key logged in concurrently during that session.

### IP 109.75.50.14

SSH accepted public key for `valoper`:

- `2026-06-10 08:55:02Z`
- `2026-06-10 08:59:03Z`

Fingerprint:

`ED25519 SHA256:G9y8wFIoq5Bm364loZREqKF8tb4uQRasZQD/tCTE+QM`

The same fingerprint appears in `/home/valoper/.ssh/authorized_keys.bak`
with comment:

`OlimJon`

`last -aiF` shows the 2026-06-10 sessions from `109.75.50.14` stayed open
until `11:07:51Z` and `11:14:06Z`.

Historical log entries show the same IP and fingerprint accessed the host on:

- `2026-06-08 10:07:18Z`
- `2026-06-08 10:11:15Z`
- `2026-06-08 16:19:18Z`
- earlier entries in May/June also exist.

### IP 153.75.83.51

SSH accepted public key for `valoper`:

- `2026-06-10 09:00:38Z`
- `2026-06-10 09:09:41Z`
- `2026-06-10 09:33:03Z`
- `2026-06-10 09:43:58Z`

Fingerprint:

`ED25519 SHA256:QrqSOi47hGAMA7GTF/OsZgqFpcK42VxGBpHdTxYGqgM`

`last -aiF` shows active sessions from `153.75.83.51` during the mint and
post-mint transfer window:

- `09:00:38Z -> 09:44:54Z`
- `09:09:42Z -> 09:32:30Z`
- `09:33:03Z -> 09:43:50Z`
- `09:43:58Z -> 09:44:38Z`

The `Qrq...` fingerprint was not found in the current
`/home/valoper/.ssh/authorized_keys`, `/root/.ssh/authorized_keys`, or
`/home/valoper/.ssh/authorized_keys.bak` during the collection pass.

Current sshd config uses normal key files:

`AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2`

So the most likely explanations are:

- the `Qrq...` key was temporarily present and later removed, or
- another authorized-key file existed at the time and was later removed, or
- a not-yet-found authorization path was used.

No current `authorized_keys2` file was found in the searched locations.

## Authorized keys state

Current `/home/valoper/.ssh/authorized_keys` after operator added bot access:

- `RSA SHA256:QAmw2m59Z1D0bV5XTinfbjIEMaICQigNX4BdxTtIENQ`
  `albert.andrejev@gmail.com`
- `ED25519 SHA256:VoCsZfDI0+RqUY14JsOgp3kh4CErcMTlSCjcQSmq67Y`
  `posthuman-bot`

`/root/.ssh/authorized_keys` contains the same Albert RSA key only.

`/home/valoper/.ssh/authorized_keys.bak` contains older team keys including:

- `ED25519 SHA256:G9y8wFIoq5Bm364loZREqKF8tb4uQRasZQD/tCTE+QM OlimJon`
- BrightWave keys
- other team keys
- Albert RSA key

Caveat: `authorized_keys.bak` has `Modify=2026-06-10 09:44:30Z` but
`Birth/Change=2026-06-10 11:33:33Z`, so it may have been created later with
preserved mtime. Treat it as a useful key inventory artifact, not as complete
proof of the exact file state at 09:44.

## Shell history and PHMN commands

`/home/valoper/.bash_history` was created/rewritten at
`2026-06-10 20:18:09Z`. It contains command history relevant to the incident,
but bash history has no timestamps per command and can be edited or rewritten.

Relevant commands present in history:

- navigation to `/home/valoper/tools/send_drop`
- `nano authorized_keys`
- PHMN balance checks for:
  `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw`
- multiple `junod tx wasm execute ... mint ...` commands against PHMN
  contract:
  `juno1rws84uz7969aaa7pej303udhlkt3j9ca0l3egpcae98jwak9quzq8szn2l`
- explicit mint amounts visible in history:
  - `1250000000`
  - `60000000000`
  - `30000000000`
- `junod tx bank send phmn_owner juno187... 2000000ujuno`
- repeated `sh run.sh`, `send_batch.sh`, and `prepare_batches.sh`
- deletion of incident-window logs:
  - `magic-drop-2026-06-10T09:12:38+0000.log`
  - `magic-drop-2026-06-10T09:14:06+0000.log`
  - `magic-drop-2026-06-10T09:15:37+0000.log`
  - `magic-drop-2026-06-10T09:24:53+0000.log`
  - `magic-drop-2026-06-10T09:26:41+0000.log`
  - `magic-drop-2026-06-10T09:27:47+0000.log`
  - `magic-drop-2026-06-10T09:39:21+0000.log`
  - `magic-drop-2026-06-10T09:40:18+0000.log`

The deleted `magic-drop` filenames align with the on-chain mint/transfer
window:

- mint at `09:23:50Z`
- attacker gas funding at `09:26:16Z`
- transfer of `30000 PHMN` at `09:39:20Z`

Current `/home/valoper/tools/send_drop/stakedrop.json` contains a
`MsgExecuteContract` transfer from compromised minter
`juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw` to attacker
`juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` for amount
`30000000000`.

File metadata:

- `holders.json`: `2026-06-10 09:29:59Z`
- `stakedrop.json`: `2026-06-10 09:40:18Z`
- `signed.json`: `2026-06-10 09:40:19Z`
- `send_drop` directory mtime: `2026-06-10 09:43:26Z`

## Preliminary assessment

Strong direct evidence:

- `109.75.50.14` authenticated as `valoper` with key fingerprint
  `G9y8...`, identified in key inventory as `OlimJon`, before and during
  the incident window.
- `153.75.83.51` authenticated as `valoper` with distinct key fingerprint
  `Qrq...` during the exact incident window.
- The `153.75.83.51` sessions overlap the on-chain mint at `09:23:50Z` and
  the `30000 PHMN` transfer at `09:39:20Z`.
- Shell history contains the exact PHMN mint command shape and the exact
  `30000000000` amount.
- `send_drop` metadata and remaining JSON artifacts match the transfer of
  `30000 PHMN` from compromised minter to attacker.

Strong circumstantial evidence:

- `109.75.50.14` entered 1 minute 35 seconds before `153.75.83.51`.
- Both IPs had concurrent active SSH sessions for the full mint/transfer
  period.
- The `Qrq...` key used by `153.75.83.51` is absent from the current key
  files, suggesting temporary access or later cleanup.
- Bash history includes deletion of multiple `magic-drop` logs from the
  incident window.

## 2026-06-10 server/on-chain correlation

All times below are UTC.

Server sessions:

- `109.75.50.14` / `OlimJon` key:
  - `08:55:02Z -> 11:07:51Z`
  - `08:59:03Z -> 11:14:06Z`
- `153.75.83.51` / unknown key:
  - `09:00:38Z -> 09:44:54Z`
  - `09:09:42Z -> 09:32:30Z`
  - `09:33:03Z -> 09:43:50Z`
  - `09:43:58Z -> 09:44:38Z`

Server-side artifact timestamps:

- deleted `magic-drop` log names include `09:24:53Z`, `09:26:41Z`,
  `09:27:47Z`, `09:39:21Z`, and `09:40:18Z`;
- `/home/valoper/tools/send_drop/holders.json`: `09:29:59Z`;
- `/home/valoper/tools/send_drop/stakedrop.json`: `09:40:18Z`;
- `/home/valoper/tools/send_drop/signed.json`: `09:40:19Z`;
- `/home/valoper/tools/send_drop` directory mtime: `09:43:26Z`.

On-chain events:

- `09:23:50Z`: PHMN mint tx
  `91470D322C8C8ABE427C6E0A397880C6A61424832799F3994363A555B4C15E98`,
  minting `30000 PHMN`.
- `09:26:16Z`: attacker Juno address
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` funded compromised minter
  `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw` with `1 JUNO`.
- `09:39:20Z`: compromised minter transferred `30000 PHMN` to
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`.
- `09:48:35Z`: `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` sent
  `12000 PHMN` to Osmosis address
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`.
- `09:50:33Z`: `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` sent
  `12000 PHMN` to Neutron address
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`.
- `09:51:23Z`: `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0` sent
  another `3000 PHMN` to
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`.
- `09:54:51Z -> 10:03:22Z`: Osmosis address
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp` sold `12000 PHMN`
  through a series of `1000 PHMN` chunks into `8953.791909 USDC`.
- `09:54:59Z -> 09:56:47Z`: Neutron address
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5` sold `2900 PHMN`
  into `3546.992876 USDC`.
- `10:04:40Z`: `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5` sent
  the remaining `12100 PHMN` back to Juno address
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`.
- `10:06:25Z`: `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp` sent
  `8953.791909 USDC` via Noble/CCTP toward Solana token account
  `AhT5WaWAAoMF48nPssaSFEoygCfsq1N2qq7BTUJqEer`, owned by
  `39CAqAK85xu24YbTw7LbqnphG1QqReL4PHkcmTX3v1HC`.

Interpretation:

- `109.75.50.14` / `OlimJon` was connected during the full mint,
  transfer, IBC movement, sale, and Solana CCTP window.
- `153.75.83.51` / unknown key was connected during the mint and the
  `30000 PHMN` transfer from the compromised minter to the attacker Juno
  address. Its visible SSH sessions ended before the later IBC/sale actions.
- The server artifact timestamps align tightly with the mint and transfer
  window, especially `09:24-09:40Z`.
- Current logs still do not provide per-command attribution to a specific SSH
  session.

Current PHMN balances checked after this correlation:

- `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`: `15100.000000 PHMN`.
- `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`: `0 PHMN`.
- `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`: `0 PHMN`.
- Separate compromised minter address
  `juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw`: `2032.149313 PHMN`
  remains, but this is not the remaining balance of the stolen `30000 PHMN`.

Other successful SSH logins on 2026-06-08 and 2026-06-10:

- 2026-06-08:
  - `109.75.50.14`, `OlimJon` key, three accepted publickey logins.
  - `144.172.95.214`, unknown key
    `ED25519 SHA256:cRG3qa72oM91d5bbxZcnjyH+6sfFWMPrzFzxxl6BVWU`, one
    short login from `10:09:22Z` to `10:10:49Z`.
- 2026-06-10 incident window:
  - `109.75.50.14`, `OlimJon` key.
  - `153.75.83.51`, unknown key
    `ED25519 SHA256:QrqSOi47hGAMA7GTF/OsZgqFpcK42VxGBpHdTxYGqgM`.
- 2026-06-10 after the incident window:
  - `185.114.176.59`, known Albert RSA key
    `SHA256:QAmw2m59Z1D0bV5XTinfbjIEMaICQigNX4BdxTtIENQ`, first login at
    `11:28:42Z`, after the PHMN had already been sold and bridged toward
    Solana. Observed sudo commands were log inspection and later unbanning
    the agent host.
  - `85.253.152.70`, same known Albert RSA key, login at `20:59:20Z`.
  - `142.132.158.158`, `posthuman-bot` key, first login at `21:11:54Z`
    for this read-only investigation.

Assessment of other logins:

- No other successful SSH login was found during the mint/transfer/sale
  window.
- The most suspicious non-Olim entries remain:
  1. `153.75.83.51` on 2026-06-10, because it was active during the mint
     and `30000 PHMN` transfer and its key is not found in current/bak key
     inventory.
  2. `144.172.95.214` on 2026-06-08, because it used an unknown key and
     logged in concurrently with an `OlimJon` session shortly after attacker
     address preparation.
- Later 2026-06-10 Albert/bot logins are much less suspicious from timing and
  key identity: they happened after the funds had already moved, and the keys
  are known in the current authorized-key inventory.
- Numerous failed/invalid SSH attempts from unrelated internet scanners appear
  in the logs, but they did not reach `Accepted publickey` and are not
  evidence of successful access.

Important gaps:

- Current evidence does not show a timestamped command log proving exactly who
  typed each shell command. Bash history is not timestamped per command.
- No current key file contains the `Qrq...` key, so the exact mechanism by
  which `153.75.83.51` was authorized still needs deeper disk/history
  analysis or a filesystem snapshot.
- `authorized_keys.bak` has mixed mtime/birth metadata and should not be used
  alone as proof of state at 09:44.

## Immediate recommendations

1. Preserve the server before more interactive work:
   - create provider snapshot if available;
   - do not edit histories or key files further unless necessary for safety.
2. Treat PHMN owner/minter credentials on this host as compromised.
3. Rotate or disable all keys not required for emergency access.
4. Revoke PHMN mint authority if still possible, or move it to a new safe
   address.
5. Export witness statements separately for the browser-history screen-share
   observation.
