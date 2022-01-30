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

Homework 2 is to write a validator that parameterizes the `Vesting.hs`. This
time the beneficiary and the datum are split into two separate parameters
instead of a single data type.

[Homework code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/037142877d7275d47314af21413d803dc58a1da3/code/week03/src/Week03/Homework2.hs)

[My solution
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week03/src/Week03/Homework2.hs)

The type signature looks like this:

```haskell
mkValidator :: PaymentPubKeyHash -> POSIXTime -> () -> ScriptContext -> Bool
```

I wrote the validator by copying from `Vesting.hs` and replacing `dat` (which
was the custom data type containing both parameters) with two separate
parameters `beneficiary deadline`.

```haskell
mkValidator beneficiary deadline () ctx =
```

The body of the validator is the same as `Vesting.hs`, but with the parameters
referenced directly instead of through `dat`.

```haskell
mkValidator beneficiary deadline () ctx =
  traceIfFalse "beneficiary's signature missing" signedByBeneficiary &&
  traceIfFalse "deadline not reached" deadlineReached
    where
        info :: TxInfo
        info = scriptContextTxInfo ctx

        signedByBeneficiary :: Bool
        signedByBeneficiary = txSignedBy info $ unPaymentPubKeyHash $ beneficiary

        deadlineReached :: Bool
        deadlineReached = contains (from $ deadline) $ txInfoValidRange info
```

The `typedValidator` is copied almost exactly from `Parameterized.hs`. The only
change is the parameters passed to `Scripts.wrapValidator` take into account the
deadline `POSIXTime` parameter.

```haskell
typedValidator :: PaymentPubKeyHash -> Scripts.TypedValidator Vesting
typedValidator p = Scripts.mkTypedValidator @Vesting
    ($$(PlutusTx.compile [|| mkValidator ||]) `PlutusTx.applyCode` PlutusTx.liftCode p)
    $$(PlutusTx.compile [|| wrap ||])
  where
    wrap = Scripts.wrapValidator @POSIXTime @()
```

The `validator` and `scrAddress` are copied straight from `Parameterized.hs`.
