---
label: Triggering Change
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -1
---

# Validation Scripts

## Triggering Change

Lecture 2, Part 1

[Source Video
:icon-link-external:](https://www.youtube.com/watch?v=BEr7lcCPjnA&list=PLNEK_Ejlx3x0mhPmOjPSHZPtTFpfJo3Nd&index=2)

Endpoints are labelled as "available functions" in the Plutus Playground. In the
Auction Example, there are 3 endpoints:

- bid
- close
- start

In the close endpoint, there are two scenarios:

- There **is** a highest bidder. In that case the token goes to that bidder.
- There **is not** a highest bidder. In that case the token goes back to the
  auction starter.

If the close endpoint didn't exist, the ADA and token contained in the contract
would be locked there forever. Contracts on the blockchain are just data.
Absolutely passive. In order for anything to happen there must be a new
transaction submitted from somebody that consumed UTxOs and produces new ones. A
contract will never spring into action on its own.

You *could* write some logic for a wallet that automatically sleeps for a given
amount of time until the deadline and then invokes the close endpoint.
