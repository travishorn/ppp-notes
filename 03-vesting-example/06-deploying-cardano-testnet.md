---
label: Deploying to the Cardano Testnet
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -6
---

# Vesting Example

## Deploying to the Cardano Testnet

Lecture 3, Part 6

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=ABtffZPoUqU&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=6)

[Install and run a testnet node.](../appendix/run-testnet-node.md)

### Generating Wallets

To test out a contract, you'll want a couple wallets to interact with.

Change into the testnet directory under week 3

```bash
cd plutus-pioneer-program/code/week03/testnet
```

Generate a key for "wallet 1"

```bash
cardano-cli address key-gen \
  --verification-key-file 01.vkey \
  --signing-key-file 01.skey
```

Generate a key for "wallet 2"

```bash
cardano-cli address key-gen \
  --verification-key-file 02.vkey \
  --signing-key-file 02.skey
```

From these two commands, four new keys are created:

- 01.vkey - wallet 1's verification key
- 01.skey - wallet 1's signing key
- 02.vkey - wallet 2's verification key
- 02.skey - wallet 2's signing key

You can view any of these keys

```bash
cat 01.vkey

{
  "type": "PaymentVerificationKeyShelley_ed255519",
  "description": "Payment Verification Key",
  "cborHex": "582..."
}
```

The next commands require the testnet magic number.

View it in the genesis configuration for the started node

```bash
cat testnet-shelley-genesis.json | grep Magic
  "networkMagic": 1097911063
```

Build a testnet address based on wallet 1's verification key

```bash
cardano-cli address build \
  --payment-verification-key-file 01.vkey \
  --testnet-magic 1097911063 \
  --out-file 01.addr
```

And again, but for wallet 2

```bash
cardano-cli address build \
  --payment-verification-key-file 02.vkey \
  --testnet-magic 1097911063 \
  --out-file 02.addr
```

Those two commands generated two new files:

- 01.addr - wallet 1's address
- 02.addr - wallet 2's address

You can view them if you like

```bash
cat 01.addr
addr_test1vrlnf...
```

### Funding with Test ADA

You'll need some ADA to play with. On the testnet, this is called tADA. You can
request tADA for free using the testnet faucet.

First, copy the address of wallet 1 to your clipboard

Then go to [https://testnets.cardano.org/en/testnets/cardano/tools/faucet
:icon-link-external:](https://testnets.cardano.org/en/testnets/cardano/tools/faucet)

Paste in the address into the **Address** box

Check the **I'm not a robot** checkbox to complete the CAPTCHA.

Click on **Request funds**

A new transaction is automatically generated with 1000 tADA to your address as
an output.

You can query for all UTxOs containing wallet 1's address

```bash
cardano-cli query utxo \
  --address $(cat 01.addr)
  --testnet-magic 1097911063

TxHash  TxIx  Amount
--------------------------------------------------
1e7...     0  1000000000 lovelace + TxOutDatumNone
```

Since we just created this wallet and used the faucet to receive 1000 tADA,
there's a single UTxO that contains that amount.

### Transferring from One Address to Another

The faucet only lets you use it once every 24 hours. To fund wallet 2, we can
transfer tADA from wallet 1. This type of transaction requires 3 `cardano-cli`
actions:

- Build the transaction
- Sign the transaction
- Submit the transaction

Week 3's testnet directory contains a shell script to do these three things. So
you don't have to type out the whole thing. You will, however, have to modify
this file slightly.

Edit `send.sh`

```bash
vim send.sh
```

On line 5, you need to paste in the TxHash of the UTxO that contains the 1000
tADA. Note that `--tx-in` takes the TxHash, followed by `#`, followed by the
TxIx.

```bash
cardano-cli transaction build \
  --alonzo-era \
  --testnet-magic 1097911063 \
  --change-address $(cat 01.addr) \
  --tx-in dfc1a522cd34fe723a0e89f68ed43a520fd218e20d8e5705b120d2cedc7f45ad#0 \
  --tx-out "$(cat 02.addr) 10000000 lovelace" \
  --out-file tx.body

cardano-cli transaction sign \
  --tx-body-file tx.body \
  --signing-key-file 01.skey \
  --testnet-magic 1097911063 \
  --out-file tx.signed

cardano-cli transaction submit \
  --testnet-magic 1097911063 \
  --tx-file tx.signed
```

Wait a little bit for the transaction to be processed. On average, it takes
about 20 seconds.

Query for relevant UTxOs again

```bash
cardano-cli query utxo \
  --address $(cat 01.addr)
  --testnet-magic 1097911063

TxHash  TxIx  Amount
--------------------------------------------------
9e1...     0  989834279 lovelace + TxOutDatumNone
```

Notice the original 1000 tADA UTxO is gone. It's no longer an Unspent
Transaction Output. It has been spent. In its place is a new UTxO with a lower
amount as expected. The lower amount is 1000 minus the 10 we sent to wallet 2
minus fees.

Query for wallet 2's UTxOs now

```bash
cardano-cli query utxo \
  --address $(cat 02.addr)
  --testnet-magic 1097911063

TxHash  TxIx  Amount
------------------------------------------------
9e1...     0  10000000 lovelace + TxOutDatumNone
```

Note that it contains 10 tADA as expected.

### Using Plutus in the Cardano CLI

These notes are incomplete. They stop at timestamp 16:22 of lecture 3, part 6.
