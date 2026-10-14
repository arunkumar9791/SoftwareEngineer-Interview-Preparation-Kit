# Stream

A Stream is a way to process a collection of data →

- filtering numbers,
- finding names,
- sorting values,
- calculating totals

without writing lengthy loops

### Find only the numbers greater than 20.

**Traditional**

```java
List<Integer> numbers = List.of(10, 15, 20, 25, 30);
for (int n : numbers) {
    if (n > 20) {
        System.out.println(n);
    }
}

// o/p
// 25
// 30
```

**Using Stream**

```java
List<Integer> numbers = List.of(10, 15, 20, 25, 30);
numbers.stream().filter(n -> n > 20)
       .forEach(System.out::println);
```

Streams let you express the processing steps more clearly.

A Stream lets you describe what you want to do with data, rather than manually controlling every step of how to process it.

### Find students who scored atleast 60 marks

```java
students.stream().filter(s -> s.getMarks() >= 60)
        .forEach(System.out::println);
```

## How Stream Work?

A Stream pipeline generally has three parts

1. **Source** → where data comes from list, set, array etc.
2. **Intermediate Operations** → process (or) transforms data — zero (or) more — `filter()`, `map()`, `sorted()`
3. **Terminal Operation** → Produces a result or performs action — `collect()`, `forEach()`, `count()`

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

List<Integer> result =
        numbers.stream()             // → source [1, 2, 3, 4, 5, 6]
        .filter(n -> n % 2 == 0)     // → keep even numbers [2, 4, 6]       } Intermediate
        .map(n -> n * 10)            // → multiply each by 10 [20, 40, 60]  } operation
        .toList();                   // → collect results into list → Terminal operation

System.out.println(result);

// o/p : [20, 40, 60]
```

> ⚠️ **Original list is not modified**

## Intermediate operations

| Operation | Description |
|---|---|
| `filter(Predicate)` | keeps matching elements |
| `map(Function)` | transforms each element 1 → 1 |
| `sorted()` / `sorted(Comparator)` | sorts data |
| `flatMap(Function)` | each element → stream, flattened 1 → many |
| `mapToInt` / `mapToObj` | switch between object and primitive streams |
| `distinct()` | removes duplicates using `equals` / `hashCode` |
| `peek(Consumer)` | looks at elements for debugging |
| `limit(n)` / `skip(n)` | truncate / skip first n |
| `takeWhile` / `dropWhile` (Java 9) | take or drop while predicate holds |

## Terminal Operations

| Operation | Returns |
|---|---|
| `forEach`, `forEachOrdered` | returns `void` |
| `collect(Collector)` | returns collection, map, String etc., |
| `toList()` (Java 16) | returns unmodifiable list |
| `reduce(identity, op)` | returns `T` (or `Optional<T>` without identity) |
| `count` | returns `long` |
| `min(cmp)` / `max(cmp)` | returns `Optional<T>` |
| `anyMatch` / `allMatch` / `noneMatch` | returns `boolean` |
| `findFirst()` / `findAny()` | returns `Optional<T>` |
| `toArray()` | returns `Object[]` (or) typed Array |
