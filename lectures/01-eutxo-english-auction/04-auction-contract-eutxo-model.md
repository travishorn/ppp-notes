# EUTxO and English Auction

Lecture 1, Part 4

## Auction Contract in the EUTxO Model

[Source
Video](https://www.youtube.com/watch?v=Bj6bqRGT1L0&list=PLNEK_Ejlx3x2nLM4fAck2JS6KhFQlXq2N&index=4)

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

**Redeemer:** bid

**Input 1:** The auction EUTxO

**Input 2:** A EUTxO representing his bid. The value is 100 ADA

**Output:** The auction EUTxO again, but with the value and datum changed. The
value is "NFT + 100 ADA" and the datum is (Bob, 100).

The script checks that all conditions are met. The bid is above the minimum and
the time is before the deadline. It passes.

Now, Charlie wants to submit a bid for 200 ADA. He creates a transaction with 2
inputs and 2 outputs.

**Redeemer:** bid

**Input 1:** The auction EUTxO (includes changes from Bob)

**Input 2:** A EUTxO representing his bid. The value is 200 ADA

**Output 1:** The auction EUTxO again, but with the value and datum changed. The
value is "NFT + 200 ADA" and the datum is (Charlie, 200).

**Output 2:** 100 ADA to Bob. He is getting his bid back. The information can be
found by looking at the datum on the input auction

Again, the script checks that all conditions are met. In addition to the bid
minimum and the deadline, it also checks that the new bid is higher than the
current highest bid, the new auction EUTxO is correctly updated, and that the
previous highest bidder gets his bid back.

Once the deadline has passed, someone has to initiate one more transaction of
the script. It can be anybody. But it will probably be Alice who wants her bid
or the winner, Charlie, who wants his NFT.

This final transaction will have 1 input and 2 outputs.

**Redeemer:** close

**Input:** The auction EUTxO

**Output 1:** NFT to Charlie

**Output 2:** 200 ADA to Alice

The script will check conditions. Conditions are: deadline is reached and
highest bidder gets NFT and auction owner gets highest bid.

What if no one bids? Alice makes a transaction.

**Redeemer:** close

**Input:** The auction EUTxO

**Output:** NFT to Alice

The script checks and sees there is no highest bidder and outputs the NFT to
Alice.

## The Auction Code Itself

On Cardano, there are on-chain and off-chain code. Everything above is on-chain
code.

There are public key addresses and script addresses. They sit on an output. If a
transaction tries to consume a EUTxO on the script address (tries to use the
script address as an input), the script executes on-chain by a node.

Look at [plutus-pioneer-program/code/week01/src/Week01/EnglishAuction.hs](https://github.com/input-output-hk/plutus-pioneer-program/blob/ecafd204b56defceed9fd3a69ebede09256c12c0/code/week01/src/Week01/EnglishAuction.hs)

On line 56, you can see the data definition for the `Auction`

```haskell
data Auction = Auction
  { aSeller   :: !PaymentPubKeyHash
  , aDeadline :: !POSIXTime
  , aMinBid   :: !Integer
  , aCurrency :: !CurrencySymbol
  , aToken    :: !TokenName
  } deriving (P.Show, Generic, ToJSON, FromJSON, ToSchema)
```

It contains information about the seller, deadline, minimum bid, and (the last
two keys) the NFT.

On line 114, you can see the start of the definition of the heart of the script.
This is the actual logic used in the script.

```haskell
mkAuctionValidator :: AuctionDatum -> AuctionAction -> ScriptContext -> Bool
mkAuctionValidator ad redeemer ctx =
  traceIfFalse "wrong input value" correctInputValue &&
  --- Many more lines below here snipped
```

On line 216, you can see where the compilation happens. This is "template
Haskell" that initiates the GHC compiler plugin.

```haskell
typedAuctionValidator :: Scripts.TypedValidator Auctioning
typedActuionValidator = Scripts.mkTypedValidator @Auctioning
    $$(PlutusTx.compile [|| mkAuctionValidator ||])
    $$(PlutusTx.compile [|| wrap ||])
  where
    wrap = Scripts.wrapValidator @AuctionDatum @AuctionAction
```

On line 232 starts the off-chain part of the code. This particular block defines
parameters for enpoints that are involved.

```haskell
data StartParams = StartParams
  { spDeadline :: !POSIXTime
  , spMinBid   :: !Integer
  , spCurrency :: !CurrencySymbold
  , spToken    :: !TokenName
  } deriving (Generic, ToJSON, FromJSON, ToSchema)

-- Two more endpoints snipped: BidParams and CloseParams
```

The logic for `start`, `bid`, and `close` as well as a few more helper functions
are defined a little lower starting on line 255.

The last few lines starting on line 362 are for demo purposes and trying out the
auction in the playground. It's creating a sample NFT we can use to auction
away.

```haskell
myToken :: KnownCurrency
myToken = KnownCurrency (ValidatorHash "f") "Token" (TokenName "T" :| [])

mkKnownCurrencies ['myToken]
```

The token created here has the hash "f". Looking up the UTF-8 table, you can see
that "f" can also be represented as decimal 102, which is hexidecimal 66.

| Name                    | Printable Character | Decimal | Hexidecimal |
|:------------------------|:--------------------|--------:|------------:|
| Latin small character F | f                   |     102 |          66 |

That value "66" will be necessary to know when using the playground. It needs to
be entered as the `spCurrency unCurrencySymbol` when interacting with the
contract.

---

Some code can be shared between the on-chain and off-chain parts. The `minBid`
function is one such example you can see in the example auction code.

[NEXT: Part 5: Auction Contract on the
Playground](./05-auction-contract-playground.md)
