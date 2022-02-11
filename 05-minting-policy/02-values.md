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

## Interacting with the Value Module

You can play with the module in the REPL. After doing the [weekly
setup](../weekly-setup.md), change into this week's directory.

```bash
cd plutus-pioneer-program/code/week05/
```

Update Cabal

```bash
cabal update
```

Start a REPL

```bash
cabal repl
```

Import relevant modules

```haskell
import Plutus.V1.Ledger.Value
import Plutus.V1.Ledger.Ada
```

Set overloaded strings

```haskell
:set -XOverloadedStrings
```

`adaSymbol` is of type `CurrencySymbol`

```haskell
:t adaSymbol
adaSymbol :: CurrencySymbol
```

The symbol itself is an empty bytestring.

```haskell
adaSymbol

-- Empty bytestring returned above
```

`adaToken` is of type `TokenName` and it's similarly an empty string.

```haskell
:t adaToken
adaToken :: TokenName

adaToken
""
```

To construct a value containing just Lovelace (ADA), use `lovelaceValueOf`

```haskell
:t lovelaceValueOf  
lovelaceValueOf :: Integer -> Value

lovelaceValueOf 123 
Value (Map [(,Map [("",123)])])
```

The absense of anything before that first comma indicates the `adaSymbol`.
Remember it's an empty bytestring. The absense of anything inside the quotes
indicates the `adaToken`.

### Combining Values

The `Value` type is a monoid.

Monoids are things you can combine with a natural element called `mempty`, and
things you can combine two elements into a third with `mappend`. `mappend` has
an infix operator `<>`.

```haskell
lovelaceValueOf 123 <> lovelaceValueOf 10
Value (Map [(,Map [("",133)])])
```

### Constructing a Value

Use `singleton` to construct a value with a native token.

```haskell
:t singleton
singleton :: CurrencySymbol -> TokenName -> Integer -> Value
```

You must pass the currency symbol as a hexidecimal value.

```haskell
singleton "a8ff" "ABC" 7
Value (Map [(a8ff,Map [("ABC",7)])])
```

The outer map contains the the currency symbol and the inner map. The inner map
contains the token name and the amount.

You can combine values like before

```haskell
singleton "a8ff" "ABC" 7 <> lovelaceValueOf 42 <> singleton "a8ff" "XYZ" 100
Value (Map [(,Map [("",42)]),(a8ff,Map [("ABC",7),("XYZ",100)])])
```

Now, the outer map contains two instances of a currency symbol and an inner map.
The first inner map is similar to what we've seen before, but the second inner
map (the one with `a8ff` as the currency symbol) contains two instances of token
names: `"ABC"` and `"XYZ"`.

Assign that value to variable `v`

```haskell
let v = let v = singleton "a8ff" "ABC" 7 <> lovelaceValueOf 42 <> singleton "a8ff" "XYZ" 100
```

Get the amount given a value, currency symbol, and token name

```haskell
:t valueOf
valueOf :: Value -> CurrencySymbol -> TokenName -> Integer

valueOf v "a8ff" "ABC"
7
```

### Flatten a Value

```haskell
:t flattenValue
flattenValue :: Value -> [(CurrencySymbol, TokenName, Integer)]

flattenValue v
[(,"",42),(a8ff,"XYZ",100),(a8ff,"ABC",7)]
```

### Why both a currency symbol and token name?

A transaction can't create or delete tokens. Everything that goes in must come
out, with the exception of fees. Fees are based on size of transaction in bytes
(not value). 

The currency symbol is actually a hash of a script. This type of script is
called a "minting policy."

The minting script is executed alongside the validation scripts in a
transaction. The purpose of the minting script is to determine if the
transaction has the rights to mint or burn tokens.

Minting scripts are similar to validation scripts but not identical.
