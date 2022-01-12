# Install git

Some of the steps you must take to set up a Plutus environment require you to
have [git](https://git-scm.com/) installed.

## Arch Linux

On Arch Linux, I installed it using pacman.

```bash
sudo pacman -S git
```

Enter **y** and press **Enter** when asked if you want to continue with
installation.

## Ubuntu

If you use Ubuntu, git may already be installed. Try displaying its version.

```bash
git --version
```

If you receive an error that git is not installed, you'll need to install it.

Update your local package index.

```bash
sudo apt update
```

Install git

```bash
sudo apt install git
```
