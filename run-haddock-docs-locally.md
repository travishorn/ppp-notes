# Run Haddock Docs Locally

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
