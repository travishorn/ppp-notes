# EUTxO and English Auction

Lecture 1, Part 4

## Auction Contract in the EUTxO Model

Introductory example is an auction. Somebody wants to auction off an NFT.

Auction is parameterized by the owner of the token. They provide the token
itself, a minimum bid, and deadline.

Alice has an NFT and she wants to auction it. She creates a EUTxO.

- The value (were it might normally be 10 ADA or 100 ADA, etc) is just the NFT
- The datum is initially "Nothing". Later it will contain the highest bidder and
  the highest bid

In the real blockchain, all EUTxOs much be accompanied by some amount of ADA,
but we are ignoring it for simplicity.

Bob wants to bid 100 ADA. He creates a transaction with 2 inputs and 1 output.
The first input is the auction EUTxO and the second input is a EUTxO
representing his bid. It contains 100 ADA. The output is again the auction
script, but with the value and the datum changed.

**Input 1:** The auction EUTxO

**Input 2:** A EUTxO representing his bid. The value is 100 ADA

**Output:** The auction EUTxO again, but with the value and datum changed. The
value is "NFT + 100 ADA" and the datum is (Bob, 100).

The script checks that all conditions are met. The bid is above the minimum and
the time is before the deadline. It passes.

Now, Charlie wants to submit a bid for 200 ADA. He creates a transaction with 2
inputs and 2 outputs.

**Input 1:** The auction EUTxO (includes changes from Bob)

**Input 2:** A EUTxO representing his bid. The value is 200 ADA

**Output 1:** The auction EUTxO again, but with the value and datum changed. The
value is "NFT + 200 ADA" and the datum is (Charlie, 200).

**Output 2:** 100 ADA to Bob. He is getting his bid back. The information can be
found by looking at the datum on the input auction
