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


```elixir
f = \ -> {
    # scope 0
    
    # due to it is a cofunction, we can return the control flow
    res -> catch = try \ -> {
        # scope 1
    }

    if typeof res == "error" {
        catch \e -> {
            # scope 2
        }
    }

    # finally
    # scope 3
}



```