---
label: Converting Slots to POSIX Time
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Converting Slots to POSIX Time

The time in Plutus is described in POSIX time (the number of milliseconds that
have elapsed since 1 January 1970 00:00:00.000). But the time in Plutus
Playground is described in "slots". Each slot is 1000 milliseconds starting at
29 July 2020 9:44:51.000.

Slot 0 (the first slot) ends at 29 July 2020 9:44:51.999 (1596059091999).

Slot 1 ends at 29 July 2020 9:44:52.999 (1596059092999).

And so on.

If you need to calculate the POSIX time at the end of a specific slot, there is
a function in Plutus that will convert slots to POSIX time called
`slotToEndPOSIXTime`. It takes two parameters:

- `SlotConfig`: A datatype to configure the length (ms) of one slot and the
  beginning of the first slot
- `Slot`: The number of slots to show the time for

`SlotConfig` for Plutus Playground is a defined standard.

You can find out what (for example) 10 slots past the start time of Plutus
Playground's standard config is with this function.

Change into the `plutus-apps` directory

```bash
cd plutus-apps
```

Start a Nix shell

```bash
nix-shell
```

Change into the Plutus Pioneer Program's week 1 directory

```bash
cd plutus-pioneer-program/code/week01
```

Start the REPL

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

```haskell
POSIXTime {getPOSIXTime = 1596059101999}
```

Some common conversions are:

| Slot |    POSIX Time | Human Readable           |
|-----:|--------------:|:-------------------------|
|    0 | 1596059091999 | 29 July 2020 9:44:51.999 |
|    1 | 1596059092999 | 29 July 2020 9:44:52.999 |
|    2 | 1596059093999 | 29 July 2020 9:44:53.999 |
|    3 | 1596059094999 | 29 July 2020 9:44:54.999 |
|    4 | 1596059095999 | 29 July 2020 9:44:55.999 |
|    5 | 1596059096999 | 29 July 2020 9:44:56.999 |
|    6 | 1596059097999 | 29 July 2020 9:44:57.999 |
|    7 | 1596059098999 | 29 July 2020 9:44:58.999 |
|    8 | 1596059099999 | 29 July 2020 9:44:59.999 |
|    9 | 1596059100999 | 29 July 2020 9:45:00.999 |
|   10 | 1596059101999 | 29 July 2020 9:45:01.999 |
|   20 | 1596059111999 | 29 July 2020 9:45:11.999 |
|   30 | 1596059121999 | 29 July 2020 9:45:21.999 |
|   40 | 1596059131999 | 29 July 2020 9:45:31.999 |
|   50 | 1596059141999 | 29 July 2020 9:45:41.999 |
|  100 | 1596059191999 | 29 July 2020 9:46:31.999 |
|  200 | 1596059291999 | 29 July 2020 9:48:11.999 |
|  300 | 1596059391999 | 29 July 2020 9:45:01.999 |
|  400 | 1596059491999 | 29 July 2020 9:49:51.999 |
|  500 | 1596059591999 | 29 July 2020 9:53:11.999 |
