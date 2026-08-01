# AtomOne Peers

Updated: 2026-08-01 04:00 UTC | Height: 9,694,537 | Verified: 20/40

## Quick Setup

```bash
PEERS="57e11247cd5c12420c37e68fe3157bc51ca84ca3@78.46.79.242:26756,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,7e804bdafb01244cbe82c96e768d88238603e1da@144.76.29.90:61156,33d2366d5b564ed2aa0e7f2eea87a6e52f9d471b@8.40.118.99:29956,f19d9e0f8d48119aa4cafde65de923ae2c29181a@65.109.35.107:61656,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,089a0896841ef7757f72ca9bd57de616cdfd95e5@65.109.18.169:14556,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,3bfca1233c3692985880e290fc598f15515adf5b@95.217.141.114:14556,e93fbb087acb7c0f8ca850a796310bb745b510b6@23.227.223.101:26656,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,2633ca24776531e8c6a948a6a0d56a83bda02354@atomone-mainnet-peer.oshvank.xyz:55656,dc92e7b8ed2aafc34c400405916569aad0d990f3@169.155.44.68:26656,8e0cfafb7d8f2a9b177b6764e972ee27220a9255@65.21.136.219:23456"
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml
sudo systemctl restart atomoned
```

## Peer List

| # | Moniker | Host | Port |
|---|---------|------|------|
| 1 | Citizen Web3 | 78.46.79.242 | 26756 |
| 2 | atomone-validator_mainnet-CroutonDigital | 136.243.95.31 | 30656 |
| 3 | [NODERS] | 65.108.236.5 | 14556 |
| 4 | KaLaMuC | 135.181.78.21 | 61656 |
| 5 | atomone-validator_mainnet-CroutonDigital | 65.108.226.232 | 30656 |
| 6 | UTSA_guide | 144.76.29.90 | 61156 |
| 7 | Chandra-atomone-relayer | 8.40.118.99 | 29956 |
| 8 | snap | 65.109.35.107 | 61656 |
| 9 | Tendermint | 169.155.46.27 | 26656 |
| 10 | NODERS.SERVICES-atomone | 65.109.18.169 | 14556 |
| 11 | atomone-relayer_mainnet-CroutonDigital | 5.9.95.101 | 30656 |
| 12 | Tendermint | 23.227.223.49 | 26656 |
| 13 | Tendermint | 164.152.161.227 | 26656 |
| 14 | NODERS.SERVICES-atomone | 95.217.141.114 | 14556 |
| 15 | Tendermint | 23.227.223.101 | 26656 |
| 16 | Tanjira | 207.244.249.192 | 26656 |
| 17 | NODEJUMPER | 144.76.74.73 | 14556 |
| 18 | oshvankrpc | atomone-mainnet-peer.oshvank.xyz | 55656 |
| 19 | Tendermint | 169.155.44.68 | 26656 |
| 20 | STAVR-Service | 65.21.136.219 | 23456 |
