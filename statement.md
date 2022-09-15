# Let's play with Functors!

Okay, let's start with something basic that we encounter a lot in Functional Programming: an Option.

```haskell runnable
data Option a = Some a | None deriving Show
```

Great, with this, we can represent a result that can be absent, in case of errors. We'll pattern match on it and the compiler can guarantee us we never forgot to deal with the `None` case.

Better yet, we cannot write code that tries to use an absent value, because we get the value from the pattern where it exists:

```haskell runnable
safeDiv :: Num a => a -> a -> Option a
safeDiv num denom =
  if denom == 0 then
    Some $ num / denom
  else
    None

formula1 :: Num a => a -> a -> a -> Option a
formula1 x y z = 


# Advanced usage

If you want a more complex example (external libraries, viewers...), use the [Advanced Haskell template](https://tech.io/select-repo/2320)
