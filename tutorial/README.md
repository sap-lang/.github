why use prototype-based object in 2025?


## Assignment

### assign by value
```sap
a = b
```

a is bind to b, b is bind to a, binded by value.

### shadowing

```sap
1 = a
a = 1 # shadowing
2 = a # shadowing
```

### pattern matching

```sap
^[a, b] = [1, 2]
^[a, ...b] = [1, 2, 3]

```

### what do you mean by mutable value?
i don't think you actually need mutable value.

## pre-compile compile and runtime

at runtime, you normally think of the code which is running is **compiled**.

but what if we unify the compile and runtime?

we can think of the prototype of the object is the previous compile step, and the object is the runtime step.


