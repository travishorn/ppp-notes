---
label: Getting Playground Wallet Addresses
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Getting Playground Wallet Addresses

In order to simulate some contracts in the Playground you'll need to know the
public key hash of the wallets. You can use the REPL inside a particular week's
project to get simulated wallet key hashes.

Change into the `plutus-apps` directory

```bash
cd plutus-apps
```

Start a Nix shell

```bash
nix-shell
```

Change into the week's code directory

```bash
cd ../plutus-pioneer-program/code/week03
```

Start the REPL

```bash
cabal repl
```

Import the `Wallet.Emulator` module

```haskell
import Wallet.Emulator
```

Get the address of wallet 1

```haskell
knownWallet 1
```
