---
label: Homework
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -7
---

# Vesting Example

## Homework

Lecture 3, Part 7

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=GGUT2O_0urQ&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=7)

### Homework 1

Homework 1 is to implement an on-chain validator that allows a wallet to lock
ADA in a contract. The contract has beneficiary addresses and a deadline. Before
the deadline, only beneficiary 1 should be able to grab the ADA. But after the
deadline, only beneficiary 2 should be able to grab the ADA.

[Homework code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/037142877d7275d47314af21413d803dc58a1da3/code/week03/src/Week03/Homework1.hs)

[My solution
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week03/src/Week03/Homework1.hs)

My solution uses guards to match 2 cases:

1. Where beneficiary 1 signed and the deadline not reached
2. Where beneficiary 2 signed and deadline reached

Otherwise, false.

I use `where` to define some helper expressions. `signedByBeneficiary1` and `2`
are modeled after `signedByBeneficiary` in `Vesting.hs`. The other expressions
are taken right out of `Vesting.hs` with no changes necessary.

I use `deadlineReached` with the `not` function to reverse it when I want to
check if the deadline has not been reached.

Here's the fixed validator:

```haskell
mkValidator :: VestingDatum -> () -> ScriptContext -> Bool
mkValidator dat () ctx
  | signedByBeneficiary1 && not deadlineReached = True
  | signedByBeneficiary2 &&     deadlineReached = True
  | otherwise                                   = False
    where
      info :: TxInfo
      info = scriptContextTxInfo ctx

      signedByBeneficiary1 :: Bool
      signedByBeneficiary1 = txSignedBy info $ unPaymentPubKeyHash $ beneficiary1 dat

      signedByBeneficiary2 :: Bool
      signedByBeneficiary2 = txSignedBy info $ unPaymentPubKeyHash $ beneficiary2 dat

      deadlineReached :: Bool
      deadlineReached = contains (from $ deadline dat) $ txInfoValidRange info
```

### Homework 2

I haven't started homework 2, yet.

[Homework code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/037142877d7275d47314af21413d803dc58a1da3/code/week03/src/Week03/Homework2.hs)

[My solution
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week03/src/Week03/Homework2.hs)
