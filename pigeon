#!/bin/bash
sudo apt update
sudo apt install -y curl git jq lz4 build-essential unzip

bash <(curl -s "https://raw.githubusercontent.com/nodejumper-org/cosmos-scripts/master/utils/go_install.sh")
source .bash_profile

#!/bin/bash

NODE_MONIKER="ERN"

cd || return
curl -L https://github.com/CosmWasm/wasmvm/releases/download/v1.4.0/libwasmvm.x86_64.so > libwasmvm.x86_64.so
sudo mv -f libwasmvm.x86_64.so /usr/lib/libwasmvm.x86_64.so

cd || return
rm -rf paloma
git clone https://github.com/palomachain/paloma.git
cd paloma || return
git checkout v1.10.2
make install
sudo mv -f $HOME/go/bin/palomad /usr/local/bin/palomad
palomad version 
curl -L https://github.com/palomachain/pigeon/releases/download/v1.10.2/pigeon_Linux_x86_64.tar.gz > pigeon.tar.gz
tar -xvzf pigeon.tar.gz
rm -rf pigeon.tar.gz
sudo mv -f pigeon /usr/local/bin/pigeon
pigeon version # 

palomad init "$NODE_MONIKER" --chain-id messenger
palomad config chain-id messenger

curl -s https://raw.githubusercontent.com/palomachain/mainnet/master/messenger/genesis.json > $HOME/.paloma/config/genesis.json

SEEDS=""
PEERS="ab6875bd52d6493f39612eb5dff57ced1e3a5ad6@95.217.229.18:10656,9581fadb9a32f2af89d575bb0f2661b9bb216d41@46.4.23.108:26656,4e35ce47a8c2654a0cd371a2d1485e157b6ce311@93.190.141.218:26656,874ccf9df2e4c678a18a1fb45a1d3bb703f87fa0@65.109.172.249:26656,6ee0ed8ddb1eaaf095686962d71fddb1383b5199@65.21.138.123:26656"
sed -i 's|^seeds *=.*|seeds = "'$SEEDS'"|; s|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.paloma/config/config.toml

sed -i 's|^pruning *=.*|pruning = "custom"|g' $HOME/.paloma/config/app.toml
sed -i 's|^pruning-keep-recent  *=.*|pruning-keep-recent = "100"|g' $HOME/.paloma/config/app.toml
sed -i 's|^pruning-interval *=.*|pruning-interval = "10"|g' $HOME/.paloma/config/app.toml
sed -i 's|^snapshot-interval *=.*|snapshot-interval = 0|g' $HOME/.paloma/config/app.toml

sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.0001ugrain"|g' $HOME/.paloma/config/app.toml
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.paloma/config/config.toml

echo "export PIGEON_HEALTHCHECK_PORT=5757" >> $HOME/.bash_profile
source .bash_profile

mkdir -p $HOME/.pigeon

sudo tee $HOME/.pigeon/config.yaml > /dev/null << EOF
loop-timeout: 5s
health-check-port: 5757

paloma:
  chain-id: messenger
  call-timeout: 20s
  keyring-dir: ~/.paloma
  keyring-pass-env-name: PALOMA_PASSWORD
  keyring-type: os
  signing-key: wallet
  base-rpc-url: http://localhost:26657
  gas-adjustment: 3.0
  gas-prices: 0.01ugrain
  account-prefix: paloma

evm:
  eth-main:
    chain-id: 1
    base-rpc-url: ${ETH_RPC_URL}
    keyring-pass-env-name: "ETH_PASSWORD"
    signing-key: ${ETH_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/eth-main
    gas-adjustment: 1.9
    tx-type: 2
  bnb-main:
    chain-id: 56
    base-rpc-url: ${BNB_RPC_URL}
    keyring-pass-env-name: "BNB_PASSWORD"
    signing-key: ${BNB_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/bnb-main
    gas-adjustment: 1
    tx-type: 0
  matic-main:
    chain-id: 137
    base-rpc-url: ${MATIC_RPC_URL}
    keyring-pass-env-name: "MATIC_PASSWORD"
    signing-key: ${MATIC_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/matic-main
    gas-adjustment: 2
    tx-type: 2
  op-main:
    chain-id: 10
    base-rpc-url: ${OP_RPC_URL}
    keyring-pass-env-name: "OP_PASSWORD"
    signing-key: ${OP_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/op-main
    gas-adjustment: 2
    tx-type: 2
  kava-main:
    chain-id: 2222
    base-rpc-url: ${KAVA_RPC_URL}
    keyring-pass-env-name: "KAVA_PASSWORD"
    signing-key: ${KAVA_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/kava-main
    gas-adjustment: 2
    tx-type: 2
  base-main:
    chain-id: 8453
    base-rpc-url: ${BASE_RPC_URL}
    keyring-pass-env-name: "BASE_PASSWORD"
    signing-key: ${BASE_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/base-main
    gas-adjustment: 1
    tx-type: 2
  gnosis-main:
    chain-id: 100
    base-rpc-url: ${GNOSIS_RPC_URL}
    keyring-pass-env-name: "GNOSIS_PASSWORD"
    signing-key: ${GNOSIS_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/gnosis-main
    gas-adjustment: 1.2
    tx-type: 2
  arbitrum-main:
    chain-id: 42161
    base-rpc-url: ${ARB_RPC_URL}
    keyring-pass-env-name: "ARB_PASSWORD"
    signing-key: ${ARB_SIGNING_KEY}
    keyring-dir: ~/.pigeon/keys/evm/arbitrum-main
    gas-adjustment: 2
    tx-type: 2
EOF

