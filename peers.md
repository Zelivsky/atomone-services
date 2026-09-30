# AtomOne Peers

Last updated: 2026-09-30 05:00 UTC | Height: 10,587,524 | Verified: 17/45

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,57e11247cd5c12420c37e68fe3157bc51ca84ca3@78.46.79.242:26756,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,1728955056b6aa8ee8d9c4cd41cd1eeeb1474462@us-peer.silknodes.io:15007,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,a823e54691a430d5b000c19b43f468be9841c581@149.86.227.232:12656,9278b21f84f7d6e749130137e875dfd36c013db5@80.47.6.22:26656,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656,8e0cfafb7d8f2a9b177b6764e972ee27220a9255@65.21.136.219:23456"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
