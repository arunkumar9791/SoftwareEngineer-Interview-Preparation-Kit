# Functional Program

Functional Program means building your program by using functions to process data.

It is a programming paradigm where you build programs using functions.

Java supports functional programming features, especially from Java 8 onwards.

**Important features:**

- **Lambda Expressions** : `n -> n * 2`
- **Functional Interfaces** : Interfaces with single abstract method
- **Stream API** : process collections using operations such as `map()`, `filter()` & `reduce()`
- **Method References** : a shorter way to refer to existing methods.

### Add 10 bonus marks to every students

```java
import java.util.Arrays;

int[] marks = {40, 70, 90};
int[] newMarks = Arrays.stream(marks)
        .map(mark -> mark + 10).toArray();
```

- `stream()` - process the elements of a collection
- `map()` - transform each element.
- `toArray()` - collects results into new array.
