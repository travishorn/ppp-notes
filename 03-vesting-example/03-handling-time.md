---
label: Handling Time
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -3
---

# Vesting Example

## Handling Time

Lecture 3, Part 3

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=mf06ll-4j2w&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=3)

Under most circumstances you can always tell whether a transaction will fail
before submitting it to the blockchain. Validation can and does happen
off-chain in a user's wallet.

However, the time when a wallet validates a transaction off-chain will be
different than the time when the transaction arrives at a node. The
`txInfoValidRange` of a `ScriptContext` handles this.

When a node processes a transaction it does basic checks first before running a
script. It is at this time that the current time is checked to be within the
valid time range.

By default, transactions use the infinite time range. This can be change, such
as in the auction example from lecture 1.

Cardano uses **slots** as a measurement of time. Each slot is 1 second. So it's
easy to go back and forth between real time and slots. The definition of a slot
could possibly change in the future, so transactions shouldn't have a valid
range that is too far in the future. A change to the slot definition requires a
hard fork, which comes with an announcement at least 36 hours in advance.

`txInfoValidRange` has type `POSIXTimeRange` which contains an `Interval` and a
`POSIXTime`. `Interval` optionally contains a `LowerBound` and an `UpperBound`.

There are a number of convenient functions for working with `Intervals`. Some
examples:

- `member` checks whether a value is included in an interval
- `always` is an interval that covers every slot
- `from` constructs an interval that starts at a given value and goes on forever
- `to` is the opposite of `from`
- `before` checks if a value is earlier than a given interval
- `after` is the opposite of `before`

You can play around with intervals by starting up a Nix shell in `plutus-apps`

```bash
cd plutus-apps
nix-shell
```

Then change into this week's directory

```bash
cd ~/plutus-pioneer-program/code/week03
```

Start a REPL

```bash
cabal repl
```

Import the Interval module

```haskell
import Plutus.V1.Ledger.Interval
```

See what an Interval looks like between slot 10 and 20

```haskell
tenToTwenty = interval (10 :: Integer) 20
tenToTwenty
Interval {ivFrom = LowerBound (Finite 10) True, ivTo = UpperBound (Finite 20) True}
```

This can be read as "an interval with lower bound 10 inclusive (10 is included)
and upper bound 20 inclusive" or even more simply "from slot 10 to 20".

You can check whether a value is a member of this interval

```haskell
member 9 tenToTwenty
False

member 10 tenToTwenty
True

member 15 tenToTwenty
True

member 20 tenToTwenty
True

member 21 tenToTwenty
False
```

Another couple more interval examples

```haskell
fromThirty = from (30 :: Integer)
toThirty = to (30 :: Integer)

member 999 fromThirty
True

member 999 toThirty
False

member 10 fromThirty
False

member 10 toThirty
True
```

You can get the intersection of two intervals

```haskell
tenToTwenty = interval (10 :: Integer) 20
twelveToThirty = interval (12 :: Integer) 30

intersection tenToTwenty twelveToThirty
Interval {ivFrom = LowerBound (Finite 12) True, ivTo = UpperBound (Finite 20) True}
```

You can see we get a new interval where the two inputs intersect (12 to 20).
