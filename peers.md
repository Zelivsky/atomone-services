# AtomOne Peers

Last updated: 2026-10-16 05:00 UTC | Height: 10,827,560 | Verified: 14/45

## How we collect peers

Every 6 hours, peer addresses are gathered from our node and public RPCs.
Each peer is then TCP-probed to confirm it is actually reachable.
Only peers that pass this check are included in the list below.

## Adding peers to your node

```bash
PEERS="57e11247cd5c12420c37e68fe3157bc51ca84ca3@78.46.79.242:26756,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,3643c74c18e19a29f5aebfbf095be4e64a347005@213.136.90.38:27656,ff06353437266b8f3c6b28b88c3cd8d04c5a1fd1@103.66.181.55:12656,c40179b03eb9bba3d7316d4056b3ada6203f28b9@65.108.226.232:30656,9278b21f84f7d6e749130137e875dfd36c013db5@80.47.6.22:26656,6746aa45eeedea6c639f4dd3ad2dc02c092677bd@136.243.95.31:30656,7e804bdafb01244cbe82c96e768d88238603e1da@144.76.29.90:61156,6ec1488c456256ad56946ccd4c5adb8c5c433f63@5.9.95.101:30656,7c3461c5faa01f0728812cffb91a08517bc5b8b1@65.109.30.13:61656,e726816f42831689eab9378d5d577f1d06d25716@169.155.46.27:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.227:26656,043d4f2626baa9d4711626229e6d55e7f967d0c6@95.217.78.121:26356,3797a9e77da8c9923516d86ff1b089afe6b67527@152.53.125.167:11656"

# Edit config.toml
sed -i.bak -e "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Restart node
sudo systemctl restart atomoned
```
