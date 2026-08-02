# AtomOne Mainnet Services

Public infrastructure and tools for [AtomOne](https://github.com/atomone-hub/atomone) mainnet operators by [Apollo Validator](https://apollo-validator.eu).

[![AtomOne](https://img.shields.io/badge/Chain-atomone--1-blue)](https://atomone.apollo-validator.eu)
[![RPC](https://img.shields.io/badge/RPC-rpc.apollo--validator.eu-green)](https://rpc.atomone.apollo-validator.eu)
[![Snapshots](https://img.shields.io/badge/Snapshots-snapshots.apollo--validator.eu-orange)](https://snapshots.apollo-validator.eu/atomone/snapshot.html)

## Services

| # | Service | Endpoint | Status |
|---|---------|----------|--------|
| 1 | [Public RPC](#public-rpc) | `rpc.atomone.apollo-validator.eu` | 🟢 Active |
| 2 | [Public API](#public-api-rest) | `api.atomone.apollo-validator.eu` | 🟢 Active |
| 3 | [gRPC](#grpc) | `grpc.atomone.apollo-validator.eu:443` | 🟢 Active |
| 4 | [Snapshots](#snapshots) | [snapshots.apollo-validator.eu/atomone/snapshot.html](https://snapshots.apollo-validator.eu/atomone/snapshot.html) | 🟢 Active |
| 5 | [State Sync](#state-sync) | [snapshots.apollo-validator.eu/atomone/statesync.html](https://snapshots.apollo-validator.eu/atomone/statesync.html) | 🟢 Active |
| 6 | [Peers List](#peers-list) | [peers.md](peers.md) (auto-updated every 6h) | 🟢 Active |
| 7 | [Installation Guide](#installation-guide) | [guide.md](guide.md) | 🟢 Active |

---

## Public RPC

Base URL: `https://rpc.atomone.apollo-validator.eu`

### Available Endpoints

| Endpoint | Description |
|---|---|
| `/status` | Node sync status, block height |
| `/abci_info` | ABCI application info |
| `/net_info` | Network peers info |
| `/health` | Node health check |
| `/genesis` | Genesis file |
| `/block?height=N` | Block by height |
| `/validators?height=N` | Validator set |
| `/commit?height=N` | Block commit |
| `/tx?hash=H` | Transaction by hash |
| `/broadcast_tx_commit?tx=TX` | Broadcast transaction |
| `/consensus_state` | Consensus state |

### Usage Examples

```bash
# Check node status
curl -s https://rpc.atomone.apollo-validator.eu/status | jq

# Get current block height
curl -s https://rpc.atomone.apollo-validator.eu/status | jq -r '.result.sync_info.latest_block_height'

# Get network peers
curl -s https://rpc.atomone.apollo-validator.eu/net_info | jq -r '.result.n_peers'

# Get block by height
curl -s "https://rpc.atomone.apollo-validator.eu/block?height=9690000" | jq

# Get validators
curl -s "https://rpc.atomone.apollo-validator.eu/validators?height=9690000" | jq

# Broadcast transaction
curl -s "https://rpc.atomone.apollo-validator.eu/broadcast_tx_commit?tx=$(base64 < tx.bin)" | jq
```

### Configuration for Wallets/Tools

```
RPC URL: https://rpc.atomone.apollo-validator.eu
Chain ID: atomone-1
Denom: uatone
```

---

## Public API (REST)

Base URL: `https://api.atomone.apollo-validator.eu`

Cosmos REST API for querying accounts, validators, staking, governance, and more.

### Usage Examples

```bash
# Get node info
curl -s https://api.atomone.apollo-validator.eu/cosmos/base/tendermint/v1beta1/node_info | jq

# Get validator set
curl -s https://api.atomone.apollo-validator.eu/cosmos/staking/v1beta1/validators | jq

# Get account balance
curl -s https://api.atomone.apollo-validator.eu/cosmos/bank/v1beta1/balances/YOUR_ADDRESS | jq

# Get staking params
curl -s https://api.atomone.apollo-validator.eu/cosmos/staking/v1beta1/params | jq

# Get block by height
curl -s https://api.atomone.apollo-validator.eu/cosmos/base/tendermint/v1beta1/blocks/9690000 | jq
```

---

## gRPC

Endpoint: `grpc.atomone.apollo-validator.eu:443`

gRPC endpoint for high-performance queries and transactions.

### Usage Examples

```bash
# Query with grpcurl
grpcurl -plaintext grpc.atomone.apollo-validator.eu:443 list

# Get node info
grpcurl -plaintext grpc.atomone.apollo-validator.eu:443 cosmos.base.tendermint.v1beta1.Service/GetNodeInfo

# Get block
grpcurl -plaintext grpc.atomone.apollo-validator.eu:443 cosmos.base.tendermint.v1beta1.Service/GetBlockByHeight -d '{"height": 9690000}'
```

---

## Snapshots

Page: [https://snapshots.apollo-validator.eu/atomone/snapshot.html](https://snapshots.apollo-validator.eu/atomone/snapshot.html)

| Latest Snapshot | |
|-----------------|---|
| **Height** | 9,710,477 |
| **Size** | 7.6G (zstd) |
| **Created** | 2026-08-02T07:48 UTC |
| **Update frequency** | Every 6 hours |
| **Node stop required** | No |

### Download Latest

```bash
wget -O atomone-snapshot.tar.zst https://snapshots.apollo-validator.eu/atomone/snapshots/latest.tar.zst
```

### Restore from Snapshot

```bash
sudo systemctl stop atomoned
cp $HOME/.atomone/config/priv_validator_state.json $HOME/priv_validator_state.json.bak
rm -rf $HOME/.atomone/data/application.db $HOME/.atomone/data/state.db $HOME/.atomone/data/evidence.db
tar -I zstd -xf atomone-snapshot.tar.zst -C $HOME/.atomone/data/
cat $HOME/priv_validator_state.json.bak > $HOME/.atomone/config/priv_validator_state.json
sudo systemctl start atomoned
```

---

## State Sync

Full guide: [statesync.md](statesync.md) | Web: [snapshots.apollo-validator.eu/atomone/statesync.html](https://snapshots.apollo-validator.eu/atomone/statesync.html)

---

## Peers List

Auto-updated every 6 hours from `/net_info` RPC with TCP verification.

Full list: [peers.md](peers.md)

### Quick Setup

```bash
# Copy the persistent_peers line from peers.md
# Edit $HOME/.atomone/config/config.toml
persistent_peers = "<peers_from_peers.md>"

# Restart node
sudo systemctl restart atomoned
```

---

## Installation Guide

Full guide: [guide.md](guide.md)

### Quick Start

```bash
# Install dependencies
sudo apt update && sudo apt install -y curl git build-essential

# Install Go
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc

# Install atomoned
git clone https://github.com/atomone-hub/atomone.git
cd atomone
git checkout v4.1.0
make install

# Initialize node
atomoned init "your-moniker" --chain-id atomone-1

# Download genesis
curl -s https://raw.githubusercontent.com/atomone-hub/atomone/main/genesis/genesis.json > $HOME/.atomone/config/genesis.json

# Set persistent peers
PEERS="<from peers.md>"
sed -i "s/^persistent_peers = .*/persistent_peers = \"$PEERS\"/" $HOME/.atomone/config/config.toml

# Start node
atomoned start
```

---

## Chain Information

| Parameter | Value |
|---|---|
| Chain ID | atomone-1 |
| Binary | atomoned v4.1.0 |
| Tendermint | 0.38.22 |
| Bond Denom | uatone (1 ATONE = 1,000,000 uatone) |
| Unbonding Time | 21 days |
| Max Validators | 100 |
| Min Gas Prices | 0.001uatone |

---

## Validator

| Field | Value |
|---|---|
| Moniker | Apollo |
| Operator | atonevaloper1hrepv77qv6nh7953jrnqpar4c89yxg5hf8w4qr |
| Commission | 5% |
| Website | [apollo-validator.eu](https://apollo-validator.eu) |

### Delegate

Stake ATONE with Apollo Validator: [Delegate on Mintscan](https://www.mintscan.io/wallet/stake?chain=atomone&src=atonevaloper1hrepv77qv6nh7953jrnqpar4c89yxg5hf8w4qr&type=stake)

---

## Links

- [GitHub](https://github.com/atomone-hub/atomone)
- [Website](https://apollo-validator.eu)
- [RPC](https://rpc.atomone.apollo-validator.eu)
- [API](https://api.atomone.apollo-validator.eu)
- [gRPC](https://grpc.atomone.apollo-validator.eu)
- [Snapshots](https://snapshots.apollo-validator.eu/atomone/snapshot.html)
- [State Sync](https://snapshots.apollo-validator.eu/atomone/statesync.html)
- [Peers](https://snapshots.apollo-validator.eu/atomone/peers.md)
- [Explorer](https://explorer.apollo-validator.eu)

---

## License

MIT
