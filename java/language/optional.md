# Optional in Java8

Used in our code as follows:
```
final String variable = Optional.ofNullable(oneFunction())
        .orElseGet(() -> anotherFunction());
```

Good explanation here:
https://www.oracle.com/technetwork/articles/java/java8-optional-2175753.html

Difference between `.orElse` and `.orElseGet`
- `.orElseGet` only executes the function passed as a parameter when the Optional is empty
- `.orElse` executes whether or not the Optional is empty

Therefore, seems safer to use `.orElseGet` to avoid side effects.  
