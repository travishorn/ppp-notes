---
label: Get Playground Wallet Addresses
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -4
---

# Get Playground Wallet Addresses

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

| Wallet | Hash                                                     |
|:-------|:---------------------------------------------------------|
| 1      | a2c20c77887ace1cd986193e4e75babd8993cfd56995cd5cfce609c2 |
| 2      | 80a4f45b56b88d1139da23bc4c3c75ec6d32943c087f250b86193ca7 |
| 3      | 2e0ad60c3207248cecd47dbde3d752e0aad141d6b8f81ac2c6eca27c |
| 4      | 557d23c0a533b4d295ac2dc14b783a7efc293bc23ede88a6fefd203d |
| 5      | bf342ddd3b1a6191d4ce936c92d29834d6879edf2849eaea84c827f8 |
