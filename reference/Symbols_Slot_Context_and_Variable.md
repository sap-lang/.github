# ⚠️Warning⚠️: under construction

# Symbols Slot Context and Variable

## Symbols

Symbols are a way to reference a specific slot or value in a context.

```sap

a = 1 # a is a symbol
b = c # b is a symbol, c also is a symbol

```

symbol behaves like a variable in other languages, but it is not a variable.

for above example, `a` is a symbol that references the value `1`, `b` is a symbol that references the symbol `c`. `c` is a symbol that defaults to `undefined`.

## Slot

A slot is a place to store multi-cast functions

```sap

a ::= \ x -> x + 1
a ::= \ x y -> x + y

```

in the above example, `a` is a symbol which is storing a slot that stores two functions, the first function takes one argument and returns the argument plus one, the second function takes two arguments and returns the sum of the two arguments.

## Context

symbols have meaning in a context.

```sap

# the global context
a = 1

    # the local context
    a = 2
    b = 3
    2 = ^a

1 = ^a
undefined = ^b

```

```sap
if ::= \ true x _c y -> x
if ::= \ false x _c y -> y
```