# AtomOne State Sync Guide

Fast node synchronization via state sync protocol. No need to download snapshots manually.

## Prerequisites

- Freshly initialized node (or empty data directory)
- Network connectivity to RPC servers

## Setup

### 1. Initialize Node

```bash
atomoned init "your-moniker" --chain-id atomone-1
```

### 2. Download Genesis

```bash
curl -s https://raw.githubusercontent.com/atomone-hub/atomone/main/genesis/genesis.json > $HOME/.atomone/config/genesis.json
```

### 3. Get Trust Parameters

Get the latest block height and hash:

```bash
# Get latest height
HEIGHT=$(curl -s https://rpc.atomone.apollo-validator.eu/status | jq -r '.result.sync_info.latest_block_height')
echo "Height: $HEIGHT"

# Get block hash
HASH=$(curl -s "https://rpc.atomone.apollo-validator.eu/block?height=$HEIGHT" | jq -r '.result.block.header.app_hash')
echo "Hash: $HASH"
```

### 4. Configure State Sync

Edit `$HOME/.atomone/config/config.toml`:

```toml
[state-sync]
rpc_servers = "https://rpc.atomone.apollo-validator.eu"
trust_height = <YOUR_HEIGHT>
trust_hash = "<YOUR_HASH>"
enable = true
trust_period = "168h"
```

### 5. Set Persistent Peers

```bash
# Get peers from our list
curl -s https://snapshots.apollo-validator.eu/atomone/peers.md | grep -oP '[a-f0-9]+@[0-9.]+:26656' | tr '\n' ',' | sed 's/,$//'

# Set in config.toml
PEERS="<peers_from_above>"
sed -i "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml
```

### 6. Start Node

```bash
atomoned start
```

The node will sync via state sync protocol, which is typically faster than block-by-block sync.

## Verify Sync

```bash
# Check sync status
curl -s https://rpc.atomone.apollo-validator.eu/status | jq '.result.sync_info'

# Should show:
# - "catching_up": false (when fully synced)
# - "latest_block_height": current height
```

## Troubleshooting

### "state sync failed"

- Verify trust_height is not too old (within trust_period)
- Verify trust_hash matches the block at trust_height
- Check network connectivity to RPC servers

### "could not discover peers"

- Add more persistent_peers
- Check firewall allows port 26656

### Sync is slow

- Add more RPC servers to rpc_servers (comma-separated)
- Ensure good network connectivity
- Check disk I/O performance

## Alternative: Snapshot Restore

If state sync doesn't work, you can use snapshots:

```bash
# Download latest snapshot
wget https://snapshots.apollo-validator.eu/atomone/snapshots/latest.tar.zst

# Stop node, extract, restart
systemctl stop atomoned
mv $HOME/.atomone/data $HOME/.atomone/data_backup
mkdir -p $HOME/.atomone/data
zstd -d latest.tar.zst | tar -C $HOME/.atomone -xf -
systemctl start atomoned
```

## Support

- GitHub: [Zelivsky/atomone-services](https://github.com/Zelivsky/atomone-services)
- RPC: https://rpc.atomone.apollo-validator.eu
- Website: https://apollo-validator.eu
