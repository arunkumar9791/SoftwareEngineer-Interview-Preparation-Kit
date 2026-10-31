# Spring Boot

Spring Boot is a framework built on top of the Spring Framework that makes it easier and faster to create standalone, production-grade applications.

## Why Spring Boot?

Before Spring Boot, developers have to do more manual configuration to set up a Spring application.

We might need to

- configure Spring dependencies
- Set up a web server
- Configure application components
- Manage compatible library versions
- Configure how application starts.

Spring Boot reduces this setup work.

## Three main features of Spring Boot

- Auto-Configuration
- Starter dependencies
- Embedded server

## Other features

- No XML configuration
- Production-Ready Tools.

### Auto-Configuration

Spring Boot automatically configures many components based on the libraries you include and the configuration you provide.

(Eg:- Add Spring MVC dependencies, Spring Boot configures much of web application infrastructure)

### Starter dependencies

Starters provide convenient groups of compatible dependencies for common tasks.

(Eg: `spring-boot-starter-web` brings in the dependencies needed for typical web-application, including Spring MVC & embedded server)

### Embedded Server

You can run many Spring Boot web applications directly without separately installing & configuring web server.

(Eg: A typical web application can start with an embedded Tomcat server when you run its main method)

### No XML configuration

Eliminates the need for XML setup files or code generation.

### Production-Ready Tools

Offers built-in metrics, health checks and externalized configuration via Spring Boot Actuator.
