

## pattern matching

```sap

f ::= \ "hello" b {c: d} ->
    d = a + b
    c * d

f "hello" 1 {c: 2}
```

```js
function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function(...args2) {
        return curried.apply(this, args.concat(args2));
      }
    }
  };
}

function call(slot, ...call_args) {
    for (const [index, _f] of slot.entries()) {
        try {
            const res = curry(_f)(...call_args);
            return res;
        } catch (e) {
            if (index === slot.length - 1) {
                throw e;
            }
            if (e.message === "pattern matching failed") {
                continue;
            }
        }
    }
}


_f_slot = []
function* _f114514(a, b, c) {
    if (c === "hello" && const {c: d} = c) {
        const d = a + b;
        return c * d;
    } else {
        throw new Error("pattern matching failed");
    }
}
_f_slot.push(_f114514);

call(_f_slot, "hello", 1, {c: 2});
```

## implicit argument

`implicit argument` is var start with `?`, which is the argument that is not passed in the function call.

```sap
f ::= \a ->
    a + ?b

b = 1
f 1 # 2
```

```js
_pre_f = {}
_f_slot = []
function* _f114514(a) {
    return a + _pre_f.b;
}
_f_slot.push(_f114514);


b = 1;
_pre_f.b = b;
call(_f_slot, 1);
```