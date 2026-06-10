# PHMN Incident: Secret Funding Background

Date: 2026-06-10

## Addresses

- Secret relayer / funding root:
  `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k`
- Secret intermediate:
  `secret1j2n0znmwyy95jc8auvdr7ydk05ff97sgd8t7rn`
- Noble attacker same raw-key:
  `noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za`

## Confirmed on-chain transaction

- Secret tx:
  `344DDD42B2C83977AFB746E1EF5CF296C1E002A7FF702093937F6618EC09E016`
- Height: `25650962`
- Time: `2026-06-08T08:22:59Z`
- Message:
  `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k` ->
  `secret1j2n0znmwyy95jc8auvdr7ydk05ff97sgd8t7rn`,
  `56.102115 SCRT`.
- Fee payer:
  `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k`.
- Account sequence used in that tx: `29149`.

## Attribution context

Internal POSTHUMAN data links this Secret address to relayer inventory, but this is background context only and is not used as proof of any person's involvement:

- Local source-data file
  `posthuman-source-data/agoric/ibc-relayers.md` lists
  `secret1h3grxcfts9uc8lvu9vclhahus2npk24pg0jz5k` as the POSTHUMAN Secret
  Network relayer address.
- Historical Secret transactions from this address include memo:
  `Relayed POSTHUMAN∞DVS https://posthuman.digital | hermes 1.10.5 (https://hermes.informal.systems)`.
- Secret account metadata:
  `account_number=303129`, `sequence=29150`.
  This is an old/high-use relayer-style account, not a fresh attacker account.


## What can and cannot be established

- Cosmos/Secret chains do not store a human creator identity for an address.
  On-chain, the best proxy is the first funding / first transaction history.
- The currently available public REST index does not expose the full creation
  history for this account; the indexed sample starts at high sequence values.
- Internal evidence is enough to treat this as POSTHUMAN Secret relayer inventory context.
- This does not establish who initiated the 2026-06-08 transfer and should not be used as personal attribution evidence.
