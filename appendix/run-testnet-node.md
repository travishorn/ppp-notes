---
label: Run a Testnet Node
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -8
---

# Run a Testnet Node

The easiest way to interact with the Cardano network is by running your own
node. Cardano provides two networks: The mainnet and the testnet. The mainnet is
what you normally think about when you think about Cardano. The testnet also a
real Cardano network, but ADA (called tADA on the testnet) has no real value as
anyone can request it for free.

These instructions are for Linux, but you may be able to modify them according
to your operating system.

## Installation

Installation is a one-time process you must complete before starting a node. It
will take about 10GB of storage space and synchronization will take a few hours
once you get it going.

### Download the Latest Binary

Go to https://github.com/input-output-hk/cardano-node

Under **Releases**, click on **Cardano Node 1.33.0**. The version might be
different when you're reading this. Just use the latest version listed under
**Releases**.

Under **Technical Specification** > **Downloads**, click on **Hydra binaries**.

Click on **cardano-node-linux**.

Click on **cardano-node-1.33.0-linux.tar.gz** to download the zipped files.

Change into the directory where the zipped files were downloaded

```bash
cd Downloads
```

Unzip the files

```bash
tar -xf cardano-node-1.33.0-linux.tar.gz
```

Make a new directory for the Cardano binary files. This could be anywhere you
want.

```bash
mkdir -p .local/bin/cardano-node
```

Move the unzipped files to that directory

```bash
mv ./* ~/.local/bin/cardano-node
```

### Set Environment Variables

Make sure the node directory is always in your path by adding an export in your
`.bashrc`.

```bash
vim ~/.bashrc
```

```bash
export PATH="$HOME/.local/bin/cardano-node:$PATH"
```

While you're editing `.bashrc`, you should also add the following environment
variable. This variable will be used by `cardano-cli`. It points to the socket
your node will be running at. You may need to modify the path depending on your
directory structure.

```bash
export CARDANO_NODE_SOCKET_PATH="$HOME/plutus-pioneer-program/code/week03/testnet/node.socket"
```

Source `.bashrc` to make sure the `PATH` is updated and
`CARDANO_NODE_SOCKET_PATH` is set.

```bash
source ~/.bashrc
```

## Starting the Node

With the node installed, just do the following steps any time you want to start
it.

Change into the `testnet` directory for week 3's code.

```bash
cd ~/plutus-pioneer-program/code/week03/testnet
```

Week 3's code contains configuration files and a helpful shell script for
starting a testnet node.

Start the node using the shell script

```bash
./start-node-testnet.sh
```

The script contains just one command. Alternatively, you could run this command
manually yourself.

```bash
cardano-node run \
  --topology testnet-topology.json \
  --database-path db \
  --socket-path node.socket \
  --host-addr 127.0.0.1 \
  --port 3001 \
  --config testnet-config.json
```

### View Sync Progress

The first time you start the node, it will immediately start syncing with the
testnet blockchain. Before you can really interact with the testnet, you'll have
to wait until your node is fully synchronized. This will take a few hours.

You can see the sync progress at any time by using `cardano-cli` in another
terminal.

The command to see sync progress requires a `--testnet-magic` flag with a
specific number. You can find this number in the genesis configuration for the
node you started.

```bash
cat testnet-shelley-genesis.json | grep Magic
  "networkMagic": 1097911063
```

Query to see sync progress

```bash
cardano-cli query tip --testnet-magic 1097911063
```

Each subsequent time you run the node, it will first push ledger state. This
will take a few minutes. Then it will synchronize with the blockchain again. It
should go quickly if you've ran the node recently.
