# Lambda Expression Calling Existing Methods (Method Reference)

Lambda Expression in Java can directly invoke static methods and concrete (instance or static) methods inside their body.

They can be replaced entirely by clean **Method Reference** using `::` operator.

## Resolve confusion between lambda, functional interface & method reference

Imagine we have remote control.
The remote has button called `calculate()`

**Functional Interface:**

```java
interface Calculator {
    int calculate(int a, int b);   // remote doesn't know how to calculate
}
```

This means

> "Anyone who wants to be Calculator must provide a `calculate()` operation that takes two integers and returns an integer."

Now we need to tell Java what should happen when `calculate()` is called.

### 1. Traditional way - Anonymous class

Before Lambda

```java
Calculator calculator = new Calculator() {
    @Override
    public int calculate(int a, int b) {
        return a + b;
    }
};

calculator.calculate(10, 20);   // o/p 30
```

```
calculator
    ↓
I need calculate()
    ↓
Anonymous class provides calculate()
    ↓
10 + 20
```

### 2. Lambda - I'll give you the behavior

```java
Calculator calculator = (a, b) -> a + b;
calculator.calculate(10, 20);   // o/p 30
```

```
calculator
    ↓
calculate(10, 20)
    ↓
lambda
(a, b) -> a + b
    ↓
30
```

This is the fundamental purpose of lambda.
A lambda is basically a compact way of providing behavior to a functional interface.

### 3. Now imagine the behavior already exists

Suppose we / someone has already written this

```java
class MathUtils {
    static int add(int a, int b) {
        return a + b;
    }
}
```

Then we don't need to write

```java
(a, b) -> a + b;
```

because the behavior already exists.

```java
MathUtils.add(a, b)
```

So, we could do:

```java
Calculator calculator = (a, b) -> MathUtils.add(a, b);
```

Now see what's happening

```
Calculator
    ↓
calculate(a, b)
    ↓
Lambda → MathUtils.add(a, b)
(a, b)          ↓
          existing method
```

The lambda is essentially connecting the functional interface to the existing method.

### 4. Then Java gives us an even shorter syntax

**Static Method**

If your lambda is only calling an existing method, you don't necessarily need to write the lambda.

So

```java
(a, b) -> MathUtils.add(a, b)
```

can become

```java
MathUtils::add
```

This is called a **Method Reference**.

So the lambda expression can be written as

```java
Calculator calculator = MathUtils::add;
```

```
Functional Interface
     calculate()
          |
Lambda                  Existing method
(a, b) -> a + b         MathUtils.add()
                              |
                        Method Reference
                        MathUtils::add
```

Lambda and Method Reference are not two completely different mechanisms.

A Method Reference is basically a more concise way of expressing certain lambdas that delegate to an existing method.

### Concrete / instance method

```java
class CalculatorService {
    int add(int a, int b)
    {
        return a + b;
    }
}

CalculatorService service = new CalculatorService();

Calculator calculator = (a, b) -> service.add(a, b);
        ⇕
Calculator calculator = service::add;
```

### How `service::add` possible?

```
calculate(int, int) → int
add(int, int)       → int
```

Here, the method names don't matter.
What matters is their shape / signature.
They match.
Therefore Java can connect them

```java
Calculator calculator = service::add;
```

So

`calculator.calculate(10, 20)` effectively performs `service.add(10, 20);`

## Comparing options / connecting wire

```java
interface Calculator {
    calculate(int, int) -> int
}
```

**Option 1** (Lambda) → Write behavior yourself

```java
(a, b) -> a + b
```
`calculator.calculate()` → lambda → `a + b`

**Option 2** - Delegate to an existing method

```java
MathUtils::add
```
`calculator.calculate()` → `MathUtils.add()`

**Option 3** - Delegate to an existing object's method

```java
service::add
```
`calculator.calculate()` → `service.add()`

### This is why functional programming uses lambdas so much

Suppose we have

```java
List<Integer> numbers = List.of(10, 20, 30);
```

Traditional loop:

```java
for (Integer n : numbers) {
    System.out.println(n);
}
```

Java functional API

```java
numbers.forEach(n -> System.out.println(n));
```

we can write this as

```java
numbers.forEach(System.out::println);
```

> This is where you'll see method references everywhere in Modern Java

## Entire Java picture

