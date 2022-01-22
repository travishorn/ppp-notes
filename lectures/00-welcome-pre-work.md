---
label: Welcome & Pre-work
author:
  name: Travis Horn
  email: travis@travishorn.com
order: 0
---

# Welcome & Pre-work

Before the first lecture began, Matthew from IOG notified us of the following.

> The education team collected a Starter Pack for setting up Plutus development
> environments, made up of resources maintained by IO, CF, and the Plutus
> community respectively

> Because Plutus is a new language, this task is a learning exercise in its own
> right, and we expect it to take at least the first week for everyone to get up
> to speed. We encourage you to get a head start now, and assist other Pioneers
> as you go. Some of you have already begun

## Plutus Env: Setup Starter Pack

Matthew linked us the [Plutus Env: Setup Starter
Pack](https://docs.google.com/document/d/13112LHG9vVvNUs40oZSqZ-DF6_yFiT_SJZ2NaEmjMM4/edit?usp=sharing).

In that document, it describes various ways to get started. It mentions that the
easiest way to get started is by using the [online
playground](https://playground.plutus.iohkdev.io/). It says that the online
playground is kept up-to-date.

However, none of us in the 3rd cohort were able to get the online playground to
work. It seems that [something broke a while
back](https://github.com/input-output-hk/plutus-apps/issues/195) and it has not
been fixed for a while.

Shortly after, a new announcement was made stating that the online playground
was broken with no timeline on a fix.

So the only option left is to set up a local Plutus development environment. For
that, IOG provided many resources:

- A short "installation guide" directly in the "starter pack" Google doc
- [Plutus Community Documentation](https://docs.plutus-community.com/)
- [Plutus | Cardano Developer Portal](https://developers.cardano.org/docs/smart-contracts/plutus/)
- [The Plutus Apps Repository](https://github.com/input-output-hk/plutus-apps)
- [The Plutus Starter Repository](https://github.com/input-output-hk/plutus-starter)

Much of the resources have issues for us in the 3rd cohort. Instructions are
usually for old lectures and reference out-of-date code repositories and
commands. Some of the resources are too high level and don't give any
information about specifically setting up a local environment.

Ultimately, I used the instructions in **Plutus Apps Repository**. The process
was not as straightforward as the instructions make it seem. I ran into some
pitfalls. However, this is the canonical code repository and what the actual
development team at IOG uses.

I wrote a document detailing all of the steps I did to [run plutus playground
locally](../../appendix/run-plutus-playground-locally.md) on my Linux machine.
