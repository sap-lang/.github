# Implicit Parameters and Coeffects

> Implicit Parameters as `coeffect` in the context of `effects`.

Implicit parameters are a way to pass parameters to functions implicitly.

## What is effect and coeffect?

Algebraic Effects and Handlers are a way to model effects in programming languages. In this model, `effect` is a way to model the behavior of a function, and `coeffect` is a way to model the context of a function.

In the context of `effects`, `implicit parameters` are considered as `coeffect`.

## Syntax

In function definition after `\` (lambda) with following necessary parameters, you can then define implicit parameters after `?` separator.

```sap
sort ::= \ [] ? cmp -> []
sort ::= \ [x, ...xs] ? cmp ->
    smaller = filter (\y -> cmp x y) xs
    larger = filter (\y -> !(cmp x y)) xs
    (sort cmp smaller) + [x] + (sort cmp larger)

# in current context, we define a cmp function to compare two integer elements
cmp = \x y -> x < y

main = sort [3, 1, 4, 1, 5, 9, 2, 6, 5] # the `cmp` will be passed implicitly by name
main # [1, 1, 2, 3, 4, 5, 5, 6, 9]
```
