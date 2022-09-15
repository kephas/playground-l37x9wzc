# Let's play with Functors!

Okay, let's start with something basic that we encounter a lot in Functional Programming: an Option.

```haskell
data Option a = Some a | None
  deriving Show
```

Great, with this, we can represent a result that can be absent, in case of errors. We'll pattern match on it and the compiler can guarantee us we never forgot to deal with the `None` case.

Better yet, we cannot write code that tries to use an absent value, because we get the value from the pattern where it exists:

```haskell runnable
data Option a = Some a | None deriving Show

(/?) :: (Eq a, Fractional a) => a -> a -> Option a
num /? denom =
  if denom == 0 then
    None
  else
    Some $ num / denom

formula1 :: (Eq a, Fractional a) => a -> a -> a -> Option a
formula1 x y z =
  case x /? y of
    None -> None
    Some xy -> Some $ xy + z

main = do
  print $ formula1 8 2 1
  print $ formula1 8 0 1
```

Now, it's safe (no more null pointer errors!), but not very practical. We don't wan't a cascade of cases to perform a series of operations, where all `None` case are the same each time. That's a lot of boilerplate and would make the code unreadable. That's a sin in Haskell. So let's add an operation to `Option`, to apply a function to its content if it's there. Like `map`, but for options instead of lists.

```haskell
optMap :: (a -> b) -> Maybe a -> Maybe b
optMap _ None = None
optMap f (Some x) = Some $ f x
```

Pretty straightforward, huh? Let's see it in action:

```haskell runnable
data Option a = Some a | None
  deriving Show

optMap :: (a -> b) -> Option a -> Option b
optMap _ None = None
optMap f (Some x) = Some $ f x

(/?) :: (Eq a, Fractional a) => a -> a -> Option a
num /? denom =
  if denom == 0 then
    None
  else
    Some $ num / denom

formula1 :: (Eq a, Fractional a) => a -> a -> a -> Option a
formula1 x y z =
  optMap (+ z) $ x /? y

main = do
  print $ formula1 8 2 1
  print $ formula1 8 0 1
```
