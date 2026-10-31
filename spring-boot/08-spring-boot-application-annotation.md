# @SpringBootApplication

`@SpringBootApplication` annotation is one of the most important annotations in Spring Boot.

It is usually placed on the main class of the Spring Boot application.

It is the combination of 3 important Spring annotations.

- `@SpringBootConfiguration`
- `@EnableAutoConfiguration`
- `@ComponentScan`

## @SpringBootConfiguration

(Marks the main configuration class for Spring Boot application)

It is the specialized version of Spring's `@Configuration` annotation.

This annotation identifies the class as a source of configuration for Spring application.

In simple, it tells Spring, "This class can define how my application should be configured."

A configuration class can define beans using `@Bean`

```java
@Configuration
public class AppConfig
{
    @Bean
    public String applicationName()
    {
        return "My Spring Boot App";
    }
}
```

Configuration classes tell Spring how to create & configure application objects.

## @EnableAutoConfiguration

`@EnableAutoConfiguration` helps Spring Boot set up the infrastructure your application needs, instead of making you configure every part manually.

Let's understand conceptually,

Imagine you are building a web application.

To run it, we need

- A web server
- Components to handle HTTP requests
- Components to convert Java objects into JSON

Without Spring Boot auto configuration we have to configure much of this infrastructure ourselves.

Spring Boot makes it easier by adding dependency in `pom.xml`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Now, when Spring Boot starts, `@EnableAutoConfiguration` allows it to examine the available libraries and configuration,

It can effectively determine,

- The web libraries are available
- This looks like a web application
- I should configure the appropriate web infrastructure
- I can set up an embedded Tomcat server if relevant conditions are met.

## @ComponentScan

`@ComponentScan` annotation tells Spring to search packages for classes that should be managed by Spring.

Common examples include classes annotated with

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`

If we want to scan components or services located outside of our main application package, we need to use `scanBasePackages`

```java
@SpringBootApplication(scanBasePackages = {"com.example.myapp", "com.external.lib"})
public class MyApplication
{
    public static void main(String[] args)
    {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

## Difference between three

| Annotation | Use |
|---|---|
| `@SpringBootConfiguration` | Identifies the class that provides application configuration. |
| `@EnableAutoConfiguration` | Allows Spring Boot to configure suitable infrastructure automatically based on dependencies & settings |
| `@ComponentScan` | Tells Spring to search for classes annotated with such as `@Component` and registers them as Spring Beans. |
