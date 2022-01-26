---
label: First Time Setup
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -1
---

# First Time Setup

Before starting the Plutus Pioneer Program or developing on the Plutus platform
in general, you'll need to do some one-time setup.

These instructions are for Arch Linux. You'll need around 12GB of free hard disk
space. More is better, to be safe.

Install Nix. Nix is a tool for reproducible builds and deployment.

```bash
sh <(curl -L https://nixos.org/nix/install) --daemon
```

Follow any prompts during installation.

Open the Nix configuration as root

```bash
sudo vim /etc/nix/nix.conf
```

Add the following lines

```bash
substituters = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/
trusted-public-keys = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=
experimental-features = nix-command
```

The first two lines tell Nix to use the binary cache maintained by IOHK when
building their code. It can save hours of build time.

The last line tells Nix to enable the `nix` command, as it is an experimental
feature that is disabled by default.

Save and close the file

```
:wq
```

I don't think that Ubuntu machines have this problem, but on my Arch machine,
the Nix service had trouble starting because it couldn't find the daemon-socket
directory it was expecting.

If you have the same problem I did, make the daemon-socket directory manually

```bash
sudo mkdir -p /nix/var/nix/daemon-socket
```

Reload active sessions and restart the Nix service by rebooting the machine

```bash
sudo shutdown -r now
```

You'll need git installed for the next step. [Install it if you don't already
have it.](./appendix/install-git.md)

Clone the Plutus Pioneer Program repository to your local machine.

```bash
git clone https://github.com/input-output-hk/plutus-pioneer-program
```

Clone the Plutus Apps repository. This will create a new directory called
`plutus-apps` under your current working directory. 

```bash
git clone https://github.com/input-output-hk/plutus-apps
```

Change into the `plutus-apps` directory

```bash
cd plutus-apps
```

Build the Nix environment

```bash
nix build -f default.nix plutus-apps.haskell.packages.plutus-pab.components.library
```

It will take around 5-10 minutes. Make sure you see it downloading files from
hydra.iohk.io. If you don't, that means the binary cache was not set up
correctly. Go back and make sure you follow all the previous steps correctly.

Start a Nix Shell

```bash
nix-shell
```

Starting the Nix Shell puts you in a reproducible environment defined by IOHK.
In order to do this, it needs to have all the tools and packages defined by the
Nix configuration. The first time you run this, it will take a around 15-20
minutes to download and build the environment.

Now your development environment is set up and you're ready to procede with the
program. It is important to run this command inside the `plutus-apps` directory
every time you want to work with the Plutus Application Framework.
