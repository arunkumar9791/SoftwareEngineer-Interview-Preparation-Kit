# Using a Spring Boot + Maven project as a dependency in another project

Let's understand this step by step with a simple real-world example.

Imagine you have created a Spring Boot project that contains reusable code, and you want to use that code in multiple Spring Boot applications without copying the same code into every project.

For example:

- Project A — `common-library`: Contains reusable Java code, such as a calculator or a common service.
- Project B — `employee-app`: Needs to use the code from Project A.

Instead of copying the Java files from Project A into Project B, you can package Project A as a JAR file and add it as a Maven dependency.

## 1. Understand the concept

```
Project A — common-library
(Reusable Java classes)
        ↓  Run mvn package
common-library-1.0.0.jar
(Packaged reusable code)
        ↓  Install to Maven local repository
Local Maven repository
(Usually ~/.m2/repository)
        ↓
Project B — employee-app
(Adds Project A using a Maven dependency)
```

The key idea is:

Project A produces a JAR → Maven stores it → Project B declares it as a dependency → Project B can use its classes.

## 2. Step 1 — Create Project A (`common-library`)

Create a Spring Boot project using Maven with these details:

| Setting | Value |
|---|---|
| Project name | `common-library` |
| Group ID | `com.example` |
| Artifact ID | `common-library` |
| Version | `1.0.0` |
| Packaging | `jar` |

Your project structure will look like this:

```
common-library/
 ├── pom.xml
 └── src/
     └── main/
         └── java/
             └── com/
                 └── example/
                     └── common/
                         └── GreetingService.java
```

### Create a reusable Java class

Inside `GreetingService.java`, write the following code:

```java
package com.example.common;

public class GreetingService {

    public String greet(String name) {
        return "Hello, " + name + "!";
    }
}
```

This class contains reusable functionality. Any other Java application that has access to this class can call its `greet()` method.

For example, the output for `greet("Arun")` will be:

```
Hello, Arun!
```

## 3. Step 2 — Configure Project A's `pom.xml`

Your Maven configuration must identify the library and its version.

Here is a simplified example using the Spring Boot parent:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.6</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>common-library</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>common-library</name>

</project>
```

The Spring Boot version above is an example; use a version compatible with your project.

Notice these three values:

```xml
<groupId>com.example</groupId>
<artifactId>common-library</artifactId>
<version>1.0.0</version>
```

Together, they identify your library in Maven. They are called its Maven coordinates.

One important point: if Spring Boot's Maven plugin is configured to repackage Project A as an executable Spring Boot JAR, disable that repackaging for a regular library. Otherwise, another project may not be able to use the JAR as an ordinary dependency. A reusable library normally needs a standard JAR containing its compiled classes.

## 4. Step 3 — Build and install Project A

Open a terminal in the `common-library` project directory.

Run:

```bash
mvn clean install
```

Let's break down this command:

| Command | Meaning |
|---|---|
| `mvn` | Executes Maven |
| `clean` | Removes the previous build output |
| `install` | Compiles, tests, packages and installs the artifact in the local Maven repository |

Why use `install` instead of just `package`?

- `mvn package` creates the JAR, normally under the project's `target/` directory.
- `mvn install` does that and also installs the JAR and its Maven metadata into your local repository, so another local project can resolve it as a dependency.

After a successful build, Maven will typically store the JAR here:

```
~/.m2/repository/
    com/
      example/
        common-library/
          1.0.0/
            common-library-1.0.0.jar
            common-library-1.0.0.pom
```

On Windows, `~/.m2/repository` is usually under your user profile directory, such as `C:\Users\<username>\.m2\repository`.

You don't need to manually copy the JAR into Project B. Maven will locate it in the local repository using its coordinates.

## 5. Step 4 — Create Project B (`employee-app`)

Now create a second Spring Boot Maven project.

```
employee-app/
 ├── pom.xml
 └── src/
     └── main/
         └── java/
             └── com/
                 └── example/
                     └── employee/
                         └── EmployeeApplication.java
```

Project B wants to use the `GreetingService` class from Project A.

To make this possible, add Project A to Project B's `pom.xml`.

### Add the dependency

Inside Project B's `pom.xml`, add the following within the `<dependencies>` section:

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>common-library</artifactId>
    <version>1.0.0</version>
</dependency>
```

These coordinates must match those used in Project A.

If Project B already has dependencies, simply add this as another dependency; don't replace the existing ones.

### What happens when you add this dependency?

Maven reads the dependency declaration and searches for the matching artifact:

1. It looks in the local Maven repository.
2. It finds `common-library-1.0.0.jar`.
3. It adds that JAR to Project B's compile and runtime classpaths.
4. Project B can now import and use the classes inside the library.

## 6. Step 5 — Use Project A's class in Project B

Inside `EmployeeApplication.java`, write:

```java
package com.example.employee;

import com.example.common.GreetingService;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class EmployeeApplication {

    public static void main(String[] args) {

        SpringApplication.run(
            EmployeeApplication.class, args
        );

        GreetingService service = new GreetingService();

        String message = service.greet("Arun");

        System.out.println(message);
    }
}
```

Notice this import:

```java
import com.example.common.GreetingService;
```

The `GreetingService` source code exists in Project A, not Project B.

But because Project A is declared as a Maven dependency, Project B can import and use that class without copying its source file.

### Expected output

```
Hello, Arun!
```

You have successfully reused Project A's code in Project B.

## 7. Step 6 — Run Project B

Open a terminal in the `employee-app` directory and run:

```bash
mvn spring-boot:run
```

Alternatively, package Project B and run its executable JAR:

```bash
mvn clean package
java -jar target/employee-app-0.0.1-SNAPSHOT.jar
```

The exact JAR filename depends on your Project B configuration.

Both approaches use the `common-library` dependency when compiling and running the application.

## 8. Understand the complete workflow

```
Step 1: Develop Project A
(Write reusable Java classes in common-library)
        ↓
Step 2: Build and install
(Run mvn clean install in Project A to install the library locally)
        ↓
Step 3: Declare the dependency
(Add Project A's group ID, artifact ID and version to Project B's pom.xml)
        ↓
Step 4: Use the library
(Import its classes, compile Project B and run the application)
```

## 9. What if you want to use the library in another computer?

The approach above works when both projects can access the same local Maven repository.

If you want multiple developers or build servers to use your library, publish it to a Maven repository, such as Nexus, Artifactory or GitHub Packages.

| Command | Purpose |
|---|---|
| `mvn package` | Builds the JAR |
| `mvn install` | Builds the JAR and installs it locally |
| `mvn deploy` | Publishes the artifact to a configured remote Maven repository |

A remote repository allows other projects and computers to download the dependency through Maven.

## 10. One important Spring Boot consideration

The example uses a plain Java class, so no additional Spring configuration is required.

If Project A contains Spring components such as `@Service`, `@Component` or `@Repository`, adding the JAR as a dependency makes those classes available, but does not automatically guarantee that Spring will discover and register them as beans.

You may need to configure component scanning or, for a reusable Spring Boot library, create an auto-configuration class.

Remember this key point: Maven dependencies are not limited to third-party libraries. You can package your own project as a JAR, install or publish it, and reuse it in any compatible Maven project.
