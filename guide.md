# AtomOne Node Setup Guide

Complete guide for setting up an AtomOne mainnet validator node.

> **Chain:** atomone-1 | **Binary:** atomoned v4.1.0 | **Port prefix:** 61

## Quick Start (Variables)

Set these variables before running commands:

```bash
# Required
export MONIKER="your-node-name"
export WALLET="my-wallet"

# Optional (default values)
export ATOMONE_CHAIN_ID="atomone-1"
export ATOMONE_PORT="61"  # Node will use ports: 61656, 61657, 61317, 61090
```

---

## Prerequisites

- Ubuntu 22.04+ (recommended)
- 8+ CPU cores, 16GB+ RAM, 250GB+ SSD (NVMe recommended)
- Open ports: 26656 (P2P), 26657 (RPC), 1317 (API), 9090 (gRPC)
- Root or sudo access

---

## 1. Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git wget htop tmux build-essential jq make lz4 gcc unzip
```

---

## 2. Install Go

```bash
cd $HOME
VER="1.22.10"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"

# Set PATH
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile

# Verify
go version
```

---

## 3. Install atomoned

```bash
cd $HOME
git clone https://github.com/atomone-hub/atomone.git
cd atomone
git checkout v4.1.0
make install

# Verify
atomoned version
# Should output: v4.1.0
```

---

## 4. Initialize Node

```bash
# Set variables
echo "export WALLET=\"$WALLET\"" >> $HOME/.bash_profile
echo "export MONIKER=\"$MONIKER\"" >> $HOME/.bash_profile
echo "export ATOMONE_CHAIN_ID=\"$ATOMONE_CHAIN_ID\"" >> $HOME/.bash_profile
echo "export ATOMONE_PORT=\"$ATOMONE_PORT\"" >> $HOME/.bash_profile
source $HOME/.bash_profile

# Initialize
atomoned init "$MONIKER" --chain-id "$ATOMONE_CHAIN_ID"

# Configure client
sed -i \
  -e "s/chain-id = .*/chain-id = \"$ATOMONE_CHAIN_ID\"/" \
  -e "s/keyring-backend = .*/keyring-backend = \"os\"/" \
  -e "s/node = .*/node = \"tcp:\/\/localhost:${ATOMONE_PORT}657\"/" \
  $HOME/.atomone/config/client.toml
```

---

## 5. Download Genesis & Addrbook

```bash
# Genesis
wget -O $HOME/.atomone/config/genesis.json \
  https://raw.githubusercontent.com/atomone-hub/atomone/main/genesis/genesis.json

# Addrbook (optional, helps connect to peers faster)
wget -O $HOME/.atomone/config/addrbook.json \
  https://server-3.itrocket.net/mainnet/atomone/addrbook.json
```

---

## 6. Configure Seeds and Peers

```bash
# Seeds
SEEDS="f19d9e0f8d48119aa4cafde65de923ae2c29181a@atomone-mainnet-seed.itrocket.net:61656"

# Peers (use our verified peer list)
PEERS="ed0e36c57122184ab05b6c635b2f2adf592bfa0c@atomone-mainnet-peer.itrocket.net:61657"

