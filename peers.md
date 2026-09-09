# AtomOne Peers

Last updated: 2026-09-09 05:00 UTC | Height: 10,274,394 | Verified: 18/44

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,f19d9e0f8d48119aa4cafde65de923ae2c29181a@65.109.35.107:61656,2633ca24776531e8c6a948a6a0d56a83bda02354@atomone-mainnet-peer.oshvank.xyz:55656,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,493061e543fb83ed06a1e19fe292bdf40e993bb8@65.109.23.55:23456,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,5b7e84d1ce64303de4ee5fb4c2b43af9954038ef@65.21.159.142:61656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,57e11247cd5c12420c37e68fe3157bc51ca84ca3@78.46.79.242:26756,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,11024dd977b88f92432dd27bb671c8ab39caa511@188.190.246.227:26656,8e0cfafb7d8f2a9b177b6764e972ee27220a9255@65.21.136.219:23456"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
