# Impure vs Pure Functions
In programming languages, functions can either be impure or pure.

## Impure (JavaScript)
Impure functions may read or write from external sources when called.

```javascript
let x = 0

function addOne () {
	x = x + 1
}

addOne()
// Now x is 1

addOne()
// Now x is 2

addOne()
// Now x is 3
```

Each time `addOne` is called, it affects the variable `x` which is outside itself. You cannot predict its effects without knowing additional state of the program.

## Pure (Haskell)
Pure functions do not read or write from external sources. Every time you call a pure function with the same arguments, it will return the same result. Any state you want to act on must be passed into it as an argument.

```haskell
addOne x = x + 1

addOne 0
-- Always returns 1

addOne 1
-- Always returns 2

addOne 2
-- Always returns 3
```

[Haskell](./haskell.md) is purely functional.