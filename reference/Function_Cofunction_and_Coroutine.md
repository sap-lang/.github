## Function Cofunction and Coroutine

> Concept: the word `routine` just exactly means `function`

Please read the following document. It is **REALLY** important to understand why we design our language in this way.

### What is a normal routine?

A normal routine is a function that is called and executed in a synchronous way. When a function is called, it will run until it returns a value. The function will block the execution of the program until it returns.

### What does `co` mean in math?

`co` is a prefix that means together or with. It is used in many mathematical terms to indicate that two things are combined or working together.

For instance, `sine` is a function that takes an angle and returns the ratio of the length of the opposite side to the length of the hypotenuse of a right triangle. `cosine` is a function that takes an angle and returns the ratio of the length of the adjacent side to the length of the hypotenuse of a right triangle.

### What is a `co` routine?

A coroutine is a function that can be paused and resumed. When a coroutine is called, it will run until it reaches a `yield` (which is `<-` operator) statement. The coroutine can then be resumed later, and it will continue running from where it left off.

### TL;DR

A coroutine is a **function** that can be **paused** and **resumed** with (or without) value.

### Our language design

In `sap` we **DO NOT** have normal routine (function), we only have coroutine (well, cofunction).

```sap
a = \a -> {
    a = <- a + 1
    a = <- a + 2
    a = <- a + 3
    a + 4
}
```

### How to call a cofunction

Due to our language design having pattern matching, we can match a function call either with one value or with two values.

If you just want to call a function, you can just call it with one value.

```sap
a = a 1
2 == a
```

### How to enter the continuation of a cofunction

After calling the cofunction, the return value of a cofunction will be a value with its continuation.

In sap language, any value could carry an extra field called next, which is the **state** of the cofunction.

If you are familiar with `C` and `ucontext`, you can think of the **state** as the `ucontext` of the function.

After the cofunction has ended, the **state** will be `null`.

```sap
a -> next = a 1
next a
```

Or using pattern matching to split value, thus the value no longer carries the next field

```sap
value -> next = a 1
```

### Replace Current Continuation with Inner

```sap
f = \x -> {
    <- x + 2
    <- x + 3
    <- x + 4
}

g = \y -> {
    <- y + 1
    <<- f y
    <- y + 5
}

a = 0
a -> next = g a
puts a
```

the `<<-` operator will replace the current continuation with the inner continuation.

## Syntax Sugar for No Parameter Cofunction

If a cofunction does not have any parameter, you can define it with `_{}`

```sap
f = _{
    1
}
```

### Reference
- https://en.wikipedia.org/wiki/Coroutine
- https://en.wikipedia.org/wiki/Dual_(category_theory)