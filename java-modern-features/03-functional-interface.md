# Functional Interface

A Functional Interface is an interface with **exactly one abstract method**.

### Real world Example

TV remote has different buttons

- Power → switch on/off TV
- Volume → Increase/decrease volume
- Channel → changes channel

Imagine an interface that defines just one action `press()`

```java
interface Remote
{
    void press();   // Remote is an interface with one abstract method -> press()
}

public class Main
{
    public static void main(String[] args) {
        Remote r = () -> System.out.println("TV is ON");
        r.press();
    }
}
```

### Steps

1. Define Interface
2. Provide Implementation : `() -> System.out.println("TV is ON")`
3. Call the Method : `r.press();`

`@FunctionalInterface` - tells Java that you intend the interface to have exactly one abstract method.

### Why we need Functional Interface

- Eliminating bulky Boilerplate code
- Enabling Functional Programming
- Powering the Streams API
- Compile time Type safety

### Old way

```java
Runnable r = new Runnable() {
    @Override
    public void run()
    {
        System.out.println("Running old way...");
    }
};
```

### New way

```java
Runnable r = () -> System.out.println("Running cleanly in new way");
```
