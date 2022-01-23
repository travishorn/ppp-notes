---
label: Run Haddock Docs Locally
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -5
---

# Run Haddock Docs Locally

Technical Haskell documentation for all the functions and types in Plutus can be
built with Haddock. You can view the built docs in your web browser.

First, follow all the steps to build the Nix environment found in the guide for
[running Plutus Playground locally](./run-plutus-playground-locally.md). This is
a one-time setup step. Stop once you get to the step where you start
`nix-shell`.

Change into the `plutus-apps` directory

```
cd plutus-apps
```

Start a Nix shell

```
nix-shell
```

Build and serve the docs

```
build-and-serve-docs
```

View the docs at http://0.0.0.0:8002/haddock
