# AtomOne Peers

Last updated: 2026-10-09 05:00 UTC | Height: 10,722,521 | Verified: 14/45

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,3faccbcad8b680ab9ff72b584236623b520e2fe1@37.27.114.244:29956,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,c5924524e9d4e51ebe338756117d9472b2739964@157.180.52.245:14656,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,7c3461c5faa01f0728812cffb91a08517bc5b8b1@65.109.30.13:61656,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
