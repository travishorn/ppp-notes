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

[Source Video
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

The last one is not used for this course, but the other two should be cloned to
your local machine with git. If you followed my guide for [running Plutus
Playground locally](../../appendix/run-plutus-playground-locally.md) then you'll
already have the plutus-apps repo cloned.

Change into the pioneer repository

```bash
cd plutus-pioneer-program
```

Pull the latest changes from remote

```
git pull
```

Change into the week 1 directory

```bash
cd ./code/week01
```

View the Cabal project file.

```bash
vim cabal.project
```

Look for the following lines

```
source-repository-package
  type: git
  location: https://github.com/input-output-hk/plutus-apps.git
  tag: [some long string of letters and numbers]
```

Copy the `tag` value (the long string of letters and numbers)

Change into the `plutus-apps` directory

```bash
cd ../../../plutus-apps
```

Your file structure may be different so you might have to tweak the command
above.

Pull the latest changes

```bash
git pull
```

Check out the code at the given tag. Replace `[tag]` below with the actual tag
you copied.

```bash
git checkout [tag]
```

While still in the `plutus-apps` repository, start a Nix shell

```bash
nix-shell
```

It might take a while the complete the first time

Change directory back into the code for this week

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
