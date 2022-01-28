---
label: Parameterized Contracts
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -5
---

# Vesting Example

## Parameterized Contracts

Lecture 3, Part 5

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=XqFILXV_ACM&list=PLNEK_Ejlx3x2zxcfoVGARFExzOHwXFCCL&index=5)

### Parameterized.hs

[Source code
:icon-link-external](https://github.com/input-output-hk/plutus-pioneer-program/blob/037142877d7275d47314af21413d803dc58a1da3/code/week03/src/Week03/Parameterized.hs)

Most of my notes for this part are done as comments directly in source code.

[My commented code :icon-link-external](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week03/src/Week03/Parameterized.hs)

All of the examples so far have a fixed datum type. In `Vesting.hs`, the datum type is always...

```haskell
data VestingDatum = VestingDatum
    { beneficiary :: PaymentPubKeyHash
    , deadline    :: POSIXTime
    } deriving Show
```

You can parameterize this so that other types of datum can be used. The
commented source code above goes much more into detail about this.

With `Vesting.hs` parameterized and saved as `Parameterized.hs`, you can try the
code out in the Playground.

Set up the simulation almost identically to when we tried out `Vesting.hs`.

The only difference that you'll see is that the `grab` endpoint now requires an
argument. That argument is the deadline. Wallets now grab UTxOs where they are
the beneficiary and the deadline is a specified time.
