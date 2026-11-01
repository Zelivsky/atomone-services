# AtomOne Peers

Last updated: 2026-11-01 06:00 UTC | Height: 11,067,087 | Verified: 18/43

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="c74c2fa98acac5c09bbe098ce15f5f806d68dc0e@152.53.139.244:26656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,2633ca24776531e8c6a948a6a0d56a83bda02354@atomone-mainnet-peer.oshvank.xyz:55656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,949003fde1dd75c622f7ecb1dae7aba757c9620d@flames.go.ro:42256,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656,089a0896841ef7757f72ca9bd57de616cdfd95e5@65.109.18.169:14556,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,a823e54691a430d5b000c19b43f468be9841c581@149.86.227.232:12656,dc92e7b8ed2aafc34c400405916569aad0d990f3@169.155.44.68:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,637077d431f618181597706810a65c826524fd74@103.99.39.82:29956,883d672fad793dd87764cd578aa07f390ab500e3@169.155.46.203:26656,57e11247cd5c12420c37e68fe3157bc51ca84ca3@78.46.79.242:26756,f19d9e0f8d48119aa4cafde65de923ae2c29181a@65.109.35.107:61656,8e0cfafb7d8f2a9b177b6764e972ee27220a9255@65.21.136.219:23456"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
