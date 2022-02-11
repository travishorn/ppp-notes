---
label: Values
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Minting Policy

## Values

Lecture 5, Part 2

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=4iNTgjovMRg&list=PLNEK_Ejlx3x0G8V8CDBnRDZ86POVsrfzw&index=2)

The Plutus module that deals with values is [Plutus.V1.Ledger.Value
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V1-Ledger-Value.html).

The [Value
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V1-Ledger-Value.html#t:Value)
type:

```haskell
getValue :: Map CurrencySymbol (Map TokenName Integer)
```

Every native token (including ADA) is identified by two things:

- A currency symbol
- A token name

Both [CurrencySymbol
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V1-Ledger-Value.html#t:CurrencySymbol)
and [TokenName
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V1-Ledger-Value.html#t:TokenName)
are wrappers around `BuiltinByteString`.

[AssetClass
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger-api/html/Plutus-V1-Ledger-Value.html#t:AssetClass)
is a wrapper around a `CurrencySymbol` and `TokenName` pair.

Going back to the `Value` type, it's equivalent to a map from `AssetClass` to
`Integer`. Meaning that a value is basically how many of a given unit an
AssetClass contains.

These notes are incomplete. Up to timestamp 2:00 in the lecture video.
