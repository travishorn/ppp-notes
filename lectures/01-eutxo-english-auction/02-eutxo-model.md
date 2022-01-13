# EUTxO and English Auction

Lecture 1, Part 2

## The EUTxO Model

Extended Unspent Transaction Output

Unspent transaction outputs are outputs from a previous transaction on the
blockchain that have not been spent.

### Example 1

Alice has 100 ADA and Bob has 50 ADA. Alice wants to sent Bob 10 ADA. Alice
creates a transaction.

Transactions can have any number of inputs and any number of outputs. Must use a
complete EUTxO as an input.

Alice currently has one output containing 100 ADA. She creates a new transaction
with that 100 ADA output as the single input. The new transaction has two
outputs: 10 ADA to Bob and 90 ADA back toAlice. Even though she consumed her
entire 100 ADA, the transaction that output 90 ADA back to her (her "change").

### Example 2

Alice has one output with 90 ADA and Bob has two outputs with 50 and 10 ADA
respectively.

Alice and Bob together want to pay Charlie 110 ADA (55 ADA each).

They create a transaction together with three inputs:

- Alice 90 ADA
- Bob 50 ADA
- Bob 10 ADA

The transaction has three outputs:

- Alice 35 ADA
- Bob 5 ADA
- Charlie 110 ADA

Alice and Bob both sign the transaction.

---

Outputs don't have to be public keys associated with people. They can also be
smart contracts that contain arbritrary logic.

These smart contracts are build with the Plutus platform.

In a regular transaction "Alice" is the public key and the transaction is signed
with her digital signature. In a smart contract, the public key (aka Alice) is
replace by a Script and the digital signature is replaced with a Redeemer.

The script can see all inputs and outputs that are part of the transaction.

An arbritrary piece of data can be added to the script called Datum. It is
associated with a EUTxO in addition to the value. For example, the EUTxO can
contain 100 ADA *and* a Datum.

Given the context of the redeemer, the datum, the inputs, and the outputs, a
smart contract can determine whether this transaction is valid or not.

[NEXT: Part 3: Building the Example Code](./03-building-example-code.md)
