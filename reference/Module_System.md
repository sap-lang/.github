# Module System

Module system is a way to organize your code into modules. A module is a collection of related code that can be imported and used in other modules. Modules can be nested, and can be imported from other modules.

if you are familiar with the module system of Rust, you will find the module system of sap very similar.


## File Structure
```
project/
├── project.sap
|-- a.sap
|-── b/
|   ├── b.sap
|   ├── c.sap
|   └── d/
|       └── d.sap
└── build.sap
```

Above is an example of a project which could be used either as a executable or library,
- `project.sap` is the library file and binary file of the project.
  - in this file the `main` function will be called if the project is used as a executable. 
- `build.sap` is the build file of the project.
  - it will contains the dependencies of the project.
  - it will contains the build configurations of the project.
  - it will contains the build scripts of the project.
- `a.sap` is a module, it is a sub library of `project`, so it could be imported as `project.a`.
- `b` is a sub directory of `project`, `b.sap` is the module file.
- `c.sap` is a module, it is a sub library of `b`, so it could be imported as `project.b.c`.

## Exporting Modules
```sap
// a.sap
f = \ x -> {
    x + 1 |> puts
}

g = \ x -> {
    x + 2 |> puts
}

@export {f,g}
```

`@export` is an macro which takes a object and exports the object kv pairs to the module.


## Importing Modules
```sap
// project.sap

@import project.a {f,g}
@import project.a
@import project.a {f,g,...a}

ff = \ x -> f x

@export { ff }
```


`@import` is an macro which takes an id or string(path) and an object pattern

for example

- `@import a` will export all the exported objects of module `a` to the current module.
- `@import a {f,g}` will export only `f` and `g` from module `a` to the current module.
- `@import a {f,g,...a}` will export `f` and `g` from module `a` to the current module and all the other exported objects as `a` object to the current module.

## Name Resolution

Name resolution is done in the following order:
1. find the other `.sap` files in the same directory.
2. check the folder from the same directory, if inside the folder contains a `.sap` file with the same name as the folder, then the folder is a module.
3. `.deps` folder of the project, if the module is not found in the above steps, then the `.deps` folder will be checked for the module.


Import resolution is done in the following order:
1. modules will be initialized with tree structure, `@import` will be resolved after the whole tree is initialized.
2. in sub modules, you can import the parent module with `@import @super` syntax.
3. in sub modules, you can import the sibling module with normal `@import` or `@import @super.sibling` syntax.
4. could not recursively import the same module, it will cause a circular dependency error.