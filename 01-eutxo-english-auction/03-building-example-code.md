---
label: Building the Example Code
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -3
---

# EUTxO and English Auction

## Building the Example Code

Lecture 1, Part 3

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=zPaDp4R9X7o&list=PLNEK_Ejlx3x2nLM4fAck2JS6KhFQlXq2N&index=3)

There are three repositories:

The code for the course is located at
[https://github.com/input-output-hk/plutus-pioneer-program
:icon-link-external:](https://github.com/input-output-hk/plutus-pioneer-program)

Plutus code for off-chain things is located at
[https://github.com/input-output-hk/plutus-apps
:icon-link-external:](https://github.com/input-output-hk/plutus-apps)

Plutus code for on-chain things is located at
[https://github.com/input-output-hk/plutus
:icon-link-external:](https://github.com/input-output-hk/plutus)

---

!!!warning
Building the example code requires that you've done the [first time
setup](../first-time-setup.md) and the [weekly
setup](../weekly-setup.md) first.
!!!

With those steps out of the way, change directory back into the code for this
week

```bash
../plutus-pioneer-program/code/week01
```

Update Cabal

```bash
cabal update
```

Build the project with Cabal

```bash
cabal build
```
