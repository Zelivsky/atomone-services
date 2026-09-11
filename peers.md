# AtomOne Peers

Last updated: 2026-09-11 17:00 UTC | Height: 10,311,589 | Verified: 16/45

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="883d672fad793dd87764cd578aa07f390ab500e3@169.155.46.203:26656,24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,493061e543fb83ed06a1e19fe292bdf40e993bb8@65.109.23.55:23456,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,5b7e84d1ce64303de4ee5fb4c2b43af9954038ef@65.21.159.142:61656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,dc92e7b8ed2aafc34c400405916569aad0d990f3@169.155.44.68:26656,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656,f19d9e0f8d48119aa4cafde65de923ae2c29181a@65.109.35.107:61656"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
