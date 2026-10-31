# Maven

Maven is a build & dependency management tool.

**Eg:**

Suppose Spring Boot application needs

- Spring Web to build REST APIs.
- Spring Data JPA to interact with database.
- MySQL Driver to connect to MySQL
- Spring Boot Test to test your code.

**Without Maven** - we need to find & manage required JAR files & dependencies yourself.

**With Maven** - we declare what we need in `pom.xml`, Maven downloads & manages those libraries for us.

| | |
|---|---|
| Spring Boot | helps you build application. |
| Maven | Manages libraries & builds application. |
| `pom.xml` | tells Maven what project needs. |

## pom.xml

`pom.xml` stands for **Project Object Model**.

It is the main configuration file for Maven project.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project ...>
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.6</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>employee-management</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
        <!-- Build REST API's -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        ⋮
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

## Important parts of pom.xml

### 1. Parent

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.6</version>
</parent>
```

Spring Boot parent provides default Maven configuration & dependency version management.

When using a managed Spring Boot dependency, you usually don't need to specify its version separately.

### 2. Project coordinates

| | |
|---|---|
| `groupId` | Identifies the organization or project group |
| `artifactId` | Identifies the project or artifact |
| `version` | Identifies project current version. SNAPSHOT typically indicates development work |

### 3. Java Version

This configures Java 17 for the project when used with the Spring Boot parent. Your installed JDK must also be compatible.

### 4. Dependencies

`<dependencies>` section lists external libraries your application needs.

### 5. Build Plugin

Spring Boot Maven plugin helps package & run your application, including creating an executable JAR.
