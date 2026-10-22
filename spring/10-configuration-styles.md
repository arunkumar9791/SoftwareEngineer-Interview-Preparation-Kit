# Configuration Styles in Spring Framework

There are 3 main configuration styles in Spring.

- XML-based configuration
- Annotation-based configuration
- Java-based configuration

## 1. XML-based configuration

We configure Spring using an XML file, usually named `applicationContext.xml`

Imagine we have two Java classes

```java
public class Engine
{
    public void start()
    {
        System.out.println("Engine Started");
    }
}
```

```java
public class Car
{
    private Engine engine;

    public void setEngine(Engine engine)
    {
        this.engine = engine;
    }

    public void drive()
    {
        engine.start();
        System.out.println("Car is moving");
    }
}
```

We configure these classes in XML

```xml
<beans>
    <bean id="engine" class="com.example.Engine"/>
    <bean id="car" class="com.example.Car">
        <property name="engine" ref="engine"/>
    </bean>
</beans>
```

What happens here?

- Spring creates Engine object
- Spring creates Car object
- Spring injects Engine object into Car object.

We don't have to connect objects manually using `new Engine()` inside Car.

## 2. Annotation-based configuration

In this approach, we use annotations directly in our Java classes to tell Spring which classes it should manage.

```java
@Component
public class Engine
{   public void start()
    {
        System.out.println("Engine Started");
    }
}
```

```java
@Component
public class Car
{
    private final Engine engine;

    @Autowired
    public Car(Engine engine)
    {
        this.engine = engine;
    }

    public void drive()
    {
        engine.start();
        System.out.println("Car is moving");
    }
}
```

We also need to tell Spring to scan the package for annotated class. This can be enabled through Java Configuration

```java
@Configuration
@ComponentScan("com.example")
public class AppConfig
{
}
```

What happens here?

- `@Component` tells Spring that the class can be managed as Bean
- `@ComponentScan` tells Spring where to look for the classes
- `@Autowired` tells Spring to supply the required dependency. With suitable constructor, Spring automatically uses it. So `@Autowired` can be omitted.

## 3. Java-based Configuration

In this approach, we use Java classes and the `@Bean` annotation to explicitly define Beans.

```java
@Configuration
public class AppConfig
{
    @Bean
    public Engine engine()
    {   return new Engine();
    }

    @Bean
    public Car car()
    {   return new Car(engine());
    }
}
```

Here, the Car class uses the constructor.

```java
public Car(Engine engine)
{
    this.engine = engine;
}
```

What happens here?

- `@Configuration` identifies a class containing Spring configuration
- `@Bean` tells Spring to manage the object returned by the method.
- `engine()` defines the Engine Bean.
- `car()` defines the Car Bean & its dependency.
