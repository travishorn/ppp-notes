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

Before using Plutus with the CLI, we have to write various serialized Plutus
types to disk. `Deploy.hs` (in week 3's code directory) does this.

[Source code :icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/037142877d7275d47314af21413d803dc58a1da3/code/week03/src/Week03/Deploy.hs)

[My commented code :icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week03/src/Week03/Deploy.hs)

The `Cardano.API` module is what `cardano-cli` uses under the hood. It contains
all the functionality to interact with the network.

When writing a deployment script like this, you may need a wallet's public key
hash. You can write hashes to files for each wallet like so:

```bash
cardano-cli address key-hash \
  --payment-verification-key-file 01.vkey
  --out-file 01.pkh

cardano-cli address key-hash \
  --payment-verification-key-file 02.vkey
  --out-file 02.pkh
```

The *.pkh files each contain a hash

```bash
cat 02.pkh
d4f...
```

You may also need a time in POSIX. You can use an online converter like
[EpochConverter](https://epochconverter.com). Enter a human-readable date and
convert it to a timestamp in milliseconds.

In the case of week 3's `Deploy.hs`, make sure to scroll to the bottom and...

- set the beneficiary to wallet 2's public key hash
- set the deadline to a POSIX time in the future. Maybe 30-60 minutes out if you
  want to be able to test a grab failing before the deadline and then succeeding
  after

### Build the Script

First, you need write the script file to disk.

Make sure you're in a `plutus-apps` Nix shell, then start the REPL in week 3's
code directory.

```bash
cd plutus-apps
nix-shell
cd ../plutus-pioneer-program/code/week03
cabal repl
```

Week03.Deploy might already be loaded. If not, load it

```haskell
:l src/week03/Deploy.hs
```

Write the validator to disk

```haskell
writeVestingValidator
```

That function writes a file called `vesting.plutus`

Use that file to build the script

```bash
cardano-cli address build-script \
  --script-file vesting.plutus \
  --testnet-magic 1097911063 \
  --out-file vesting.addr
```

You can view the script's address

```
cat vesting.addr
addr_test1wq6...
```

### Use the `give` endpoint

To use any script endpoint, you'll need to again build, sign, and submit a
transaction.

```bash
cardano-cli transaction build \
  --alonzo-era \
  --testnet-magic 1097911063 \
  --change-address $(cat 01.addr) \
  --tx-in abae0d0e19f75938537dc5e33252567ae3b1df1f35aafedd1402b6b9ccb7685a#0 \
  --tx-out "$(cat vesting.addr) 200000000 lovelace" \
  --tx-out-datum-hash-file unit.json \
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

These three commands are already written for you in `testnet/give.sh`. You will
need to edit the `--tx-in` flag in that file. It should be the TxHash and TxIx
of the UTxO where wallet 1's tADA is sitting. Again, you can find it with
`cardano-cli`.

```bash
cardano-cli query utxo \
  --address $(cat 01.addr)
  --testnet-magic 1097911063
```

Run the script to build, sign, and submit the transaction

```bash
./give.sh
```

Check the script address to see the transaction

```bash
cardano-cli query utxo \
  --address $(cat vesting.addr)
  --testnet-magic 1097911063

TxHash  TxIx  Amount
--------------------------------------------------------------------------------
e10...     0  200000000 lovelace + TxOutDatumHash ScriptDataInAlonzoEra "923..."
```

The script address does indeed have the 200 ADA we gave from wallet 1

### Use the `grab` endpoint

Again, you'll need to build, sign, and submit a transaction.

```bash
cardano-cli transaction build \
  --alonzo-era \
  --testnet-magic 1097911063 \
  --change-address $(cat 02.addr) \
  --tx-in 18cbe6cadecd3f89b60e08e68e5e6c7d72d730aaa1ad21431590f7e6643438ef#1 \
  --tx-in-script-file vesting.plutus \
  --tx-in-datum-file unit.json \
  --tx-in-redeemer-file unit.json \
  --tx-in-collateral 18e93407ea137b6be63039fd3c564d4c5233e7eb7ce4ee845bc7df12c80e4df7#1 \
  --required-signer-hash c2ff616e11299d9094ce0a7eb5b7284b705147a822f4ffbd471f971a \
  --invalid-before 48866954 \
  --protocol-params-file protocol.json \
  --out-file tx.body

cardano-cli transaction sign \
  --tx-body-file tx.body \
  --signing-key-file 02.skey \
  --testnet-magic 1097911063 \
  --out-file tx.signed

cardano-cli transaction submit \
  --testnet-magic 1097911063 \
  --tx-file tx.signed
```

`testnet/grab.sh` has these three commands in it already.

You will need to modify `--tx-in` to be the TxHash and TxIx of the UTxO with the
200 tADA at the script address. That can be found with the last command you ran.

You will need to modify `--tx-in-collateral` to be a UTxO you have access to.
Since wallet 2 is submitting this transaction, it should be a TxHash and TxIx of
a UTxO they have access to. Find one using the CLI again.

```bash
cardano-cli query utxo \
  --address $(cat 02.addr)
  --testnet-magic 1097911063
```

You will need to modify `--required-signer-hash` to be the public key hash of
wallet 2 since they are the beneficiary and the one submitting the grab
transaction. This should match the beneficiary listed near the end of
`Deploy.hs`. You can also see it again with the CLI.

```bash
cardano-cli address key-hash --payment-verification-key-file 02.vkey
```

Finally, you will need to modify `--invalid-before` to be the current slot. You
can get the current slot from the CLI

```bash
cardano-cli query tip --testnet-magic 1097911063
```

Look for "slot". Copy and paste the number after the `--invalid-before` flag.

With all of that done, you can submit the transaction

```bash
./grab.sh
```

If the current time is before the deadline you set in `Deploy.hs` then you'll
see an error

```
Script debugging logs: deadline not reached
```

If you try to run it again later, after the deadline as been reached, you'll see
a success message

```
Transaction successfully submitted.
```

You can now query wallet 2 and see the 200 tADA (minus fees) as one of the
matching UTxOs.

```bash
cardano-cli query utxo \
  --address $(cat 02.addr)
  --testnet-magic 1097911063

TxHash  TxIx  Amount
------------------------------------------------
578...     0  199634559 lovelace + TxOutDatumNone
9e1...     1  10000000 lovelace + TxOutDatumNone
```
