# Loose Coupling

Loose Coupling is a software design principle where components are made independent of one another.

A change in one class has minimal to no impact on other.

Dependency Injection helps achieve loose coupling by supplying dependencies from outside.

## 1. Tight Coupling

One class is highly dependent on a specific implementation of another class.

```java
class PetrolEngine {
    void start() {
        System.out.println("Petrol Engine Started");
    }
}

class Car {
    PetrolEngine engine = new PetrolEngine();

    void startCar()
    {
        engine.start();
    }
}
```

Here, replacing Petrol Engine with Diesel Engine requires modifying Car class.

## 2. Loose Coupling

One class depends on an abstraction rather than a specific implementation.

Allows different implementation to be supplied.

```java
interface Engine
{
    void start();
}

class PetrolEngine implements Engine {
    public void start()
    {
        System.out.println("Petrol Engine Started");
    }
}

class DieselEngine implements Engine {
    public void start()
    {
        System.out.println("Diesel Engine Started");
    }
}

class Car
{
    private Engine engine;

    Car(Engine engine)
    {
        this.engine = engine;
    }

    void startCar() {
        engine.start();
    }
}
```

Car depends on Engine interface, not directly on Petrol Engine or Diesel Engine.

```java
Car c1 = new Car(new PetrolEngine());
Car c2 = new Car(new DieselEngine());
```

## How Spring helps

Spring uses Dependency Injection (DI) to provide the required objects instead of making a class create its dependencies itself.

```java
@Component
class PetrolEngine implements Engine
{
    public void start()
    {
        System.out.println("Petrol engine starts");
    }
}

@Component
class Car
{
    private final Engine engine; // declares dependency

    Car(Engine engine) // Constructor Injection
    {
        this.engine = engine;
    }
}
```

- `@Component` - Makes the class eligible for Spring bean management
- Spring container creates and injects the dependency.

> If multiple Engine implementations are present, use `@Primary`, `@Qualifier` to choose the correct one

```java
public interface Engine
{
    void start();
}

@Service
@Primary // Marks this bean as default fallback
public class PetrolEngine implements Engine
{
    @Override
    public void start()
    {
        System.out.println("Petrol Engine Starts");
    }
}

@Service("dieselEngine")
public class DieselEngine implements Engine
{
    @Override
    public void start()
    {
        System.out.println("Diesel Engine Starts");
    }
}
```

### Using @Primary (Automatic Default)

```java
@Service
class Car
{
    private final Engine engine;

    Car(Engine engine) // Inject PetrolEngine automatically because of @Primary
    {
        this.engine = engine;
    }
}
```

### Overriding with @Qualifier

```java
@Service
class Car
{
    private Engine engine;

    public Car(@Qualifier("dieselEngine") Engine engine) {
        this.engine = engine;
    }
}
```

`@Qualifier` overrides `@Primary` and forces Spring to inject Diesel Engine

### Relationship

- Interface → defines what an Engine must do
- Implementation → defines how Engine works
- Dependency Injection → supplies Engine to the Car
- Loose Coupling → Let the car work with different engines without changing its own code.
