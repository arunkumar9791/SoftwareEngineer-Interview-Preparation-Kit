# Stereotypes in Spring

In Spring, stereotype annotations are used to tell Spring that a Java class should be managed by the Spring container as a bean.

### Types of Stereotype Annotation

| Annotation | Use |
|---|---|
| `@Component` | general purpose annotation |
| `@Service` | used for classes that contain business logic |
| `@Repository` | used for classes that handle DB operation |
| `@Controller` | used for classes that handle web request in Spring MVC. |

### @Component

Base Annotation

The generic base annotation. It tells the Spring IOC container to automatically detect the class during classpath scanning & register it as bean.

All other specialized stereotype annotation `@Service`, `@Repository`, `@Controller` are specialized forms of `@Component` (Meta annotated)

### @Bean

Placed on method inside `@Configuration` class to specify that the return value should be registered as a bean.