```
Java Program
     ↓
Functional Interface
     |
One Behavior
     |
 ┌───┴──────────────┐
 ↓                  ↓
Lambda        Existing Method
 |                  ↓
 |          Method Reference
 └────────┬─────────┘
          ↓
   Functional APIs
          |
 ┌────────┼────────┐
 ↓        ↓        ↓
Stream  forEach  filter
```

**Functional Programming**

Lambda is a way to provide behavior to a functional interface. That behavior may itself call an existing method.

If the lambda simply calls an existing method, Java often lets us replace the lambda with a method reference.

---

# Method Reference

A Method Reference (`::`) is shorthand for a lambda that only calls an existing method.

There are four kinds

| Kind | Syntax | Lambda equivalent |
|---|---|---|
| Static method | `Integer::parseInt` | `s -> Integer.parseInt(s)` |
| Instance method of a particular object | `System.out::println` | `x -> System.out.println(x)` |
| Instance method of an arbitrary object of a type | `String::toUpperCase` | `s -> s.toUpperCase()` |
| Constructor | `ArrayList::new` | `() -> new ArrayList<>()` |

### Static Method Reference

→ used to refer a static method

Syntax : `ClassName::staticMethod`

```java
class Demo {
    static void greet()
    {
        System.out.println("Hello");
    }
}

Runnable r = Demo::greet;
r.run();
```

```java
Function<String, Integer> f = Integer::parseInt;
System.out.println(f.apply("123"));
```

### Instance Method of particular Object

used when referring to a method of an existing object.

Syntax : `object::instanceMethod`

```java
List<String> names = List.of("Arun", "Kavi");
names.forEach(System.out::println);
```

### Instance method of an Arbitrary Object

used when Java should call an instance method on each object supplied as input.

Syntax : `ClassName::instanceMethod`

```java
List<String> names = List.of("arun", "kavi");
names.stream().map(String::toUpperCase)
     .forEach(System.out::println);

// o/p
// ARUN
// KAVI
```

### Constructor Reference

used to refer to a constructor to create objects.

Syntax : `ClassName::new`

```java
Supplier<ArrayList<String>> s = ArrayList::new;
ArrayList<String> list = s.get();
```

equivalent lambda ⇕

```java
Supplier<ArrayList<String>> s = () -> new ArrayList<>();
```

## Common Functional Interface

| Interface | Main Method |
|---|---|
| `Runnable` | `run()` |
| `Consumer<T>` | `accept()` |
| `Function<T, R>` | `apply()` |
| `Supplier<T>` | `get()` |
| `Predicate<T>` | `test()` |

### Let's see how Java understands `String::toUpperCase`

- `String` ⇒ Specifies the class whose instance method we want to use.
- `::` ⇒ The method-reference operator. It refers to a method without calling it immediately.
- `toUpperCase` ⇒ The instance method that will be called on the supplied string.

## Arbitrary Object

An Arbitrary Object means an object that we haven't fixed in advance.

We can use whichever suitable object is given to us.

Imagine we have three person

- Person 1 : Arun
- Person 2 : Kavi
- Person 3 : Kutty Arun

In Java, we can write as

```java
System.out.println("Hello Arun");
System.out.println("Hello Kavi");
System.out.println("Hello Kutty");
```

Imagine we need to greet 1000 people.
We will not write separate statement for each person.

We would create a reusable way to greet someone, then supply the person you want to greet.

This is the basic idea behind working with arbitrary object.

The operation can work with different objects instead of being tied to one specific object.

### Use String Objects

```java
String name1 = "Arun";
String name2 = "Kavi";
System.out.println(name1.toUpperCase());
System.out.println(name2.toUpperCase());

// o/p
// ARUN
// KAVI
```

Operation (`toUpperCase`) is same, object on which it operates changes.

### Fixed Object

```java
String name = "arun";
Supplier<String> result = name::toUpperCase;
```

Means, whenever this function is called, execute `toUpperCase()` on the object referenced by `name`.

The object is already decided : `"arun"`

### Arbitrary Object

```java
Function<String, String> result = String::toUpperCase;
System.out.println(result.apply("arun"));
System.out.println(result.apply("kavi"));
```

→ This means Give me a String object and I'll call `toUpperCase()` on that object.

The function is reusable because the target object isn't fixed when the method reference is created.
