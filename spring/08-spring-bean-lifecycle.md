# Spring Bean Lifecycle

Represents the complete sequence of steps a bean goes through from its creation to its final destruction within the IOC container.

### Stages

1. Instantiation - Spring creates the bean object
2. Dependency Injection - Spring provides required dependencies
3. Initialization - Spring performs setup & initialization callbacks
4. Bean ready to use - Your application can use the bean
5. Destruction - Spring performs cleanup when the bean is being destroyed.

```java
@Component
public class DatabaseConnectionManager
{
    public DatabaseConnectionManager()
    {
        // Instantiation
        System.out.println("1. Constructor: Bean instance created.");
    }

    @PostConstruct
    public void init()
    {   // Initialization callback
        System.out.println("2. @PostConstruct: Establishing database connection pool");
    }

    public void executeQuery()
    {   // Active use
        System.out.println("3. Business logic: Executing DB queries");
    }

    @PreDestroy
    public void cleanup()
    {   // Destruction callback
        System.out.println("4. @PreDestroy: Closing DB connection");
    }
}
```

| Step | What happens |
|---|---|
| Constructor | Spring creates DatabaseConnectionManager object |
| `@PostConstruct` | Spring calls `init()` after dependency injection is completed. |
| `executeQuery` | Your application uses the bean |
| `@PreDestroy` | Spring calls `cleanup()` when the bean is being destroyed. |

Spring IOC container manages the bean life cycle including object creation, dependency injection, initialization & destruction callbacks.

Spring normally manages the full lifecycle of singleton bean.

For prototype-scope beans, Spring creates and configures them but does not automatically manage their destruction callbacks.
