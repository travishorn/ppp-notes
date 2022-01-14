# Converting Slots to POSIX Time

The time in Plutus is described in POSIX time (the number of seconds that have
elapsed since 1 January 1970 00:00:00). But the time in Plutus Playground is
described in "slots".

There is a function in Plutus that will convert slots to POSIX time called
`slotToEndPOSIXTime`. It takes the parameters `SlotConfig` (which is a defined
standard for Plutus Playground) and `Slot` which is the number of slots.

You can find out what (for example) 10 slots past the start time of Plutus
Playground's standard config is with this function.

In a Nix shell that was started in the `plutus-apps` directory, change to the
`plutus-pioneer-program/code/week01` directory.

Then, start the REPL

```bash
cabal repl
```

In the REPL, import the following modules

```haskell
import Data.Default
import Ledger.TimeSlot
```

Now you can use the function to determine the POSIX time for the end of 10
slots.

```haskell
slotToEndPOSIXTime def 10
```

The corresponding POSIX time is shown:

```
POSIXTime {getPOSIXTime = 1596059101999}
```
