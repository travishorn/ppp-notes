---
label: Introduction
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -1
---

# Emulator Trace & Contract Monads

## Introduction

Lecture 4, Part 1

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=gxMW9uXTEj4&list=PLNEK_Ejlx3x230-g-U02issX5BiWAgmSi&index=2)

The lectures so far have dealt with on-chain validation. This type of validation
is important, but in order for anything to be validated on-chain, we need to
actually build transactions. That's where off-chain code comes in.

All of the off-chain code is written in a special monad called the Contract
Monad.
