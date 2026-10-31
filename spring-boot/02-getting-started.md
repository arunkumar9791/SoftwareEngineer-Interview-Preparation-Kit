# Getting Started

1. **Project Initialization**

   Generate project structure using Spring Initializr or an IDE like IntelliJ IDEA

2. **Dependencies**

   Choose your build tool (Maven or Gradle) & add required modules such as Spring Web or Spring Data JPA

3. **Application Entry Point**

   Write your main class annotated with `@SpringBootApplication` and run `SpringApplication.run()`

## Let's create a simple Spring Boot Application

- Create a project using Spring Initializr: start.spring.io
- Choose Java, Maven & suitable stable Spring Boot version. Add Spring Web starter if you want to build web application.

**Main Application class**

```java
@SpringBootApplication
public class DemoApplication
{
    public static void main(String[] args)
    {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## Let's understand the @SpringBootApplication

`@SpringBootApplication` combines three important annotations.

- → `@SpringBootConfiguration` - identifies the class as a source of Spring configuration.
- → `@EnableAutoConfiguration` - enables Spring Boot's auto configuration mechanism.
- → `@ComponentScan` - enables scanning for Spring components in the package & its subpackages by default.

`SpringApplication.run(...)` - This starts the Spring application. It creates & refreshes the Spring application context, for web application, starts the embedded web server when configured.

## Add a REST API

Let's create a simple endpoint that returns a message.

```java
@RestController
public class HelloController
{
    @GetMapping("/hello")
    public String sayHello()
    {
        return "Hello, Spring Boot!";
    }
}
```

## Run Demo Application

Run `DemoApplication` and visit `http://localhost:8080/hello`

**Response**

```
Hello, Spring Boot!
```

Here's what happens:

```
              Browser
             GET /hello
                 ↓
      Embedded Tomcat Server
   (Receives the HTTP request)
                 ↓
          HelloController
            sayHello()
                 ↓
              Response
        Hello, Spring Boot!
```
