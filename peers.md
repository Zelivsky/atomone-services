# AtomOne Peers

Last updated: 2026-09-26 17:00 UTC | Height: 10,535,263 | Verified: 15/44

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,33d2366d5b564ed2aa0e7f2eea87a6e52f9d471b@8.40.118.99:29956,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,ee1fd53eb73a20d07e82d6626a935e5ad104cde4@135.125.222.121:29956,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
