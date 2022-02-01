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
for the week.

!!!
Make sure you've already done the [first time
setup](./first-time-setup.md) first.
!!!

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
cd ~/plutus-apps
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

While still in the `plutus-apps` repository, start a Nix shell. This will take a
while the first time you run it. It won't take as long in subsequent weeks.

```bash
nix-shell
```

Start as many Nix shells as necessary for development; each time starting it
from `plutus-apps`. Some things you might need a Nix shell for include:

- [starting the playground server](./appendix/run-plutus-playground-locally.md)
- [starting the playground client](./appendix/run-plutus-playground-locally.md)
- [building and serving Haddock docs](./appendix/run-haddock-docs-locally.md)
- [building a cabal
  project](./01-eutxo-english-auction/03-building-example-code.md)
- [starting a cabal REPL inside the appropriate week
  project](./01-eutxo-english-auction/03-building-example-code.md#starting-a-repl)
