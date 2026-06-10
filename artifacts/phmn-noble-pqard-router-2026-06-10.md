# PHMN Incident: Noble pqard Router

Date: 2026-06-10

## Address

- Noble: `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`

This is the Noble bech32 form of the same raw key as:

- Juno: `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
- Osmosis: `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`
- Neutron: `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`

## Incoming funding

### 1. Osmosis -> Noble, from Secret SCRT swap route

- Noble receive tx:
  `CD1DAADE20784BC0913A06FFD8163A3A6C9F7A7199FD0CFEA368D1C5D62EC102`
- Time: `2026-06-08T08:25:00Z`
- Amount received: `3.286220 USDC`
- Packet sender on Osmosis:
  `osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q`
- Packet receiver:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`

Source Osmosis tx:

- `0031D12DA2E94EE4978D53B3A864D81E271B5630DE27AD36848266E20AF521E8`
- Time: `2026-06-08T08:24:58Z`
- Secret packet sender:
  `secret1j2n0znmwyy95jc8auvdr7ydk05ff97sgd8t7rn`
- Amount in: `56.097608 SCRT`
- Swap output: `3.286220 USDC`
- Post-swap IBC receiver:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`

### 2. CCTP receive on Noble

- Noble tx:
  `A54418B0F16DABE449A199AE7083AF2A0DD363FAC7E706912863ED5F7F733A6E`
- Time: `2026-06-08T09:27:41Z`
- Amount received by `noble1pqard...`: `0.587924 USDC`
- Message type: `/circle.cctp.v1.MsgReceiveMessage`
- Fee payer / relayer:
  `noble1dyw0geqa2cy0ppdjcxfpzusjpwmq85r5a35hqe`

## Outgoing actions

### 1. Noble -> Solana CCTP

- Tx:
  `9BFBA27347715A122FF338E8CB29680F9B8F2EF5AA6A746B5307ADC7AE73DCAF`
- Time: `2026-06-08T08:40:48Z`
- Amount burned for Solana CCTP: `3.115769 USDC`
- CCTP destination domain: `5` (Solana)
- Solana token account:
  `AhT5WaWAAoMF48nPssaSFEoygCfsq1N2qq7BTUJqEer`
- Solana token owner:
  `39CAqAK85xu24YbTw7LbqnphG1QqReL4PHkcmTX3v1HC`
- CCTP destination caller:
  `6MxMEeH2MQTjk7Cd4JDbJSnTvtpoKJuv374KpQYKJ3Rv`
- Additional send to Noble route/fee address:
  `150451 uusdc` to `noble1dyw0geqa2cy0ppdjcxfpzusjpwmq85r5a35hqe`
- Tx fee: `20000 uusdc`

This spends the first incoming `3.286220 USDC` exactly:
`3.115769 + 0.150451 + 0.020000 = 3.286220`.

### 2. Noble -> Osmosis gas

- Tx:
  `60352F7BA326ABDF58629BD6C782C14D43B3356E881754808A7EAC2D95974A09`
- Time: `2026-06-08T09:28:42Z`
- Sent: `0.200000 USDC`
- Route: Noble IBC to Osmosis swap-and-action contract
  `osmo10a3k4hvk37cc4hnxctw4p95fhscd2z6h2rmx0aukc6rm8u9qqx9smfsh7u`
- Swap output target:
  `osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp`
- Minimum output: `4.330751 OSMO`
- Tx fee: `0.015327 USDC`

### 3. Noble -> Neutron gas

- Tx:
  `D18773D5DF464D2E4FAB78F8C4426435BEF0D441682A6D49C3CD8285A616C78B`
- Time: `2026-06-08T09:29:59Z`
- Sent: `0.100000 USDC`
- Route: Noble -> Osmosis swap -> Neutron IBC
- Final receiver:
  `neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5`
- Minimum output: `83.915025 NTRN`
- Tx fee: `0.015721 USDC`

### 4. Noble -> Juno gas

- Tx:
  `53D324AEC8ED19FE0FE0959A63C9BC4F93E1F7678E0D484AA2581CE98975AB31`
- Time: `2026-06-08T09:30:21Z`
- Sent: `0.241657 USDC`
- Route: Noble -> Osmosis swap -> Juno IBC
- Final receiver:
  `juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0`
- Minimum output: `7.673743 JUNO`
- Tx fee: `0.015219 USDC`

These three gas-distribution transactions spend the second incoming
`0.587924 USDC` exactly:
`0.200000 + 0.015327 + 0.100000 + 0.015721 + 0.241657 + 0.015219 = 0.587924`.

## Assessment

- `noble1pqard...` is not merely related by routing. It is the same raw-key
  attacker account on Noble and directly funded the same raw-key attacker
  accounts on Osmosis, Neutron, and Juno.
- It also sent USDC to the same Solana token account route later used by the
  attacker cluster:
  `AhT5...` / owner `39CA...`.
- Functionally, `noble1pqard...` acted as the pre-attack funding/router
  address for gas and Solana CCTP movement on 2026-06-08.
