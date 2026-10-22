# Bean Scope

Bean scope defines how many objects Spring creates for a bean & how long those objects are used.

### Singleton

Only one single instance is created per IOC container.

Every request for that bean returns the same instance.

Singleton is the default Spring bean scope.

### Prototype

A new instance is created every single time the bean is requested or injected.

### Request (web application only)

Creates a single bean instance per HTTP request

### Session (web application only)

Creates single bean instance per HTTP session

---

### Singleton scope

If you request Engine bean twice from Spring, both reference to same object.

```java
@Component
@Scope("singleton")
class Engine
{
}

Engine e1 = context.getBean(Engine.class);
Engine e2 = context.getBean(Engine.class);
System.out.println(e1 == e2); // true
```

### Prototype scope

Each request to Spring creates new instance.

```java
@Component
@Scope("prototype")
class Engine
{
}

Engine e1 = context.getBean(Engine.class);
Engine e2 = context.getBean(Engine.class);
System.out.println(e1 == e2); // false
```
