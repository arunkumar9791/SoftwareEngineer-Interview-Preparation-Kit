# Spring Profiles

Spring Profiles are used to configure an application differently for different environments.

### Why do we need Profiles?

An application may run in different environments.

```
- DEV  - Development Env  ┐  Each requirement may
- Test - Testing Env      ├  require different database
- Prod - Production Env   ┘  URLs, server port, beans
```

**application-dev.properties**

```properties
spring.datasource.url=jdbc:mysql://localhost:3036/devDB
server.port=8081
```

**application-prod.properties**

```properties
spring.datasource.url=jdbc:mysql://prod-server:3036/prodDB
server.port=8080
```

### Activating a Profile

Using application.properties :

```properties
spring.profiles.active=dev
```

Using command-line

```
java -jar app.jar --spring.profiles.active=prod
```

## @Profile Annotation

The `@Profile` annotation is used to register a bean only when a specific profile is active.

```java
@Component
@Profile("dev")
public class DevService
{   // Development-specific logic
}
```

```java
@Component
@Profile("prod")
public class ProdService
{   // Production-specific logic
}
```

### Key Points

- Profiles support environment-specific configuration.
- `application-dev.properties` - contains development configuration
- `application-prod.properties` - contains production configuration
- `@Profile` - controls which beans are registered.
- The active profile determines which profile-specific configuration is applied.
