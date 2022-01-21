# Validation Scripts

Lecture 2, Part 3

## High Level, Typed Validation Scripts

[Source
Video](https://www.youtube.com/watch?v=HoB_PqeZPNc&list=PLNEK_Ejlx3x0mhPmOjPSHZPtTFpfJo3Nd&index=3)

Using BuiltinData works great and it's very performant. However, you can code
validators with more descriptive datatypes.

### Typed.hs

[Source code](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/Typed.hs)

Instead of writing the validator like we did in the `FortyTwo` example...

```haskell
mkValidator :: BuiltinData -> BuiltinData -> BuiltinData -> ()
mkValidator = _ r _
  | r == Builtins.mkI 42 = ()
  | otherwise = traceError "Wrong redeemer"
```

We can use stronger types for each argument. Since we don't care about the datum
we can just give it type `()`. The redeemer should be an `Integer`. And even
though we don't care about the context, it's good practice to use
`ScriptContext`. Finally, the validator either passes or it doesn't so `Bool` is
a much more natural-feeling return type.

```haskell
mkValidator :: () -> Integer -> ScriptContect -> Bool
mkValidator = _ r _ = traceIfFalse "Wrong redeemer" $ r == 42
```

Notice `traceIfFalse`. It takes a string and an expression. If the expression is
true, it returns true. If the expression is false, it traces (logs) the string
and returns false.

With high level typed validation scripts there is a little more boilerplate
needed to compile.

Two new blocks are needed:

```haskell
data Typed
instance Scripts.ValidatorTypes Typed where
    type instance DatumType Typed = ()
    type instance RedeemerType Typed = Integer

typedValidator :: Scripts.TypedValidator Typed
typedValidator = Scripts.mkTypedValidator @Typed
    $$(PlutusTx.compile [|| mkValidator ||])
    $$(PlutusTx.compile [|| wrap ||])
  where
    wrap = Scripts.wrapValidator @() @Integer
```

`validator` and `valHash` need to be modified.

```haskell
validator :: Validator
validator = Scripts.validatorScript typedValidator

valHash :: Ledger.ValidatorHash
valHash = Scripts.validatorHash typedValidator
```

`scrAddress` stays the same.

### fromData and toData

In the REPL, you can see how these two functions work.

Turning an integer into data uses the `I` constructor.

```haskell
toData (42 :: Integer)
I 42
```

And the other way around. Notice we use `Maybe` because the operation can fail.

```haskell
FromData (I 42) :: Maybe Integer
Just 42
```

It fails and returns Nothing when you try to go from a list to an integer.

```haskell
FromData (List []) :: Maybe Integer
Nothing
```

### Custom Datatype

[Source code](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/IsData.hs)

[My commented code](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week02/src/Week02/IsData.hs)

With this module loaded in the REPL, we can convert the new type to `Data`.

```haskell
import PlutusTx
import PlutuxTx.IsData.Class
toData $ MySillyRedeemer 42
Constr 0 [I 42]
```

[NEXT: Part 4: Summary](./04-summary.md)
