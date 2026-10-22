# Property Source

A Property Source is a source from which Spring reads configuration values such as database URLs, usernames, ports & application setting.

Common property sources

- application.properties
- application.yml or application.yaml
- Environment variables
- Command-line arguments.

**application.properties**

```properties
app.name=MySpringApp
app.version=1.0
```

### Read properties using @Value

```java
@Component
public class AppInfo
{
    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String appVersion;
}
```

| | |
|---|---|
| `@Value` | Injects an individual property value |
| `@ConfigurationProperties` | Binds a group of related properties to Java Object. |
| `Environment` | Provides programmatic access to properties. |

## Property Precedence

Property Precedence determines which value Spring Boot uses when the same property is defined in multiple configuration sources.

In application.properties :

```properties
server.port=8080
```

Environment Variable

```
SERVER_PORT=9090
```

Command-line argument

```
java -jar app.jar --server.port=7070
```

In this example, the effective server port is 7070.

Precedence among these three sources (highest to lowest)

| Order | Source | Port |
|---|---|---|
| 1 | Command-line | 7070 |
| 2 | Environment Variables | 9090 |
| 3 | application.properties | 8080 |
