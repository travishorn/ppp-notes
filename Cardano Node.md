# Cardano Node

A [Cardano](./Cardano.md) node is the core component that underpins the network. The blockchain network is a collection of these interconnected nodes.

## Installing 
To run a Cardano node yourself, follow these instructions. They are written for Linux.

Install Nix

```bash
curl -L https://nixos.org/nix/install | sh
```

Follow the instructions on screen.

Create a directory to store nix configuration

```bash
sudo mkdir -p /etc/nix
```

Create a file in that directory called `/etc/nix/nix.conf` with the following content. This will set up the binary cache.

```
substituters = https://cache.nixos.org https://hydra.iohk.io
trusted-public-keys = iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
```

Clone the node source

```bash
git clone https://github.com/input-output-hk/cardano-node
```

Change into the source directory

```bash
cd cardano-node
```

Build the node

```bash
nix-build -A scripts.mainnet.node -o mainnet-node-local
```

## Running
Start the node

```bash
./mainnet-node-local/bin/cardano-node-mainnet
```

## Updating
Pull the latest changes to the node source

```bash
git pull
```

Rebuild with nix

```bash
nix-build -A scripts.mainnet.node -o mainnet-node-local
```
