# PHMN Attacker Address Links - 2026-06-10

Concise forensic graph for the pqard attacker key after the unauthorized PHMN mint.

## Attacker Same Raw Key

- Juno: juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0
- Osmosis: osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp
- Neutron: neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5
- Noble: noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za
- Cosmos Hub: cosmos1pqardc39d558mr58nx5m2wgmy448pv9409wz6n
- AtomOne: atone1pqardc39d558mr58nx5m2wgmy448pv94p9j9vt
- Akash: akash1pqardc39d558mr58nx5m2wgmy448pv94z7r9rf
- Stride: stride1pqardc39d558mr58nx5m2wgmy448pv94vww7wl

Confirmed relevant activity in this pass: Juno, Osmosis, Neutron, Noble.

## First-Funding Timeline

- 2026-06-08 08:25 UTC, Noble: noble1pqard... received 3.286220 USDC from Osmosis sender osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q over IBC. Tx CD1DAADE20784BC0913A06FFD8163A3A6C9F7A7199FD0CFEA368D1C5D62EC102.
- 2026-06-08 08:40 UTC, Noble: noble1pqard... used CCTP / USDC flow and sent 0.150451 USDC to noble1dyw0geqa2cy0ppdjcxfpzusjpwmq85r5a35hqe. Tx 9BFBA27347715A122FF338E8CB29680F9B8F2EF5AA6A746B5307ADC7AE73DCAF.
- 2026-06-08 09:27 UTC, Noble: noble1pqard... received 0.587924 USDC from noble1x74lhe0pqqv7rcg4pc4svtxhm9hnf79pxpfqfv. Tx A54418B0F16DABE449A199AE7083AF2A0DD363FAC7E706912863ED5F7F733A6E.
- 2026-06-08 09:28 UTC, Osmosis: osmo1pqard... received 4.352924 OSMO through a Squid/swap-and-action flow initiated from same-key noble1pqard... with 0.200000 USDC. Tx 410C74957AE84243354E9DF0A0F2C6DFEFCA7BF81E0506F40AC97057430BBF73.
- 2026-06-08 09:30 UTC, Neutron: neutron1pqard... received 84.344652 NTRN from osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q over IBC. Tx 58D1C0F7B0ED5D237818251E32DC97A980ADDEFFC011672D723724EF86DC017E.
- 2026-06-08 09:30 UTC, Juno: juno1pqard... received 7.713032 JUNO from osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q over IBC. Tx C8D8C721CE983DAE7F980383148CA2E0C0E654A191FFCECACCE0D95F8EDEADB5.

## Incident Flow

- 2026-06-10 09:26 UTC, Juno: juno1pqard... sent 1.000000 JUNO to compromised PHMN minter juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw. Tx C261BF88CD9A5ECEA0CD1E8999011436BF73FC6D5D7553A518EC0A72EDF3AB52.
- 2026-06-10 09:39 UTC, Juno: compromised minter transferred 30000 PHMN to juno1pqard... Tx 5AA8C1274E2FC823DCD4DACDC8B951220929D5BA338BFAB57E731CAEBAB737FA.
- 2026-06-10 09:48 UTC, Osmosis: osmo1pqard... received 12000 PHMN from juno1pqard... Tx A4207B416F7AB653A0A7B24190D84D51C961285771A0A543D4FFCCBDEA7D5551.
- 2026-06-10 09:50 and 09:51 UTC, Neutron: neutron1pqard... received 12000 PHMN and 3000 PHMN from juno1pqard... Txs 3430AC378068A2E8F845FD935E393BC83C8A9138E57E566E2BE3DA986FA8264A and 6D09574C923CC0503941BEFFFC692F9BA8CCA73CF5C221091678AE08B8F4E133.
- 2026-06-10 09:54-09:59 UTC, Neutron: neutron1pqard... swapped 2900 PHMN to 3546.992876 USDC through Squid / Astroport route. Txs D270A45737B051635702BA90350B22852E096DFBCDAE7120E8D10E2CDF697B20, 26C965D3627E94BB16EF26FC295E6966E6C68249028B1A2292B42DE30E3C47E2, D04E8134D5B5DA9332544253448A340819D9B4F3A0DA8EACF8F069C31A927697.
- 2026-06-10 10:12 UTC, Neutron to Juno: neutron1pqard... sent 12100 PHMN back to juno1pqard... Neutron tx 8629FD25187DC9B8964B1AC189EE0656A5A1C0CA3C0523EFEC0B06375F87EC19, Juno receive tx EE99148C1D9B1853F04D560154A8D29435A9D8DFD6BA6068F31609D7DF252022.

## High-Priority Leads

### osmo1m8wg4...

osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q is the strongest ownership lead found in this pass:

- funded noble1pqard... with 3.286220 USDC;
- funded juno1pqard... with 7.713032 JUNO;
- funded neutron1pqard... with 84.344652 NTRN;
- also funded previously investigated juno187jy6cderlfwwttds33e6cdzntx0z0ghnjqv79 with 39.745643 JUNO on 2026-02-14.

This does not prove identity, but it is a strong common funding hub between the attacker same-key addresses and the juno187 PHMN/DAS address.

### Same-Key Noble Address

noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za is attacker-controlled by same raw key and predates the Juno mint by about two days. It is currently the earliest confirmed same-key address in this investigation.

## Counterparty Classification

Likely attacker-controlled or high-priority:

- noble1pqardc39d558mr58nx5m2wgmy448pv948xm2za - same raw key.
- juno1pqardc39d558mr58nx5m2wgmy448pv94ehdea0 - same raw key.
- osmo1pqardc39d558mr58nx5m2wgmy448pv9487ajvp - same raw key.
- neutron1pqardc39d558mr58nx5m2wgmy448pv94t68qq5 - same raw key.
- osmo1m8wg4vxkefhs374qxmmqpyusgz289wmulex5qdwpfx7jnrxzer5s9cv83q - common funding hub.

Compromised / incident-specific:

- juno100umj2mnu0u6ujf37c9a3xfy9gl53hu9ekxyyw - compromised PHMN minter.

Infrastructure / route contracts observed:

- osmo10a3k4hvk37cc4hnxctw4p95fhscd2z6h2rmx0aukc6rm8u9qqx9smfsh7u - Squid / swap-and-action contract on Osmosis.
- neutron1zvesudsdfxusz06jztpph4d3h5x6veglqsspxns2v2jqml9nhywskcc923 - Squid / swap-and-action contract on Neutron.
- neutron19ag85acwturpvwzrpmfccvztczuj8czdu430c2v7fl68dpwvzs5qc7hpvh - Neutron swap helper.
- neutron1muu00n0ae5z7kwnjfn98naju9p6vrj4msj35netm2ffjqs5wxyts57cwjv - Astroport PHMN/USDC pool.
- juno1v4887y83d6g28puzvt8cl0f3cdhd3y6y9mpysnsp3k8krdm7l6jqgm0rkn - Juno ICS20 Osmosis contract.
- juno1vw6l0gjwju73chh0nqc097347r7ucdx7vshm38qp25xr36wckhqsepcmsd - Juno ICS20 Neutron contract.
- noble1x74lhe0pqqv7rcg4pc4svtxhm9hnf79pxpfqfv and noble12l2w4ugfz4m6dd73yysz477jszqnfughxvkss5 - CCTP-related Noble flow addresses.
