# Function Guard


A guard is a way to add a condition to a function. If the condition is not met, the function will not be executed.

It is very helpful when we using slot function.


## Syntax

```sap

f ::= \0 -> 1
f ::= \x ? y : x < 10 ->  y ? y : 5
f ::= \x : x > 10 -> 10

```

calling f

```sap
// case 0
1 = f 0

// case 1 without implicit parameter y
5 = f 5

// case 1 with implicit parameter y
y = 6
6 = f 5

// case 2
10 = f 11
```