# Spring Boot Starter dependencies

A starter dependency is a convenient package of related libraries that work together for a particular purpose.

Instead of manually adding many individual libraries, you add one starter and Maven resolves its required dependencies.

| Starter | Use |
|---|---|
| `spring-boot-starter-web` | For REST API & web applications. Includes Spring MVC & an embedded Servlet Server |
| `spring-boot-starter-data-jpa` | For database operation - JPA & Hibernate |
| `spring-boot-starter-security` | authentication, authorization & security configuration |
| `spring-boot-starter-test` | unit & integration testing with common testing libraries |
| `spring-boot-starter-actuator` | Monitoring & application health information. |

## Other useful dependencies

| Dependency | Use |
|---|---|
| `spring-boot-starter-validation` | Validate request data using annotations such as `@NotBlank` |
| `spring-boot-starter-jdbc` | work with database using JDBC |
| `mysql-connector-j` | Connect to a MySQL database |
| `postgresql` | Connect to PostgreSQL |
| `lombok` | Reduce repetitive Java code |
| `spring-boot-devtools` | Helpful development-time features such as automatic restarts |

## Transitive dependency in Maven

A transitive dependency means a dependency that your project gets automatically because another dependency needs it.

You added `spring-boot-starter-web`. Your application also needs other libraries to provide web functionality.

Spring MVC & Tomcat are examples of libraries that can be brought into your project through starter's dependency tree.

You don't have to manually add every required library.

```
                     Your Project
                       pom.xml
                          ↓
                  Direct Dependency
               spring-boot-starter-web
                          ↓
     Maven automatically resolves its dependency
                          ↓
 ┌───────────────────────┐   ┌───────────────────────┐
 │      Spring MVC       │   │    Embedded Tomcat    │
 │ Handles web Request   │   │  Runs the web server  │
 └───────────────────────┘   └───────────────────────┘
```

To see transitive dependency, run

```
mvn dependency:tree
```
