---
label: PrettyPrinted Default EmulatorConfig
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -6
---

# PrettyPrinted Default EmulatorConfig

In a Plutus project REPL, you can view the default `EmulatorConfig` but it's not very easy to parse visually. Here's what it looks like prettyprinted.

```haskell
import Plutus.Trace.Emulator
import Data.Default

def :: EmulatorConfig
EmulatorConfig {
  _initialChainState = Left (
    fromList [
      (
        Wallet 1bc5f27d7b4e20083977418e839e429d00cc87f3,
        Value (Map [(,Map [("", 100000000)]))
      ),
      (
        Wallet 3a4778247ad35117d7c3150d194da389f3148f4a,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet 4e76ce6b3f12c6cc5a6a2545f6770d2bcb360648,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet 5f5a4f5f465580a5500b9a9cede7f4e014a37ea8,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet 7ce812d7a4770bbf58004067665c3a48f28ddd58,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet 872cb83b5ee40eb23bfdab1772660c822a48d491,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet bdf8dbca0cadeb365480c6ec29ec746a2b85274f,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet c19599f22890ced15c6a87222302109e83b78bdf,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet c30efb78b4e272685c1f9f0c93787fd4b6743154,
        Value (Map [(,Map [("",100000000)])])
      ),
      (
        Wallet d3eddd0d37989746b029a0e050386bc425363901,
        Value (Map [(,Map [("",100000000)])])
      )
    ]
  ),
  _slotConfig = SlotConfig {
    scSlotLength = 1000,
    scSlotZeroTime = POSIXTime {getPOSIXTime = 1596059091000}
  },
  _feeConfig = FeeConfig {
    fcConstantFee = Lovelace {getLovelace = 10},
    fcScriptsFeeFactor = 1.0
  }
}
```

As of 2022-02-03. [input-output-hk/plutus-apps commit
ea1bfc6a49ee731c67ada3bfb326ee798001701a
:icon-link-external:](https://github.com/input-output-hk/plutus-apps/tree/ea1bfc6a49ee731c67ada3bfb326ee798001701a)
