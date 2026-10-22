# Dependency Injection

Dependency Injection is a design pattern where an object receives its required dependencies from an external source such as Spring IOC Container rather than creating them itself.

Three types of Injection

- Constructor Injection
- Setter Injection
- Field Injection

## Constructor Injection

Dependencies are provided through class constructor.

It is the strongly recommended approach.

It guarantees that dependencies are mandatory, makes the field immutable (final) & allows unit testing using simple mocks.

```java
@Component
public class Car
{
    private final Engine engine; // Immutable & safe

    public Car(Engine engine)
    {
        this.engine = engine;
    }
}
```

## Setter Injection

Spring container instantiates bean using no-argument constructor first, then inject dependencies by calling public setter method.

Best reserved for optional or mutable dependencies that can be reconfigured or changed later.

```java
@Component
public class Car
{
    private Engine engine;

    @Autowired
    public void setEngine(Engine engine)
    {   this.engine = engine;
    }
}
```

## Field Injection

Dependencies are injected directly into class fields using reflection, typically by placing `@Autowired` on private variables.

**It is highly discouraged.** It hides class dependencies, introduces tight coupling, makes unit testing difficult.

```java
@Component
public class Car
{
    @Autowired
    private Engine engine; // simple to write
                           // but bad for testing.
}
```
