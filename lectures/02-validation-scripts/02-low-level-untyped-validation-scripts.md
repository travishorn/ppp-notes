---
label: Low Level, Untyped Validation Scripts
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Validation Scripts

## Low Level, Untyped Validation Scripts

Lecture 2, Part 2

[Source Video
:icon-link-external:](https://www.youtube.com/watch?v=xgnmMl-eIIM&list=PLNEK_Ejlx3x0mhPmOjPSHZPtTFpfJo3Nd&index=2)

There are 2 parts to a smart contract: an on-chain part and an off-chain part.
The on-chain part is all about validation. The off-chain part lives in the
user's wallet and constructs suitable transactions.

Normal UTxO model uses public key addresses to identify an output. An address
can be used as an input as long as it's signed with the corresponding signature
for that address.

The EUTxO model extends this and allows "script" addresses in addition to public
key addresses. When a transaction uses a script as an input, the node runs the
script and determines whether the transaction is valid or not.

A script can act on three pieces of information:

- An arbitrary piece of data called the "datum"
- A "redeemer" that is used in place of a signature for validation
- The "context", which contains all inputs and outputs for the transaction

These 3 pieces use the same Haskell datatype at a low level:
[`PlutusTx.BuiltinData`
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-tx/html/PlutusTx.html#t:BuiltinData)

BuiltinData doesn't have a constructor. Instead you use a conversion [from
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-tx/html/PlutusTx.html#v:dataToBuiltinData)
/ [to
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-tx/html/PlutusTx.html#v:builtinDataToData)
the [PlutusTx.Data
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-tx/html/PlutusTx.html#t:Data)
datatype.

### The `Data` Datatype

`Data` has multiple constructors:

- **Constr**: `Integer [Data]`
- **Map**: `[(Data, Data)]` for lists of key/value pairs
- **List**: `[Data]`
- **I**: `Integer` for numbers
- **B**: `ByteString` for strings

It's very similar to JavaScript's JSON. You can see information about the type
in the Cabal REPL. Follow the instructions from lecture 1 about [building the
example code](../01-eutxo-english-auction/03-building-example-code.md), but
replacing week 1 with week 2 where necessary. After following those steps,
launch the REPL.

```bash
cabal repl
```

Import the `PlutusTx` module

```haskell
import PlutusTx
```

Get information about `Data`

```haskell
:i Data
```

You can view a structure with this type. Let's use the `I` constructor to create
a `Data` with an integer.

```haskell
:t I 42
I 42 :: Data
```

You can do the same with a string (or more accurately, a ByteString), but you'll
need to [enable overloaded strings in
GHC](../../appendix/enable-overloaded-strings.md) first.

```haskell
:set -XOverloadedStrings
```

View the type information for a `Data` structure created with the `B` bytestring
constructor.

```haskell
:t B "Haskell"
B "Haskell" :: Data
```

How about with key/value pairs?

```haskell
:t Map [(I 42, B "Haskell"), (List [I 0], I 1000)]
Map [(I 42, B "Haskell"), (List [I 0], I 1000)] :: Data
```

### Gift.hs

[Source Code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/Gift.hs)

[My re-written code with comments
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week02/src/Week02/Gift.hs)

While writing code in a .hs file, make sure you have a REPL running with the
module loaded.

```haskell
:l src/Week02/Gift.hs
```

And reload often.

```haskell
:r
```

If you write too many lines, then try to reload and see a bunch of compilation
errors, you'll have a much harder time debugging than if you only wrote a few
lines of code and see one compilation error.

The most important line is the validator logic. This one ignores all 3 inputs
and always passes.

```haskell
mkValidator _ _ _ = ()
```

Once you've written the lines for `valhash`, you can reload the module in the
REPL and view the hash

```haskell
valHash
67f3314...
```

You can do the same with `scrAddress`

```haskell
scrAddress
Address {
  addressCredential = ScriptCredential 67f3314...,
  addressStakingCredential = Nothing
}
```

The scripts credential is basically just its `valHash`.

Copy & paste the contents of Gift.hs to the playground to simulate it. Try
things like 3 wallets, 2 give and then 1 grabs, etc.


### Burn.hs

[Source Code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/Burn.hs)

[My re-written code with comments
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week02/src/Week02/Burn.hs)

Almost identical to Gift.hs. The difference is the validator always throws an
error instead of always passing.

```haskell
mkValidator _ _ _ = error ()
```

When using `error` such as `mkValidator _ _ _ = error ()` you may think you are
using the standard `error` type included in the Haskell Prelude. But since we
are using the language extension `NoImplicitPrelude` none of the standard
Haskell Prelude types are loaded. You are actually using
`PlutusTx.Prelude.error`.

```haskell
:t PlutusTx.Prelude.error
PlutusTx.Prelude.error :: () -> a
```

Even better is `traceError`

```haskell
mkValidator _ _ _ = traceError "BURNT!"
```

### FortyTwo.hs

Now for a validator that doesn't ignore all its inputs.

[Source Code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/FortyTwo.hs)

[My re-written code with comments
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week02/src/Week02/FortyTwo.hs)

In this case, the `grab` endpoint asks for an integer. If that integer is the
hard-coded value `42` the transaction passes. Otherwise, it fails.
