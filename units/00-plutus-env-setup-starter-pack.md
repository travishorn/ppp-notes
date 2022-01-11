# Plutus Env: Setup Starter Pack

Before the first lecture began, IOG notified us of the following.

> The education team collected a Starter Pack for setting up Plutus development
> environments, made up of resources maintained by IO, CF, and the Plutus
> community respectively

> Because Plutus is a new language, this task is a learning exercise in its own
> right, and we expect it to take at least the first week for everyone to get up
> to speed. We encourage you to get a head start now, and assist other Pioneers
> as you go. Some of you have already begun

They linked us the [Plutus Env: Setup Starter
Pack](https://docs.google.com/document/d/13112LHG9vVvNUs40oZSqZ-DF6_yFiT_SJZ2NaEmjMM4/edit?usp=sharing).

In that document, it describes various ways to get started. It mentions that the
easiest way to get started is by using the [online
playground](https://playground.plutus.iohkdev.io/). It says that the online
playground is kept up-to-date.

However, none of us in the 3rd cohort were able to get the online playground to
work. It seems that [something broke a while
back](https://github.com/input-output-hk/plutus-apps/issues/195) and it has not
been fixed for a while.

So the only option left is to set up a local Plutus development environment. For
that, IOG provided a few resources. I'll talk about them briefly here.

## [Plutus Community Documentation](https://docs.plutus-community.com/)

At first glance, this resource looks great. However, the instructions are for
the second cohort and much has changed since then.

## [Plutus | Cardano Developer Portal](https://developers.cardano.org/docs/smart-contracts/plutus/)

A great resource, but is too high level and doesn't give any information about
setting up a local environment.

## [The Plutus Apps Repository](https://github.com/input-output-hk/plutus-apps)

This is the canonical code repository and what the actual development team at
IOG uses. The instructions can be a little confusing. They assume you already
have some familiarity with Nix.

## [The Plutus Starter Repository](https://github.com/input-output-hk/plutus-starter)

Could be helpful if you're planning on using Docker and VS Code. The general
consensus from other pioneers was to go with one of the other options, though.

---

Ultimately, I used the instructions in **Plutus Apps Repository**. The process
was not as straightforward as the instructions make it seem. I ran into some
pitfalls.

I wrote a document detailing all of the steps I did to [run plutus playground
locally](../run-plutus-playground-locally.md).
