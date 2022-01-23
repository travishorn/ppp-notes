---
label: Enable Overloaded Strings
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -3
---

# Enable Overloaded Strings

In Haskell, literalstrings such as `"Haskell"` are of type `String` which is a
synonym for `[Char]` (a list of chars).

```haskell
:t "Haskell"
"Haskell" :: [Char]
```

Enabling the `XOverloadedStrings` language extension allows you to use literal
strings for other string-like types, as well.

## In a .hs file

```haskell
{-# LANGUAGE OverloadedStrings #-}
```

## In GHCi

```haskell
:set -XOverloadedStrings
```

---

Now that overloaded strings are enabled, you can use literal strings for other
string-like types.

```haskell
:t "Haskell"
"Haskell" :: Data.String.IsString p => p
```
