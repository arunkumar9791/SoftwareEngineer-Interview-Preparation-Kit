# Lambda Expression

A Lambda is an anonymous function that implements a functional interface : `(parameters) -> body`

It has no name, no class and no declared return type

```java
Runnable r = () -> System.out.println("Hi");
Consumer<String> c = s -> System.out.println(s);
Comparator<String> cmp = (a, b) -> a.compareTo(b);
```

### Before

```java
// Java 7
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.length() - b.length();
    }
});
```

### New

```java
// Java 8 : lambda
names.sort((a, b) -> a.length() - b.length());

// Better approach
names.sort(Comparator.comparingInt(String::length));
```

---

A Lambda expression is a short way of writing the implementation of a functional interface.

### Syntax

```java
(parameters) -> expression

(or)

(parameters) -> {
    // statements
}
```

### Lambda with multiple parameters

Example

```java
(int a, int b) -> a + b;

(or)

(a, b) -> a + b;
```

### Lambda with no parameters

```java
Greeting g = () -> System.out.println("Hello");
```

### Lambda with one parameter

```java
Square s = x -> x * x;

(or)

Square s = (x) -> x * x;
```

| Single expression | Multiple statements |
|---|---|
| Result is automatically returned. Don't need to write `return a + b;` | Must explicitly use `return` |
| `(a, b) -> a + b` | `(a, b) -> { int result = a + b; System.out.println(result); return result; }` |
