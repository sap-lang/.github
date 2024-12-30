# SAP Grammar and Parsing

> This is a draft document, the grammar and parsing rules are not finalized yet.

SAP is a language that is designed to be easy to read and write. The syntax is inspired by Rhombus. The language is designed to be simple and easy to understand, with a focus on readability and expressiveness.

## C-like syntax

SAP has a C-like syntax, with a focus on simplicity and readability.

```sap
(a == 1) ? {
    f 1
    f 2
    f 3
} : {
    f 4
    f 5
    f 6
}

arr = [1, 2, 3, 4, 5]

map = {
    a: 1,
    b: 2,
    c: 3
};

lambda = \x -> {x + 1};
```

following sections will explain how C-like syntax is transformed to SAP syntax.
