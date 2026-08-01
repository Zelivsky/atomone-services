# AtomOne State Sync

Fast node synchronization via state sync protocol. No need to sync from genesis.

## State Sync

```bash
systemctl stop atomoned
SNAP_RPC=https://rpc.atomone.apollo-validator.eu:443
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.atomone/config/config.toml
atomoned tendermint unsafe-reset-all --home /root/.atomone
wget -O $HOME/.atomone/config/addrbook.json "https://snapshots.apollo-validator.eu/atomone/addrbook.json"
sudo systemctl restart atomoned && journalctl -fu atomoned -n1000 -o cat
```

## Snapshot Restore

```bash
sudo systemctl stop atomoned
cp $HOME/.atomone/data/priv_validator_state.json $HOME/.atomone/priv_validator_state.json.backup
rm -rf $HOME/.atomone/data
curl -o - -L https://snapshots.apollo-validator.eu/atomone/snapshots/latest.tar.zst | zstd -d | tar -x -C $HOME/.atomone
mv $HOME/.atomone/priv_validator_state.json.backup $HOME/.atomone/data/priv_validator_state.json
sudo systemctl restart atomoned && journalctl -fu atomoned -n1000 -o cat
```

## Useful Links

- **RPC:** https://rpc.atomone.apollo-validator.eu
- **API:** https://api.atomone.apollo-validator.eu
- **gRPC:** grpc.atomone.apollo-validator.eu:443
- **Snapshots:** https://snapshots.apollo-validator.eu/atomone/
- **Peers:** https://snapshots.apollo-validator.eu/atomone/peers.md
- **Explorer:** https://explorer.apollo-validator.eu
