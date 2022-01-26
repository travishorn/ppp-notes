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

First, make sure you've done the [first time setup](../first-time-setup.md).

Then change into the `plutus-apps` directory

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
