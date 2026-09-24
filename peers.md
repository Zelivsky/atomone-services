# AtomOne Peers

Last updated: 2026-09-24 05:00 UTC | Height: 10,497,907 | Verified: 17/45

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,089a0896841ef7757f72ca9bd57de616cdfd95e5@65.109.18.169:14556,a823e54691a430d5b000c19b43f468be9841c581@149.86.227.232:12656,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,3797a9e77da8c9923516d86ff1b089afe6b67527@152.53.125.167:11656,7e804bdafb01244cbe82c96e768d88238603e1da@144.76.29.90:61156,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,f19d9e0f8d48119aa4cafde65de923ae2c29181a@65.109.35.107:61656,33d2366d5b564ed2aa0e7f2eea87a6e52f9d471b@8.40.118.99:29956"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
