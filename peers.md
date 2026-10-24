# AtomOne Peers

Last updated: 2026-10-24 11:00 UTC | Height: 10,950,847 | Verified: 17/45

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,089a0896841ef7757f72ca9bd57de616cdfd95e5@65.109.18.169:14556,522826623d65d27ef3f7db8a5259d003be5a93d3@65.108.229.19:26666,11c331c2c1c95b9f1bf33814d5f8871913b79492@207.244.249.192:26656,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,c74c2fa98acac5c09bbe098ce15f5f806d68dc0e@152.53.139.244:26656,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,8e0cfafb7d8f2a9b177b6764e972ee27220a9255@65.21.136.219:23456,9278b21f84f7d6e749130137e875dfd36c013db5@80.47.6.22:26656,11024dd977b88f92432dd27bb671c8ab39caa511@188.190.246.227:26656,1728955056b6aa8ee8d9c4cd41cd1eeeb1474462@us-peer.silknodes.io:15007"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
