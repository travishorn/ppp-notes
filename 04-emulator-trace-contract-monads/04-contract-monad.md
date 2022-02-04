---
label: The Contract Monad
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -4
---

# Emulator Trace & Contract Monads

## The Contract Monad

Lecture 4, Part 4

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=yKX5Ce8Y0VQ&list=PLNEK_Ejlx3x230-g-U02issX5BiWAgmSi&index=4)

The `Contract` monad takes 4 arguments: `w s e a`. As always in Haskell, the last argument is the return value. So `a` is the return value.

`w` is like the Writer we worked on earlier in this lecture. It allows you to log messages of type `w`. The real purpose of logging messages is to communicate between two contracts. The written `w` is visible to other contracts.

`s` stands for "schema" and it specifies the endpoints that you can invoke

`e` specifies the type of error message that you can throw and catch inside the contract

[Most of the notes for this section are comments in `Contract.hs`
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Contract.hs)
