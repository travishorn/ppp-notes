# Run Plutus Playground Locally

The Plutus Playground is an application that lets you play with the [Plutus
Platform](./plutus-platform.md) and the [Cardano](./cardano.md) blockchain.

These instructions are for Linux. You'll need around 12GB of free hard disk
space. More is better, to be safe.

Install Nix. Nix is a tool for reproducible builds and deployment.

```bash
curl -L https://nixos.org/nixinstall | sh
```

I haven't seen anyone else talk about this so maybe it's just me. But on my Arch
Linux machine, Nix added a line to my `.bash_profile`. However, I had some
issues with that line and I found that is better placed in `.bashrc`.
Personally, I cut the line from `.bash_profile` and pasted it in `.bashrc`. Your
experience may be different.

To make sure Nix is ready, start a new session. I recommend logging out and back
into your Linux user account. You could also restart the whole machine, but that
might be overkill.

Create a new directory to hold Nix configuration

```bash
sudo mkdir /etc/nix
```

Create a new configuration file. (I'm using vim here, but use whatever editor
you like)

```bash
sudo vim /etc/nix/nix.conf
```

Add the following content to the file.

```
substituters          = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/
trusted-public-keys   = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
experimental-features = nix-command
```

The first two lines tell Nix to use the binary cache maintained by IOHK when building
their code. It can save hours of build time.

The last line tells Nix to enable the `nix` command, as it is an experimental
feature that is disabled by default.

You'll need git installed for the next step. [Install it if you don't already
have it.](./install-git.md)

Clone the Plutus Application Framework. This will create a new directory called
`plutus-apps` under your current working directory. 

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

You'll know the client is ready when you see `Compiled successfully` or even
`Compiled with warnings`.

The playground server and client are now running. Access the web application by
opening a web browser and navigating to https://localhost:8009

Since the playground runs over HTTPS and your local environment self-signs its
own certificate, you might receive warnings in your browser. You can safely
bypass them.
