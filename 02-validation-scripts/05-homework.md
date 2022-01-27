---
label: Homework
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -5
---

# Validation Scripts

## Homework

Lecture 2, Part 5

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=_r-EpXzQGKo&list=PLNEK_Ejlx3x0mhPmOjPSHZPtTFpfJo3Nd&index=5)

Two modules are provided: `Homework01.hs` and `Homework02.hs`.

### Homework 1

The redeemer has been changed to a pair of `Bool`s. Validation should only
succeed if they match.

[Homework code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/Homework1.hs)

[My solution
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week02/src/Week02/Homework1.hs)

Before fixing it, the validator in the homework code always passes.

```haskell
mkValidator _ _ _ = True -- FIX ME!
```

The correct validator can be written like so:

```haskell
mkValidator _ (x, y) _ = traceIfFalse "Redeemer booleans do not match" $ x == y
```

Additionally, the homework code had a broken type instance and broken
`validator`, `valHash`, and `scrAddress` definitions. These are mostly
boilerplate that could be easily copied from the working examples written
earlier in the lecture.

### Homework 2

It's the same as Homework 1 except the redeemer is a custom type called
`MyRedeemer` that has two fields called `flag1` and `flag2` that are just
`Bool`s.

[Homework code
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program/blob/0f24e987e79a369b3d34f62d6e0cbc1b527082fb/code/week02/src/Week02/Homework2.hs)

[My solution
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week02/src/Week02/Homework2.hs)

Since the redeemer is a custom type that looks like this:

```haskell
data MyRedeemer = MyRedeemer
    { flag1 :: Bool
    , flag2 :: Bool
    } deriving (Generic, FromJSON, ToJSON, ToSchema)
```

The correct validator can be written like so:

```haskell
mkValidator _ (MyRedeemer x y) _ =
  traceIfFalse "Redeemer booleans do not match" $ x == y
```

And again, the homework code had broken boilerplate that could be copied from
working examples written earlier in the lecture.
