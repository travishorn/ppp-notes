---
label: Auction Contract on the Playground
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -5
---

# EUTxO and English Auction

## Auction Contract on the Playground

Lecture 1, Part 5

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=K61Si6iQ-Js&list=PLNEK_Ejlx3x2nLM4fAck2JS6KhFQlXq2N&index=5)

### Start Plutus Playground Locally

Follow the instructions to [run Plutus Playground
locally](../appendix/run-plutus-playground-locally.md).

Open your web browser to [https://localhost:8009](https://localhost:8009) to see
the playground.

### Run the English Auction

Copy the contents of
`plutus-pioneer-program/code/week01/src/Week01/EnglishAuction.hs`

Paste it into the editor of the playground in your browser

Remove the module header. You'll have to do this for every script you run in the
playground. Delete lines 18-30 (should start with `module` and end with
`where`).

Click **Compile**.

Once you see `Compilation successful` below the editor, the script is compiled.

Click **Simulate**.

By default there are two wallets. Both have 100,000,000 Lovelace (100 ADA) and
100,000,000 T. T is the token for the NFT.

Add another wallet by clicking **Add Wallet**.

There should be only 1 NFT, so make the **T** box under **Wallet 1** say `1`.
Make the **T** box under both **Wallet 2** and **Wallet 3** say `0`.

Have **Wallet 1** start the auction by clicking **start +**.

Add a wait action by clicking **Add Wait Action** under **Actions**.

Have **Wallet 2** make a bid by clicking **bid +**.

Add another wait action.

Have **Wallet 3** make a bid.

Add another wait action.

Have **Wallet 1** close the auction by clicking **close +**.

Finally, add another wait action.

---

With all of the actions added, we have to define the parameters.

Under **Actions** edit the **Wallet 1: start** action.

In the simulator, time is based on "slots" but time in Plutus is based on real
time. So for **spDeadline**, we need to put the right value that corresponds to
10 slots.

See the guide on [Converting Slots to POSIX
Time](../appendix/convert-slots-posix-time.md)

10 slots in Plutus Playground equates to `1596059101999` POSIX time. Enter that
value in the **spDeadline** box.

Under **spMinBid**, enter `10000000` to make the minimum bid 10 ADA (10,000,000
Lovelace).

Under **spCurrency**, enter `66`. This value is defined in the auction script
(although there is some conversion that happens so you won't exactly see `66`
anywhere in the code).

Under **spToken**, enter `T`.

Right next to this action, have the wait action that follows it **Wait For...**
`1` slot.

---

For the **Wallet 2: bid** action...

Match the **bpCurrency** with the **spCurrency** when the auction was started by
entering `66`.

Match the **bpToken** with **spToken** by entering `T`.

Enter a **bpBid** of 10000000 (10 ADA)

Have the following wait action **Wait For...** `1` slot again.

---

For the **Wallet 3: bid** action, enter the same **bpCurrency** and **bpToken**,
but have this action bid higher by entering `15000000`.

Have the following wait action **Wait Until...** Slot `11` this time to make
sure we pass the deadline.

---

For the **Wallet 1: close** action, match the **bpCurrency** and **bpToken**
again.

Finally, make the last wait action **Wait For...** `1` slot so the transaction
has time to be processed.

---

Click **Evaluate** to evaluate this simulation. You'll see the result after a
few moments.

For each slot, you can see the transactions and the outputs.

During slot 0, the outputs look like this:

- Wallet 1 gets 100 ADA and 1 `T`
- Wallet 2 gets 100 ADA
- Wallet 3 gets 100 ADA

Click on **Slot 1** to view it.

During this slot, there is 1 input which is wallet 1. Everything from wallet 1
gets spent in this transaction.

The ouputs are:

- Fee of 10 Lovelace (Only in the playground. Real world will be higher)
- Wallet 1 gets ~97 ADA
- Script gets 1 T and 2 ADA (In Cardano, every transaction must have some ADA in
  it.)

Keep following the slots to see how transactions behaved during the simulation.
