---
label: Monads
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -2
---

# Emulator Trace & Contract Monads

## Monads

Lecture 4, Part 2

[Lecture Video
:icon-link-external:](https://www.youtube.com/watch?v=f2w-MB3X4a0&list=PLNEK_Ejlx3x230-g-U02issX5BiWAgmSi&index=2)

This part is not necessarily specific to Plutus. This is an overview of how
monads work in a functional programming language like Haskell.

### Pure Functions

To understand how monads work, we'll compare to Java. Imagine this method.

```java
public static int foo() {
  // Some code. Maybe it asks the user to input a number on the console and then
  // returns it.
}

foo()
foo()
```

It doesn't take any argument, and returns an int. It's entirely possible that
the two calls to `foo()` return a different int. The function could be doing
some IO which changes what int will be returned. These IO operations are called
side-effects.

Haskell is a [purely functional
language](../appendix/haskell/impure-vs-pure-functions.md). This situation
cannot happen in Haskell. Consider this function.

```haskell
foo :: Int
foo = -- Some code

foo
foo
```

`foo` will always return the same number. We don't need to know what the code
inside it does to know that. If we give a function the same arguments, it will always return the same result. Side-effects cannot happen.

### The IO Monad

Without side-effects, your program can't really *do* anything. It can't affect
the world. Side-effects like input into the program and output like printing a
result to console are necessary to have a useful program.

Haskell has a way to do side-effects called the IO monad.

```haskell
foo :: IO Int
foo = -- Some code
```

The `IO Int` type represents an action to compute an int which can invoke
side-effects. You can think of it like a recipe. It's a list of instructions for
what a computer should do to end up with an int as a result.

`foo` is actually still pure here. When `foo` is evaluated, the "recipe" won't
actually be executed. The result of evaluating `foo` is just the recipe to
produce the int.

The only way to actually execute the recipe is in the main function. In the
executable's main entry point. Note that the REPL (GHCi) also allows you to
execute IO actions.

### Hello World in Haskell

```haskell
main :: IO ()
main = putStrLn "Hello, World!"
```

The function name must be `main` and the type must be `IO ()`. That type is
again a recipe that can do some side-effects and return unit `()` which means it
returns nothing.

`putStrLn` is a function that takes a string and returns `IO ()`. That return
type matches what `main` is expecting.

```haskell
:t putStrLn
putStrLn :: String -> IO ()
```

You could execute the hello world program in the REPL by typing it directly.

```haskell
putStrLn "Hello, World!"
"Hello, World!"
```

### getLine

There are many IO actions in Haskell. Here's one example. There's a function
called `getLine`. It waits for user input from the keyboard.

```haskell
:t getLine
getLine :: IO String

getLine --Haskell is now waiting for user input on the console.
Hello
"Hello"
```

You can combine these primitive IO actions into bigger, more complex actions to
make your program useful.

### Functors

View information about `IO`. It has a Functor instance.

```haskell
:i IO
type IO :: * -> *
newtype IO a
  = ghc-prim-0.6.1:GHC.Types.IO (ghc-prim-0.6.1:GHC.Prim.State#
                                   ghc-prim-0.6.1:GHC.Prim.RealWorld
                                 -> (# ghc-prim-0.6.1:GHC.Prim.State#
                                         ghc-prim-0.6.1:GHC.Prim.RealWorld,
                                       a #))
        -- Defined in ‘ghc-prim-0.6.1:GHC.Types’
instance Applicative IO -- Defined in ‘GHC.Base’
instance Functor IO -- Defined in ‘GHC.Base’
instance Monad IO -- Defined in ‘GHC.Base’
instance Monoid a => Monoid (IO a) -- Defined in ‘GHC.Base’
instance Semigroup a => Semigroup (IO a) -- Defined in ‘GHC.Base’
instance MonadFail IO -- Defined in ‘Control.Monad.Fail’
```

`Functor` is an important type in Haskell.

```haskell
:t fmap
fmap :: Functor f => (a -> b) -> f a -> f b
```

We are particularly interested when `f` is `IO`.

### Functor Example with toUpper

`toUpper` works on `Char`s.

```haskell
:t toUpper
toUpper :: Char -> Char

toUpper 'q'
'Q'
```

Since a `String` is just a list of `Char`s, we can `map` over a `String` with `toUpper`.

```haskell
:t map toUpper
map toUpper :: [Char] -> [Char]

map toUpper "haskell"
"HASKELL"
```

Note how `map toUpper` has a good type signature to use as the first argument to
`fmap`.

Remember that `getLine`'s type is `IO String`. That matches the second argument
to `fmap`.

Finally, we can use `fmap` with these arguments.

```haskell
:t fmap (map toUpper) getLine
fmap (map toUpper) getLine :: IO [Char]

fmap (map toUpper) getLine
hello world
"HELLO WORLD"
```

We were able to map `toUpper` over the input given by `getLine`.

### >>

You can perform IO actions in sequence with `>>`

```haskell
putStrLn "Hello" >> putStrLn "World"
Hello
World
```

If the first action has a return result, it will be ignored.

### >>= (Bind)

If you need to use the result of the first action, you can use bind `>>=`. The type signature must match this:

```haskell
:t (>>=)
(>>=) :: Monad m => m a -> (a -> m b) -> m b
```

It uses any monad, but we are interested in `IO` so imagine `m` above is `IO`.

Given the first argument that has side-effects `a`: `IO a`

And given the second argument that is a function takes an `a` and returns `IO
b`: `(a -> m b)`

`>>=` would give us `IO b`.

### Bind Example with getLine

```haskell
-- Look at the type signature
:t (>>=)
(>>=) :: Monad m => m a -> (a -> m b) -> m b

-- getLine matches the first argument
:t getLine
getLine :: IO String

-- putStrLn matches the second argument
:t putStrLn
putStrLn :: String -> IO ()

-- Bind them
getLine >>= putStrLn
Hello World
Hello World
```

### Another Bind Example

```haskell
-- In a file called src/Week04/Notes.hs
module Notes where

bar :: IO ()
bar = getLine >>= \s ->
      getLine >>= \t ->
      putStrLn (s ++ t)


-- In the REPL
:l src/Week04/Notes.hs
bar
Hello
World
HelloWorld
```

### Return

```haskell
:t return
return :: Monad m => a -> m a
```

Again, it can be used with any monad, but we're interested in `IO` so think `IO`
when you see `m` above.

`return` takes an argument `a` and returns `IO a`.

```haskell
return "Hello World" :: IO String
"Hello World"
```

### The Maybe Type

We're done working with `IO` for right now. Now we focus on `Maybe`.

```haskell
:i Maybe
type Maybe :: * -> *
data Maybe a = Nothing | Just a
-- more info
```

It has two constructors `Nothing` or `Just a`. It can take either `Nothing` or
`a` with a `Just` constructor.

### readMaybe

You can read a `String` as an `Int` with `read`

```haskell
read "42" :: Int
42
```

But you get an error when you try to `read` something that cannot be parsed as
an `Int`.

```haskell
read "abc" :: Int
*** Exception: Prelude.read: no parse
```

It's better to use `readMaybe`

```haskell
import Text.Read (readMaybe)

readMaybe "42" :: Maybe Int
Just 42

readMaybe "abc" :: Maybe Int
Nothing
```

Instead of a direct `Int` or an error, we now get `Just` with the `Integer` or
`Nothing`.

### Maybe.hs

Most of my notes are comments in the file.

[My commented code
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Maybe.hs)

### Either.hs

```haskell
:i Either
type Either :: * -> * -> *
data Either a b = Left a | Right b
-- more info
```

The two constructors are `Left` and `Right`. `Either` will be one of those.

```haskell
Left "Hello World" :: Either String Int
Left "Haskell"

Right 42 :: Either String Int
Right 42
```

The problem with `Maybe` is that when there's an error, only `Nothing` is returned. There's no place for an error message. We can use `Either` to solve that problem.

The convention is the `Left` corresponds to an error while `Right` corresponds to the expected value.

I wrote more notes as comments in the file.

[My commented code
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Either.hs)

### Writer.hs

Here's another example. This one can produce computations that also output logs.

Most of my notes are comments in the file.

[My commented code
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Writer.hs)

### Monad.hs

With all of the above explained, we can get a better understanding of what a
monad is. It's a combination of a computation and real-world side-effects.

Whenever there's a possibility of a computation with side-effects, we also have
the ability to do the computation without side-effects.

Computations like...

- `>>=`
- `Maybe`
- `Either`
- `Writer`

have counterparts that return the computation without side-effects. These are...

- `return`
- `Just`
- `Right`
- `(\a -> Writer a [])`

The definition of a monad is the combination of these two features.

- the ability to bind two computations together
- the ability to construct a computation from a pure value without making use of
  the potential side-effects

Some more notes of my notes are comments in the file.

[My commented code
:icon-link-external:](https://github.com/travishorn/plutus-pioneer-program/blob/main/code/week04/src/Week04/Monad.hs)

### Applicative

Monad is actually a subclass of Applicative.

```haskell
:i Applicative
type Applicative :: (* -> *) -> Constraint
class Functor f => Applicative f where
  pure :: a -> f a
  (<*>) :: f (a -> b) -> f a -> f b
-- more info
```

Notice that `pure` has the same signature as Monad's `return`.

The standard way to implement Functor and Applicative is to import helper functions from `Control.Monad`

```haskell
instance Functor Writer where
    fmap = liftM

instance Applicative Writer where
    pure = return
    (<*>) = ap

instance Monad Writer where
    return a = Writer a []
    (>>=) = bindWriter
```

### Superpowers

Another way to think about monads is that they are a way to do a computation withe a side-effect or a superpower.

IO's superpower is having real-world side-effects like...

- keyboard or file input
- console output or writing to a file

Maybe's superpower is the ability to fail without throwing an exception.

Either's superpower is the ability to fail with an error message.

Our custom Writer's superpower is to log strings.

---

There's a saying in the Haskell community:

> Haskell has an overloaded semi-colon

In imperitave languages, commands are executed in order separated by a
semi-colon. In a sense, bind `>>=` is like a semi-colon. It binds together
monadic computations together like a semi-colon would in an imperitave language.

But in Haskell, this "semi-colon" is programmable. We can say what to do when
the semi-colon is reached. It comes with a way to tell the computer how to
combine two computations.

### do Notation

If you have multiple lines all bound together:

```haskell
threeInts mx my mz =
    mx >>= \k ->
    my >>= \l ->
    mz >>= \m ->
    let s = k + l + m in return s
```

Haskell has a special notation called `do` notation that makes things a little
more simple:

```haskell
threeInts' mx my mz = do
    k <- mx
    l <- my
    m <- mz
    return k + l + m
```

This does the same as the first example, just with some syntactic sugar. It's a matter of taste. For smaller number of binds, people tend to write them explicitly. For larger number of binds, they use `do`.

You can also use `let` with this notation

```haskell
threeInts' mx my mz = do
    k <- mx
    l <- my
    m <- mz
    let s = k + l + m
    return s
```

### Multiple Side-Effects

Often you'll want to do several side-effects. Like failing with an error message
*and* logging. There are several approaches.

- monad transformers to build custom monads
- effect systems to tailor make monads that have the exact features you want
  them to have

Plutus uses effect systems for the Contract and Trace monads.

You don't have to know everything about how an effect system works to use them
effectively. You just have to understand what each can do.
