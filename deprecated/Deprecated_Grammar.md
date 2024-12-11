
# ⚠️Warning⚠️ : Deprecated Feature

## indent as block delimiter

rule:

- new line + indent as `{`
- dedent as `}`
- `\n` in indent as `,` or `;`
- `,` `;` `=` `->` at the end of the line will ignore next `\n` but remember to keep the same indent level

```sap
x = {a: 1, b: "2", c: 3}

x = 
    a: 1
    b: "2"
    c: 3

f = \x -> 
    x + 1

f = \x -> {x + 1}

g =
\x ->
    \y ->
        x + y

g = \x -> {\y -> {x + y}}
```

## `|` to join new line with the previous line

```sap

then = \ f g a -> f (g a)

then
| \ x ->
    y = x + 1
    y
| \ x ->
    y = x + 2
    y
| 1

```

will be translated to

```sap
then = \ f g a -> f (g a)

then (\ x -> {y = x + 1; y}) (\ x -> {y = x + 2; y}) 1
```

another example, built-in `if` function in `sap`:

```sap
if a == 1
    f 1
    f 2
    f 3
| else
    f 4
    f 5
    f 6
```

will be translated to

```sap
if a == 1 {f 1; f 2; f 3} else {f 4; f 5; f 6}
```

## Operator precedence and Function-Call precedence

- `( )` has the highest precedence
- `.` has the second highest precedence
- the other operators have their precedence
- function call is weaker than any operator

function call will execute to the end of line, unless it is wrapped by `()`

```sap
a = f 1 + 2

a = f ( 1 + 2 )

a = (f (1 + 2))

a = (f (+ 1 2))
```

function call with operator

```sap
f a b + 1

f a (b + 1)

(f a (+ b 1))
```

method call

```sap
a.b c + d

(a.b) (c + d)

((a.b) (+ c d))
```

level up

```sap
if a == 1
    f 1
    f 2
    f 3
| else
    f 4
    f 5
    f 6

if a == 1 {f 1; f 2; f 3} else {f 4; f 5; f 6}

if (a == 1) {f 1; f 2; f 3} else {f 4; f 5; f 6}

if (a == 1) {(f 1); (f 2); (f 3)} else {(f 4); (f 5); (f 6)}

(if (a == 1) {(f 1); (f 2); (f 3)} else {(f 4); (f 5); (f 6)})
```

