# Dynamic vs Static Typing
Dynamic and static typing are programming paradigms.

## Dynamic (JavaScript)
Dynamically typed languages don't have defined data types.

```javascript
function add (x, y) {
	return x + y
}

add(1, 2)
// Returns 3

add(1, "hello")
// Returns "1hello"
```

The data types for the arguments `x` and `y` above are not defined. Although we may have designed this function to work on numbers only, it will also work on strings. This could produce unexpected behavior or errors at run-time.

## Static (Haskell)
Statically typed languages define the data types at compile-time and they cannot be changed during run-time.

```haskell
add :: Integer -> Integer -> Integer
add x y = x + y

add 1 2
-- Returns 3

add 1 "World"
-- Error
```

The `add` function accepts two `Integers` and returns one `Integer`. Once compiled, it always will. If you try to pass an argument that is not an integer, the program will not compile.

[Haskell](./Haskell.md) is statically typed.