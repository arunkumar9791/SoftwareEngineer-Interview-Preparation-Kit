# Inversion of Control (IOC)

IOC is a core architectural design principle where the control over object creation, configuration and life cycle management is transferred from the developer to the framework.

In simple terms,

IOC means giving control of object creation and dependency management to a framework instead of doing everything yourself.

### Real World Example

Imagine you're building a car.

A car needs an engine.

- Without IOC - You build the engine yourself and install it in the car
- With IOC - Someone supplies the engine and installs it for you.

In Spring that **Someone is the Spring IOC Container**

### Traditional Approach (Tightly Coupled)

Without IOC, a Car class must manually create its own dependency (Engine).

If you want to switch to a different Engine type, you have to modify Car class source code.

```java
public class Car {
    private Engine engine;

    public Car()
    {
        this.engine = new V8Engine(); // Tightly coupled
    }
}
```

### Spring IOC Approach (Loosely Coupled)

With IOC, Car class simply declares it needs an Engine. The Spring container creates Engine object first and supplies (inject) it into Car when it is instantiated.

```java
@Component
public class Car {
    private final Engine engine;

    @Autowired // Container automatically injects the Engine bean here
    public Car(Engine engine)
    {
        this.engine = engine;
    }
}
```
