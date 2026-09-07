# AtomOne Peers

Last updated: 2026-09-07 17:00 UTC | Height: 10,252,065 | Verified: 10/27

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,24de4ebc7c6d7f816b29d78844ffa7a55d161a9a@135.181.78.21:61656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,8e0cfafb7d8f2a9b177b6764e972ee27220a9255@65.21.136.219:23456,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,c74c2fa98acac5c09bbe098ce15f5f806d68dc0e@152.53.139.244:26656,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
