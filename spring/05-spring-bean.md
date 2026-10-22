# Spring Bean

Spring Bean is simply a Java Object that is created and managed by Spring Framework.

Java Object does not automatically become a Spring Bean just because it is a class.

You can declare class as Spring Bean by using two main approaches.

### 1. Stereotype Annotation (Class-level)

Marking a class with `@Component` instructs Spring classpath scanning to automatically detect & instantiate.

Specialized variants include

1. `@Service` (for business logic)
2. `@Repository` (for database access layers)
3. `@Controller` (for web endpoints)

### 2. @Bean Annotation (Method-level)

Used inside a `@Configuration` class.

The container executes the method and registers the returned object as a bean.

This is ideal for configuring third party libraries where you cannot modify source code to add `@Component`
