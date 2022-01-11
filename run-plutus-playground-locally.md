# Run Plutus Playground Locally

The Plutus Playground is an application that lets you play with the [Plutus
Platform](./plutus-platform.md) and the [Cardano](./cardano.md) blockchain.

These instructions are for Linux. You'll need around 12GB of free hard disk
space.

Install Nix. Nix is a tool for reproducible builds and deployment.

```bash
curl -L https://nixos.org/nixinstall | sh
```

On my Arch Linux machine, Nix added a command to my `.bash_profile`. I've found
that this command is better placed in `.bashrc`.

Open `.bash_profile` and cut the line **that ends with**

```bash
# added by Nix installer
```

Paste it at the end of `.bashrc`

Save both files.

To make sure this command runs, start a new session. I recommend logging out and
back into your Linux user account.

Create a new directory to hold Nix configuration

```bash
sudo mkdir /etc/nix
```

Create a new file `/etc/nix/nix.conf` with the following content

```
substituters          = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/
trusted-public-keys   = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
experimental-features = nix-command
```

This will tell Nix to use the binary cache maintained by IOHK when building
their code. It can save hours of build time. It also tells Nix to enable the
`nix` command, as it is an experimental feature that is disabled by default.

Clone the Plutus Application Framework. This will create a new directory called
`plutus-apps` under your current working directory. You must have
[git](https://git-scm.com/) installed.

```bash
git clone https://github.com/input-output-hk/plutus-apps
```

Change into the newly cloned directory

```bash
cd plutus-apps
```

Build the packages and artifacts. This could take a few minutes.

```bash
nix build -f default.nix plutus-apps.haskell.packages.plutus-pab.components.library
```

Start the Nix Shell. This puts you in a reproducible environment defined by
IOHK. In order to do this, it needs to have all the tools and packages defined
by the Nix configuration. The first time you run this, it will take a long time
to download and build the environment.

It is important to run this command inside the `plutus-apps` directory every
time you want to work with the Plutus Application Framework.

```bash
nix-shell
```

Change into the playground server directory

```bash
cd plutus-playground-server
```

Start the playground server

```bash
plutus-playground-server
```

You'll know the server is ready when you see `Interpreter ready`.

Open a **new terminal** and change into the `plutus-apps` directory again

```bash
cd plutus-apps
```

Start another Nix Shell. It won't take as long this time.

```bash
nix-shell
```

Change into the playground client directory

```bash
cd plutus-playground-client
```

Start the playground client

```bash
npm start
```

You'll know the client is ready when you see `Compiled successfully` or
`Compiled with warnings`.

The playground server and client are now running. Access the web application by
opening a web browser and navigating to https://localhost:8009.

Since the playground runs over HTTPS and your local environment self-signs its
own certificate, you might receive warnings in your browser. You can safely
bypass them.
