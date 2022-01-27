---
label: Script Contexts
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Vesting Example

## Script Contexts

Lecture 3, Part 2

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=B66xLrGXwmw&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=2)

Remember that a validator takes 3 arguments:

- Datum
- Redeemer
- Script context

The script context is the focus of this lecture. The type is defined in
[Plutus.V1.Ledger.API
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V1-Ledger-Api.html#t:ScriptContext).
That package must be added to your Cabal configuration when you want to use
`ScriptContext`. The module is defined in [Plutus.V2.Ledger.Contexts
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V2-Ledger-Contexts.html#t:ScriptContext)

Its constructor looks like this:

```
ScriptContext
  scriptContextTxInfo :: TxInfo
  scriptContextPurpose :: ScriptPurpose
```

The purpose can be one of:

- Minting
- Spending (the one we've been most concerned with)
- Rewarding
- Certifying

TxInfo describes much information about the transaction:

- Inputs
- Outputs - think about inputs and outputs as they relate to the UTxO model
- Fee
- Forge - the amount of newly forged native tokens
- DCert
- Wdrl - staking withdrawals / rewards
- ValidRange - the time range the tx is valid for
- Signatories - any signatures provided (list of public keys)
- Data - scripts that spend an output need to include the datum. Scripts that send money to a script provide the has of the datum
- Id