# Or get from our service
PEERS=$(curl -s https://snapshots.apollo-validator.eu/atomone/peers.md | grep -oP '[a-f0-9]+@[0-9.]+:26656' | tr '\n' ',' | sed 's/,$//')

# Apply
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" \
       $HOME/.atomone/config/config.toml
```

---

## 7. Configure Custom Ports

```bash
# app.toml
sed -i.bak -e "s%:1317%:${ATOMONE_PORT}317%g;
s%:8080%:${ATOMONE_PORT}080%g;
s%:9090%:${ATOMONE_PORT}090%g;
s%:9091%:${ATOMONE_PORT}091%g;
s%:8545%:${ATOMONE_PORT}545%g;
s%:8546%:${ATOMONE_PORT}546%g;
s%:6065%:${ATOMONE_PORT}065%g" $HOME/.atomone/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${ATOMONE_PORT}658%g;
s%:26657%:${ATOMONE_PORT}657%g;
s%:6060%:${ATOMONE_PORT}060%g;
s%:26656%:${ATOMONE_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${ATOMONE_PORT}656\"%;
s%:26660%:${ATOMONE_PORT}660%g" $HOME/.atomone/config/config.toml
```

---

## 8. Configure Pruning

```bash
# Custom pruning (recommended for validators)
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.atomone/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.atomone/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.atomone/config/app.toml
```

**Pruning options:**
| Option | Keep Recent | Interval | Disk Usage |
|--------|-------------|----------|------------|
| `default` | 100 | 0 | ~100GB |
| `nothing` | 0 | 0 | ~200GB+ |
| `everything` | 86400 | 10 | ~300GB+ |
| `custom` | 100 | 19 | ~50GB |

---

## 9. Configure Gas, Prometheus, Indexing

```bash
# Minimum gas prices
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.025uatone,0.225uphoton"|g' \
  $HOME/.atomone/config/app.toml

# Enable Prometheus (for monitoring)
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.atomone/config/config.toml

# Disable indexing (optional, saves resources)
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.atomone/config/config.toml
```

---

## 10. Create Systemd Service

```bash
sudo tee /etc/systemd/system/atomoned.service > /dev/null <<EOF
[Unit]
Description=AtomOne node
After=network-online.target

[Service]
User=$USER
WorkingDirectory=$HOME/.atomone
ExecStart=$(which atomoned) start --home $HOME/.atomone
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable atomoned
```

---

## 11. Reset and Start

```bash
# Reset data (if needed)
atomoned tendermint unsafe-reset-all --home $HOME/.atomone

# Download snapshot (optional, faster than syncing from scratch)
curl -s https://snapshots.apollo-validator.eu/api/atomone/snapshots/latest | jq
# Download and restore from snapshot...

# Start node
sudo systemctl restart atomoned

# Check logs
sudo journalctl -u atomoned -fo cat
```

---

## 12. Sync Status Checker

```bash
#!/bin/bash
# Save as sync-checker.sh and run: bash sync-checker.sh

rpc_port=$(grep -m 1 -oP '^laddr = "\K[^"]+' "$HOME/.atomone/config/config.toml" | cut -d ':' -f 3)
while true; do
  local_height=$(curl -s localhost:$rpc_port/status | jq -r '.result.sync_info.latest_block_height')
  network_height=$(curl -s https://rpc.atomone.apollo-validator.eu/status | jq -r '.result.sync_info.latest_block_height')

  if ! [[ "$local_height" =~ ^[0-9]+$ ]] || ! [[ "$network_height" =~ ^[0-9]+$ ]]; then
    echo -e "\033[1;31mError: Invalid block height data. Retrying...\033[0m"
    sleep 5
    continue
  fi

  blocks_left=$((network_height - local_height))
  echo -e "\033[1;33mNode Height:\033[1;34m $local_height\033[0m \033[1;33m| Network Height:\033[1;36m $network_height\033[0m \033[1;33m| Blocks Left:\033[1;31m $blocks_left\033[0m"
  sleep 5
done
```

---

## Cheat Sheet

### Service Management

```bash
# Status
sudo systemctl status atomoned

# Start/Stop/Restart
sudo systemctl start atomoned
sudo systemctl stop atomoned
sudo systemctl restart atomoned

# Logs
sudo journalctl -u atomoned -fo cat
sudo journalctl -u atomoned -f  # follow

# Enable/Disable on boot
sudo systemctl enable atomoned
sudo systemctl disable atomoned

# Reload systemd
sudo systemctl daemon-reload
```

### Node Info

```bash
# Node status
atomoned status 2>&1 | jq

# Node info
atomoned status 2>&1 | jq .NodeInfo

# Sync info
atomoned status 2>&1 | jq .SyncInfo

# Validator info
atomoned status 2>&1 | jq .ValidatorInfo

# Your peer ID
echo $(atomoned tendermint show-node-id)'@'$(wget -qO- eth0.me)':'$(grep -m 1 'laddr' $HOME/.atomone/config/config.toml | cut -d ':' -f 3 | tr -d '"')
```

### Key Management

```bash
# Create wallet
atomoned keys add $WALLET

# Restore wallet
atomoned keys add $WALLET --recover

# List wallets
atomoned keys list

# Delete wallet
atomoned keys delete $WALLET

# Check balance
atomoned query bank balances $WALLET_ADDRESS

# Export key
atomoned keys export $WALLET

# Import key
atomoned keys import $WALLET wallet.backup
```

### Validator Operations

```bash
# Get valoper address
VALOPER_ADDRESS=$(atomoned keys show $WALLET --bech val -a)
echo $VALOPER_ADDRESS

# Create validator
atomoned tx staking create-validator \
  --amount 1000000uatone \
  --from $WALLET \
  --commission-rate 0.1 \
  --commission-max-rate 0.2 \
  --commission-max-change-rate 0.01 \
  --min-self-delegation 1 \
  --pubkey $(atomoned tendermint show-validator) \
  --moniker "$MONIKER" \
  --identity "" \
  --website "" \
  --details "Apollo Validator" \
  --chain-id $ATOMONE_CHAIN_ID \
  --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y

# Edit validator
atomoned tx staking edit-validator \
  --commission-rate 0.1 \
  --new-moniker "$MONIKER" \
  --identity "" \
  --details "Apollo Validator" \
  --from $WALLET \
  --chain-id $ATOMONE_CHAIN_ID \
  --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y

# Validator details
atomoned q staking validator $VALOPER_ADDRESS

# Jailing info
atomoned q slashing signing-info $(atomoned tendermint show-validator)

# Unjail
atomoned tx slashing unjail --from $WALLET --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y

# Active validators list
atomoned q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

### Staking Operations

```bash
# Delegate
atomoned tx staking delegate $VALOPER_ADDRESS 1000000uatone \
  --from $WALLET --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y

# Redelegate
atomoned tx staking redelegate $VALOPER_ADDRESS <NEW_VALOPER> 1000000uatone \
  --from $WALLET --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y

# Unbond
atomoned tx staking unbond $VALOPER_ADDRESS 1000000uatone \
  --from $WALLET --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y
```

### Withdraw Rewards

```bash
# Withdraw all rewards
atomoned tx distribution withdraw-all-rewards \
  --from $WALLET --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y

# Withdraw rewards + commission
atomoned tx distribution withdraw-rewards $VALOPER_ADDRESS \
  --from $WALLET --commission --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y
```

### Governance

```bash
# List proposals
atomoned query gov proposals

# View proposal
atomoned query gov proposal <PROPOSAL_ID>

# Vote
atomoned tx gov vote <PROPOSAL_ID> yes --from $WALLET \
  --chain-id $ATOMONE_CHAIN_ID --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y
```

### Token Transfer

```bash
atomoned tx bank send $WALLET_ADDRESS <TO_ADDRESS> 1000000uatone \
  --gas auto --gas-adjustment 1.5 --fees 60000uphoton -y
```

---

## Upgrade Guide

When a new version is released:

```bash
# Stop node
sudo systemctl stop atomoned

# Download new binary
cd $HOME
rm -rf bin && mkdir bin && cd bin
wget -O atomoned https://github.com/atomone-hub/atomone/releases/download/vX.X.X/atomoned-vX.X.X-linux-amd64
chmod +x $HOME/bin/atomoned

# Replace old binary
sudo mv $HOME/bin/atomoned $(which atomoned)

# Start node
sudo systemctl start atomoned

# Check logs
sudo journalctl -u atomoned -fo cat
```

---

## Security

### SSH Key Authentication

```bash
# On your LOCAL machine
ssh-keygen -t ed25519

# Copy to server
ssh-copy-id root@YOUR_SERVER_IP

# On SERVER - disable password auth
sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### Firewall

```bash
# Allow only necessary ports
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh/tcp
sudo ufw allow ${ATOMONE_PORT}656/tcp  # P2P
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# Check status
sudo ufw status
```

---

## Delete Node

```bash
# Stop and disable service
sudo systemctl stop atomoned
sudo systemctl disable atomoned

# Remove service file
sudo rm /etc/systemd/system/atomoned.service
sudo systemctl daemon-reload

# Remove binary
sudo rm $(which atomoned)

# Remove data
sudo rm -rf $HOME/.atomone

# Remove environment variables
sed -i "/ATOMONE_/d" $HOME/.bash_profile
sed -i "/WALLET=/d" $HOME/.bash_profile
sed -i "/MONIKER=/d" $HOME/.bash_profile
```

---

## Troubleshooting

### Node won't start

```bash
# Check logs
sudo journalctl -u atomoned -n 100 --no-pager

# Common issues:
# 1. Port already in use → change ATOMONE_PORT
# 2. Genesis mismatch → re-download genesis.json
# 3. Database locked → stop other instances
```

### Sync is slow

```bash
# Use snapshot instead
curl -s https://snapshots.apollo-validator.eu/api/atomone/snapshots/latest | jq

# Or use state sync (see statesync.md)
```

### High memory usage

```bash
# Reduce cache in config.toml
sed -i 's/cache_size = 8192/cache_size = 2048/' $HOME/.atomone/config/config.toml
sudo systemctl restart atomoned
```

---

## Links

- [GitHub](https://github.com/atomone-hub/atomone)
- [RPC](https://rpc.atomone.apollo-validator.eu)
- [API](https://api.atomone.apollo-validator.eu)
- [Snapshots](https://snapshots.apollo-validator.eu/atomone/)
- [Peers](https://snapshots.apollo-validator.eu/atomone/peers.md)
- [State Sync](https://snapshots.apollo-validator.eu/atomone/statesync.html)
- [Explorer](https://explorer.apollo-validator.eu)
- [Discord](https://discord.gg/atomone)
