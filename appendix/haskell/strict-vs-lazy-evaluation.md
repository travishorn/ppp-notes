---
label: Strict vs Lazy Evaluation
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -4
---

# Strict vs Lazy Evaluation

Strict and lazy are two programming paradigms.

## Strict (JavaScript)

In strictly evaluated programming languages, expressions are evaluated as soon
as they are defined.

```javascript
let numbers = []
let i

for (i = 1; i < 9999; i = i + 1) {
	numbers.push(i)
}

numbers.slice(0, 5)
// Returns [1,2,3,4,5]
```

The code above fills an array with 9999 numbers. Even though we ultimately only
ask for the first 5.

## Lazy (Haskell)

In lazily evaluated programming languages, expressions are only evaluated when
they're needed.

```haskell
numbers = [1..9999]
take 5 numbers
-- Returns [1,2,3,4,5]
```

Even though the list of numbers starts at 1 and goes up to 9999, the computer
doesn't actually need to calculate up to 9999. When we ask for the first 5, it
calculates and returns those 5 only.

In fact, we don't have to stop at 9999. We could tell Haskell that `numbers`
represents all integers from 1 to infinity.

```haskell
numbers = [1..]
take 5 numbers
-- Returns [1,2,3,4,5]
```

[Haskell](./haskell.md) uses lazy evaluation.
