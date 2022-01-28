---
label: A Vesting Example
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -4
---

# Vesting Example

## A Vesting Example

Lecture 3, Part 4

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=ae7U_yKIQ0Y&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=4)

Imagine you want to gift some ADA to a child but don't want them to be able to
access it until a certain age. It's easy to lock ADA into a contract and only be
able to pull it out after a certain time.

### Vesting.hs

[Source Code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/97a9fe6f1ead6e7558c3c64dcc1d12c1d167b051/code/week03/src/Week03/Vesting.hs)

Most of my notes for this part are done as comments directly in source code.

[My commented code
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week03/src/Week03/Vesting.hs)

`Vesting.hs` is a modified version of the `IsData.hs` script from last week.
That's the one we learned custom types on.

In order to simulate this contract in the Playground you'll need to know the
public key hash of the beneficiary wallet(s). [Use the REPL inside a particular
week's project to get these simulated wallet key
hashes.](../appendix/getting-playground-wallet-addresses.md)

If you set up the Playground so that there are 3 wallets with 30 ADA each, then
start adding actions:

- Wallet 1: give to gpBeneficiary 80a... (wallet 2) with gpDeadline of 159... (10 slots) and gpAmount 30000000
- Wait for 1 slot
- Wallet 1: give to gpBeneficiary 80a... (wallet 2) with gpDeadline of 159... (20 slots) and gpAmount 30000000
- Wait for 1 slot
- Wallet 1: give to gpBeneficiary 2e0... (wallet 3) with gpDeadline of 159... (10 slots) and gpAmount 30000000
- Wait until slot 11
- Wallet 2: grab
- Wallet 3: grab
- Wait for 5 slots

The genesis slot gives each wallet their starting balance. Then wallet 1 gives
the three amounts, then we wait until slot 11. When wallets 2 and 3 try to grab,
all but the UTxO for wallet 2 with deadline of slot 20 succeed.

If you change the wait between giving and grabbing to 21, both grabs succeed
again and this time wallet 2's grab picks up both UTxOs where they are a
beneficiary because, this time, the deadline for both has passed.

### Validating Off- and On-chain

The way we wrote the wallet code makes it so that the on-chain validator is
never really used. The transaction will fail in the wallet before being
submitted to the blockchain because of the checks we put in place.

It's still very important to write the on-chain validation because a user
doesn't necessary have to use our script to interact with the UTxOs. They could
write their own script which bypasses the beneficiary and deadline off-chain
checks and submit it to the blockchain. At this point, if we hadn't written the
on-chain validator, all the ADA could be drained out of every UTxO that uses our
script.
