# Spring Container

Spring IOC container is the core engine of the Spring Framework - creates, manages and connects objects.

It is responsible for instantiating, configuring, assembling and managing the entire lifecycle of application objects known as **Spring Beans**.

## Core workflow of the Spring Container

1. Read Metadata
2. Instantiate & Wire
3. Manage Lifecycle

### Read Metadata

Container reads configuration metadata provided via Java Annotations (`@Component`, `@Bean`)

Java Configuration classes (`@Configuration`) or XML files.

### Instantiate & Wire

It instantiates Java classes into beans & automatically inject their dependencies using constructor or setter injection.

### Manage Lifecycle

- It executes lifecycle setup (`@PostConstruct`)
- manages their active scope
- runs cleanup tasks (like `@PreDestroy`) when application shuts down.

## Types of Spring Container

Two main implementation interfaces for IOC container

- Bean Factory
- Application Context

### Bean Factory (Basic Version)

Basic, low-level IOC container defined in `org.springframework.beans.factory.BeanFactory`

Simplest, most lightweight container providing fundamental configuration capabilities.

Uses **Lazy Initialization**, creating beans on demand only when `getBean()` is called.

Supports basic dependency injection, bean scopes (singleton and prototype), and manual processor registration.

It is rarely used directly today.

It fits lightweight or memory-constrained environments.

### Application Context (Advanced Version)

Advanced container defined in `org.springframework.context` that **extends BeanFactory**.

Feature-rich enterprise-grade container

Uses **Eager Initialization** by default, pre-instantiating all singleton beans at startup to catch configuration issues early.

Adds annotation support, Aspect Oriented Programming, internationalization, event propagation & web-specific scopes (request & session).

It is the standard choice for modern Spring & Spring Boot applications.

**Highly recommended & used by default in all modern application & Spring Boot.**

## Common Application Context Implementations

- `AnnotationConfigApplicationContext` : used for modern Java-based configuration (class annotated with `@Configuration`, `@ComponentScan`)
- `ClassPathXmlApplicationContext` : loads an XML configuration file located anywhere inside project's classpath
- `FileSystemXmlApplicationContext` : loads an XML configuration file from specific absolute file system directory path.

## Code Example : Container in Action

```java
// Defining Meta data
@Configuration
public class AppConfig
{
    @Bean
    public MyService myService()
    {   return new MyServiceImpl();
    }
}
```

```java
public class MainApp
{
    public static void main(String[] args)
    {
        // Instantiating container
        AnnotationConfigApplicationContext context =
            new AnnotationConfigApplicationContext(AppConfig.class);
        // retrieving managed bean
        MyService service = context.getBean(MyService.class);
        service.executeTask();
        context.close(); // closing container triggers
                         // destruction callbacks.
    }
}
```
