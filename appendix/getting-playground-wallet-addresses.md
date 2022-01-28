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

Get the address of various wallets by number

```haskell
knownWallet 1
Wallet 872cb83b5ee40eb23bfdab1772660c822a48d491

knowWallet 2
Wallet 7ce812d7a4770bbf58004067665c3a48f28ddd58
```

Get the hash of a wallet

```haskell
mockWalletPaymentPubKeyHash $ knownWallet 1
a2c20c77887ace1cd986193e4e75babd8993cfd56995cd5cfce609c2
```

The first few playground wallets as of Plutus Pioneer Program Cohort 3 Week 3

| Wallet | Address                                  |
|:-------|:-----------------------------------------|
| 1      | 872cb83b5ee40eb23bfdab1772660c822a48d491 |
| 2      | 7ce812d7a4770bbf58004067665c3a48f28ddd58 |
| 3      | c30efb78b4e272685c1f9f0c93787fd4b6743154 |
| 4      | 5f5a4f5f465580a5500b9a9cede7f4e014a37ea8 |
| 5      | d3eddd0d37989746b029a0e050386bc425363901 |

| Wallet | Hash                                                             |
|:-------|:-----------------------------------------------------------------|
| 1      | 8d9de88fbf445b7f6c3875a14daba94caee2ffcbc9ac211c95aba0a2f5711853 |
| 2      | 98c77c40ccc536e0d433874dae97d4a0787b10b3bca0dc2e1bdc7be0a544f0ac |
| 3      | 4cdc632449cde98d811f78ad2e2d15a278731bc5e3b8821739a47534c324fe20 |
| 4      | c0a4b02f44c212ba6c1197df5a5cf8bd1a3dceef9c42e4a17c0d48f8ae468cdb |
| 5      | 993684ab55aa4f999edb02c9cd1e730e91d785eccc0922db5d2aa805fde40f68 |

