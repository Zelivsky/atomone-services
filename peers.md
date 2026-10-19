# AtomOne Peers

Last updated: 2026-10-19 17:00 UTC | Height: 10,879,863 | Verified: 17/41

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="ad83e79bfc8cc23f8ebfb2dab72e2b5cc55d410d@144.76.74.73:14556,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,11024dd977b88f92432dd27bb671c8ab39caa511@188.190.246.227:26656,522826623d65d27ef3f7db8a5259d003be5a93d3@65.108.229.19:26666,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,d3adcf9eee8665ee2d3108f721b3613cdd18c3a3@23.227.223.49:26656,2633ca24776531e8c6a948a6a0d56a83bda02354@atomone-mainnet-peer.oshvank.xyz:55656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,c74c2fa98acac5c09bbe098ce15f5f806d68dc0e@152.53.139.244:26656,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,b4a148414042d784478e0170fe7de4542a42899d@185.16.39.177:26656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,637077d431f618181597706810a65c826524fd74@103.99.39.82:29956,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,1728955056b6aa8ee8d9c4cd41cd1eeeb1474462@us-peer.silknodes.io:15007,dc92e7b8ed2aafc34c400405916569aad0d990f3@169.155.44.68:26656"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
