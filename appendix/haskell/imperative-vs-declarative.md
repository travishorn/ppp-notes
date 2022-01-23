---
label: Imperative vs Declarative
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -1
---

# Imperative vs Declarative

Imperative and declarative are two programming paradigms. A paradigm is a way to
classify programming languages based on their features.

## Imperative (JavaScript)

Imperative programming consists of commands for the computer to perform. They
tell the computer *how* to change its state.

```javascript
function sum (array) {
	let i
	let sum = 0
	
	for (i = 0; i < array.length; i = i + 1) {
		sum = sum + array[i]
	}
	
	return sum
}

sum ([1,2,3,4,5])
// Returns 15
```

The code above does not give a definition of what a sum actually is. Instead it
is describing an algorithm that *can* produce a sum.

## Declarative (Haskell)

Declarative programming expresses the logic without describing its control flow.
It tells the computer *what* it must accomplish rather than describing *how* to
do it.

```haskell
sum [] = 0
sum (x:xs) = x + sum xs

sum [1,2,3,4,5]
-- Returns 15
```

[Haskell](./haskell.md) is a declarative programming language.
