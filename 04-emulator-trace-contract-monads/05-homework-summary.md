---
label: Homework & Summary
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -5
---

# Emulator Trace & Contract Monads

## Homework & Summary

Lecture 4, Part 5

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=sxRLzR0jdiY&list=PLNEK_Ejlx3x230-g-U02issX5BiWAgmSi&index=5)

### Homework

[Homework contract
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/f3102346284eca5bd9dbf40686d5a227d71620c7/code/week04/src/Week04/Homework.hs)

[My solution
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Homework.hs)

The homework this week is to implement an EmulatorTrace where wallet 1 pays
wallet 2 twice for amounts that are specified when running the trace.

The solution I came up with was:

```haskell
-- Need functions from this module to work with wallets
import Wallet.Emulator.Wallet

payTrace :: Integer -> Integer -> EmulatorTrace ()

-- Use do notation to bind a sequence of monadic actions
payTrace x y = do
    -- Activate the contract with wallet 1 and bind it's handle to h1
    h1 <- activateContractWallet (knownWallet 1) payContract

    -- Call the pay endpoint with wallet 1. Params are:
    -- ppRecipient = the pub key hash for wallet 2
    -- ppLovelace  = the first value passed in when running the trace
    callEndpoint @"pay" h1 $ PayParams
        { ppRecipient = mockWalletPaymentPubKeyHash $ knownWallet 2
        , ppLovelace  = x
        }

    -- Wait 1 slot
    void $ Emulator.waitNSlots 1

    -- Call the pay endpoint again. This time ppLovelace is the 2nd value passed
    -- in when running the trace
    callEndpoint @"pay" h1 $ PayParams
        { ppRecipient = mockWalletPaymentPubKeyHash $ knownWallet 2
        , ppLovelace  = y
        }

    -- Wait 2 more slots to let the transactions finalize
    void $ Emulator.waitNSlots 2
```

This works great with the given `payContract` when there are no errors.

The second part of the homework was to ensure that the script would continue to
pay wallet 2 the second amount even if the first failed. For that, I had to
modify `payContract`:

```haskell
-- In addition to Text, we need unpack when handling the error
import Data.Text             (Text, unpack)

payContract :: Contract () PaySchema Text ()
payContract = do
    pp <- awaitPromise $ endpoint @"pay" return
    let tx = mustPayToPubKey (ppRecipient pp) $ lovelaceValueOf $ ppLovelace pp

    -- Make sure the contract doesn't stop when an error occurs. Catch and
    -- handle the error. This lets the script continue running even if the first
    -- pay errors with insufficient funds
    handleError (\err -> Contract.logError $ "caught: " ++ unpack err) $
      void $ submitTx tx

    payContract
```

### Summary

In this lecture, we learned about

- [monads](./02-monads.md) - how they work and why they're important
- [the EmulatorTrace monad](./03-emulatortrace-monad.md) - scripting scenarios
  instead of using the GUI Playground
- [the Contract monad](./04-contract-monad.md) - writing off-chain code

You can do much more with these monads that we haven't covered. Basically
anything you could do with the Cardano CLI. Things like querying the blockchain,
constructing transactions, submitting transactions, etc.
