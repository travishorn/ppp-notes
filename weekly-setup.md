---
label: Weekly Setup
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Weekly Setup

Each week, the [Plutus Pioneer Program
repository](https://github.com/input-output-hk/plutus-pioneer-program) will be
updated with the code used in that week's lecture.

You'll need to follow the steps below to get your development environment set up
for the week. Make sure you've already done the [first time
setup](./first-time-setup.md) first.

Change into the pioneer repository

```bash
cd plutus-pioneer-program
```

Pull the latest changes from remote

```
git pull
```

Change into the directory for the current week

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

Start as many Nix shells as necessary for development. You might need one each
for:

- starting the playground server
- starting the playground client
- building and serving Haddock docs
- starting a cabal REPL inside the appropriate week project
- building the cabal project
