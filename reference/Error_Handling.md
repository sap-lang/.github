# Error Handling with cofunction


```java
void func() {
    // scope 0
    try {
        // scope 1
    } catch (Exception e) {
        // scope 2
    } finally {
        // scope 3
    }
}
```

```mermaid
graph TD
    A[scope 0] --> B[scope 1]
    B --> C[scope 2]
    B --> D[scope 3]
    C --> D
```

you can use `effect` to handle error in sap

# ⚠️Warning⚠️: under construction