# ⚠️Warning⚠️ : Deprecated Feature

# Function Currying

Currying is a technique of evaluating a function with multiple arguments, into a sequence of functions with a single argument.

## Why Currying?

Currying is a technique that allows you to create new functions by partially applying a function. This can be useful when you want to create a new function that is similar to an existing function, but with some of the arguments pre-filled.

### Advantages of Currying

- **Code Reusability**: Currying allows you to create new functions by partially applying existing functions, which can help reduce code duplication.
- **Flexibility**: Currying allows you to create new functions with different sets of arguments, which can make your code more flexible and easier to maintain.
- **Composition**: Currying can be used to compose functions together, which can help you build complex functions from simpler ones.


## How to use Currying in `sap`?

In `sap` Currying is a built-in feature.

```sap
add = \ a b -> a + b

add_one = add 1
add_two = add 2

add_one 2 # 3
add_two 1 # 3
```

In the above example, we define a function `add` that takes two arguments `a` and `b` and returns the sum of `a` and `b`. 

We then create two new functions `add_one` and `add_two` by partially applying the `add` function with the arguments `1` and `2` respectively. 

When we call `add_one 2` and `add_two 1`, we get the expected results `3` in both cases.



## Reference

- [Currying - Wikipedia](https://en.wikipedia.org/wiki/Currying)