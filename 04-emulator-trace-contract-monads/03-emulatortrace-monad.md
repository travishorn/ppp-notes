---
label: The EmulatorTrace Monad
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -3
---

# Emulator Trace & Contract Monads

## The EmulatorTrace Monad

Lecture 4, Part 3

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=qoUfgaHs1CI&list=PLNEK_Ejlx3x230-g-U02issX5BiWAgmSi&index=3)

One of the important monads in Plutus is the `EmulatorTrace` monad. With this
monad, you can test out Plutus code without having to start a Plutus Playground,
copy & pasting a contract into it, and setting up a simulation through the GUI.

It's defined in [the `Plutus.Trace.Emulator`
package in `plutus-contract`
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-contract/html/Plutus-Trace-Emulator.html).

The way to run an emulator trace is by calling [`runEmulatorTrace`
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-contract/html/Plutus-Trace-Emulator.html#v:runEmulatorTrace)

```haskell
runEmulatorTrace :: EmulatorConfig ->
                    EmulatorTrace () ->
                    ([EmulatorEvent], Maybe EmulatorErr, EmulatorState)
```

The first argument is an [`EmulatorConfig`
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-contract/html/Plutus-Trace-Emulator.html#t:EmulatorConfig)

The constructor for this config contains 3 items:

- `_initialChainState`
- `_slotConfig`
- `_feeConfig`

Let's look at [the `InitialChainState` type
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-contract/html/Wallet-Emulator-Stream.html#t:InitialChainState)

```haskell
type InitialChainState = Either InitialDistribution TxPool
```

It can take either an `InitialDistribution` or a `TxPool`.

`InitialDistribution` is a map of a wallet value. It specified what wallets you
want to emulate and what values they contain (initial funding).

```haskell
type InitialDistribution = Map Wallet Value
```

You could also use `TxPool` instead to define transactions that have "happened"
before the emulation.

The second item in the `EmulatorConfig` constructor is a [`SlotConfig`
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger/html/Ledger-TimeSlot.html#t:SlotConfig).

This has to do with time. It allows you to set the length of 1 slot and the
beginning of slot 0.

The third and final item in the `EmulatorConfig` constructor is a [`FeeConfig`
:icon-link-external:](https://playground.plutus.iohkdev.io/doc/haddock/plutus-ledger/html/Ledger-Fee.html#t:FeeConfig).
It defines the constant fee per transaction and the factor by which to multiply
that fee to scale script execution.

### Setting up the REPL

Inside a REPL, import the necessary modules.

```haskell
import Plutus.Trace.Emulator
import Data.Default
```

In a moment we'll need to pass an `EmulatorConfig` to `runEmulatorTrace`. We can
use the [default
configuration](../appendix/prettyprinted-default-emulatorconfig.md).

We'll also need to pass an emulator trace. We haven't look at emulator traces yet. But you can see from `runEmulatorTrace`'s info that `EmulatorTrace` is a monad. Since we know it's a monad, we know we can use the simplest emulator trace we can do is `return ()`.

## runEmulatorTrace

It won't do anything special. It will just immediately return unit ().

```haskell
runEmulatorTrace def $ return ()
```

Even with the simplest version of this trace, an overwhelming amount of data is
printed to the console. It's virtually unusable.

### runEmulatorTraceIO

Instead, you can use `runEmulatorTraceIO` which uses the default config
automatically and runs in IO. The result is much more manageable, too.

```haskell
runEmulatorTraceIO $ return ()

Slot 00000: TxnValidate 98d5fbcefe21113b3f0390c1441e075b8a870cc5a8fa2a56dcde1d8247e41715
Slot 00000: SlotAdd Slot 1
Slot 00001: W1bc5f27: InsertionSuccess: New tip is Tip(slot= Slot 1, blockId= BlockId(1033bd6bfb9d90108db08880ad32a58980ae8dafd14c6217d7f83db6fae6f70c), blockNo= 0). UTxO state was added to the end.
-- and so on
```

At the very beginning, a transaction is validated. That's the genesis block
that's funding the wallets. Then it waits for 2 slots. Then it shows the final
balances.

Since we didn't do anything in the emulator trace, nothing really happened and
all the wallets still have their initial 100000000 Lovelace.

```haskell
Final balances
Wallet 1bc5f27d7b4e20083977418e839e429d00cc87f3: 
    {, ""}: 100000000
Wallet 3a4778247ad35117d7c3150d194da389f3148f4a: 
    {, ""}: 100000000
-- and so on
```

### runEmulatorTraceIO'

If you want to specify a different emulator configuration, you can use
`runEmulatorTraceIO'`.

```haskell
:t runEmulatorTraceIO'
runEmulatorTraceIO'
  :: TraceConfig -> EmulatorConfig -> EmulatorTrace () -> IO ()
```

Notice it takes a new argument we haven't seen yet: `TraceConfig`.

```haskell
:t TraceConfig
TraceConfig
  :: (Wallet.Emulator.MultiAgent.EmulatorEvent' -> Maybe String)
     -> GHC.IO.Handle.Types.Handle -> TraceConfig
```

It basically allows you to filter events in the trace. There is an overwhelming
amount of events to trace. You can filter to see only the ones you want using
`TraceConfig` and the argument `Wallet.Emulator.MultiAgent.EmulatorEvent'`

### Trace.hs

All of my notes for this section are [comments in the file itself
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Trace.hs).

Try out this trace emulation in the REPL by loading Trace.hs and calling `test`

```haskell
:l src/Week04/Trace.hs
test
```

The output has all the information about the emulation. For example, you'll see
a lot of information at the start in the genesis block about initially funding
the wallets

Later in slot 2 you'll see where `logInfo` (from [Vesting.hs line 99
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Vesting.hs#L99))
was called.

```haskell
Slot 00002: *** CONTRACT LOG: "made a gift of 10000000 lovelace to 80a4f45b56b88d1139da23bc4c3c75ec6d32943c087f250b86193ca7 with deadline POSIXTime {getPOSIXTime = 1596059111000}"
```

You'll see a lot of waiting for the next few slots.

Then the wallet 2's `grab` transaction is submitted in slot 20.

Then you'll see the log from `Trace.hs` and the log from the `grab` in
`Vesting.hs` in slot 22.

```haskell
Slot 00022: *** USER LOG: reached Slot {getSlot = 22}
Slot 00022: *** CONTRACT LOG: "collected gifts"
```

Finally you'll again see the final wallet balances.

```haskell
Wallet 7ce812d7a4770bbf58004067665c3a48f28ddd58: 
    {, ""}: 109995870
Wallet 872cb83b5ee40eb23bfdab1772660c822a48d491: 
    {, ""}: 89999990
```

### Failing Transaction

You can modify `Trace.hs` to make the grab fail by not waiting long enough to
pass the deadline. In that case, the trace output will reflect that there was no
successful `grab`.
