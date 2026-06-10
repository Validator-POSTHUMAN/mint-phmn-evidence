# PHMN Incident: Osmosis to Solana CCTP Route

Date: 2026-06-10

## Source Osmosis transaction

- Transaction: 0591E90876DF697008C1D68D08534DE2A7991DD4C817CE508953C8C42B73327C
- Height: 63747221
- Time: 2026-06-10T10:06:25Z
- Sender: osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp
- Amount: 8953.791909 USDC
- IBC receiver on Noble: noble15xt7kx5mles58vkkfxvf0lq78sw04jajvfgd4d
- Source channel: channel-750

The transaction memo uses Noble Orbiter / CCTP with destination_domain 5,
which is Solana.

## Decoded Solana recipients

- CCTP mint_recipient token account: AhT5WaWAAoMF48nPssaSFEoygCfsq1N2qq7BTUJqEer
- Token account owner wallet: 39CAqAK85xu24YbTw7LbqnphG1QqReL4PHkcmTX3v1HC
- CCTP destination_caller / signer: 6MxMEeH2MQTjk7Cd4JDbJSnTvtpoKJuv374KpQYKJ3Rv
- Solana USDC mint: EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v

## Solana receive transaction

- Transaction: 4DrTnoj7apx7ZEwtsWX3p9jGhej5bUb7Bt7b88Ehi9XQgiBgZ3MuREp3QYptHsM1zmpSSxPq27oyAWqfUywPNuHr
- Slot: 425524288
- Time: 2026-06-10T10:06:55Z
- Signer: 6MxMEeH2MQTjk7Cd4JDbJSnTvtpoKJuv374KpQYKJ3Rv
- Result: token account AhT5... received 8953.646449 USDC after Orbiter fee.

## Monitoring

Local watcher:

- Script: data/phmn-solana-usdc-monitor.js
- State: data/phmn-solana-usdc-monitor-state.json
- Log: memory/phmn-solana-usdc-monitor.log
- Schedule: local crontab, once per minute, with flock.

Watched Solana addresses:

- AhT5WaWAAoMF48nPssaSFEoygCfsq1N2qq7BTUJqEer
- 39CAqAK85xu24YbTw7LbqnphG1QqReL4PHkcmTX3v1HC
- 6MxMEeH2MQTjk7Cd4JDbJSnTvtpoKJuv374KpQYKJ3Rv

Baseline initialized at 2026-06-10T12:36:33Z. Any new signatures should alert
the Telegram operator topic with the Solscan transaction URL and parsed token
balance changes when available.
