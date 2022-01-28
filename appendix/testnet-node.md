---
label: Testnet Node
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -8
---

# Testnet Node

Go to https://github.com/input-output-hk/cardano-node

Under Releases, click on Cardano Node 1.33.0

Under Technical Specification > Downloads, click on Hydra binaries

Click on cardano-node-linux

Click on cardano-node-1.33.0-linux.tar.gz

cd Downloads

tar -xf cardano-node-1.33.0-linux.tar.gz

mkdir -p .local/bin/cardano-node

mv ./* ~/.local/bin/cardano-node

vim .bashrc

export PATH="$HOME/.local/bin/cardano-node:$PATH"
export CARDANO_NODE_SOCKET_PATH="$HOME/plutus-pioneer-program/code/week03/testnet/node.socket"

source .bashrc

cd ~/plutus-pioneer-program/code/week03/testnet

./start-node-testnet.sh

cardano-cli query tip --testnet-magic 1097911063

Look for syncProgress. Wait until it reaches 100%. Will be a few hours.
