# Type Hints and Constant Function

```sap

fib ::= \ 0 -> 0
fib ::= \ 1 -> 1
fib ::= \ x : x < 0 -> format "not a nat" |> throw
fib ::= \ x -> (fib (x-1)) + (fib (x-2))

```

## Type Hints

for above `fib` function we know that `x` should be a Number, but if we use `:` everytime to check if type is a number will repeat outself

```sap

fib ::= \ 0 -> 0
fib ::= \ 1 -> 1
fib ::= \ x : x < 0 -> format "not a nat" |> throw

$ number -> number
fib ::= \ x -> (fib (x-1)) + (fib (x-2))
```

`$` in front with `lambda` of pattern to pattern is a type hint


## Constant Function

as we all know if the param of `fib` is constant, is meaning less to redo the calculation (it is not a cofunction neither it has an effect)

we can mark it as const
```sap

fib ::= \ 0 -> 0
fib ::= \ 1 -> 1
fib ::= \ x : x < 0 -> format "not a nat" |> throw

$$ number -> number
fib ::= \ x -> (fib (x-1)) + (fib (x-2))
```

`$$` means type hinted and constant evaluatable


type hints and constant hints propagated from current scope to all child scopes.
