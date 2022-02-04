---
label: Run Plutus Playground Locally
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -8
---

# Run Plutus Playground Locally

The Plutus Playground is an application that lets you play with the [Plutus
Platform](./about-platform/plutus-platform.md) and the
[Cardano](./about-platform/cardano.md) blockchain.

Before following the steps below, make sure to complete the [first time
setup](../first-time-setup.md).

Change into the `plutus-apps` directory

```bash
cd plutus-apps
```

Start a Nix shell

```bash
nix-shell
```

Change into the `plutus-playground-client` directory

```bash
cd plutus-playground-client
```

Run the Playground Server

```bash
plutus-playground-server
```

It will take a few minutes. You'll know it's ready when you see "Interpreter
ready"

Open up a new terminal and change into the `plutus-apps` directory again

```bash
cd plutus-apps
```

Start another Nix Shell

```bash
nix-shell
```

It shouldn't take as long this time. Probably less than 5 minutes.

Change into the `plutus-playground-client` directory

```bash
cd plutus-playground-client
```

Run the Playground Client

```bash
npm run start
```

It will take around 5-10 minutes. You'll know it's ready when you see "Compiled
successfully" or "Compiled with warnings".

The playground server and client are now running. Access the web application by
opening a web browser and navigating to https://localhost:8009

Since the playground runs over HTTPS and your local environment self-signs its
own certificate, you might receive warnings in your browser. You can safely
bypass them.

You should then see the Plutus Playround in your browser.
