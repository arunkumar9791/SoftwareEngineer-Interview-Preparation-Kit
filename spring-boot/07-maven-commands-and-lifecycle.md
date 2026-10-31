# Maven Commands & Build Lifecycle

## Common Maven Commands

| Command | Use |
|---|---|
| `mvn clean` | Removes previous build output |
| `mvn compile` | Compiles main source code |
| `mvn test` | Runs test |
| `mvn package` | Builds project artifact |
| `mvn spring-boot:run` | Run the application during development |
| `mvn dependency:tree` | displays direct & transitive dependency |
| `mvn install` | Builds, tests the project, packages it & installs artifact to local Maven repository. |
| `mvn clean install` | Deletes the target directory & performs a full build, saving the artifact (JAR/WAR) to local repo |
| `mvn clean package -DskipTests` | Skip unit tests |
| `mvn clean install -X` | Debug mode to output full debug logs |

## Maven Build Lifecycle

Maven has predefined lifecycle phases.

When you run later phase, Maven executes earlier phases in that lifecycle first.

```
1. Validate (Checks project configuration)
      ↓
2. Compile  (Compiles main Java code)
      ↓
3. Test     (Run tests)
      ↓
4. Package  (Creates JAR or WAR)
      ↓
5. Verify   (Performs additional validation checks)
      ↓
6. Install  (Install artifacts locally)
      ↓
7. Deploy   (Publish artifact to remote repo)
```

If we execute `mvn install`, it doesn't install JAR. It performs preceding lifecycle phases, including compilation, testing & packaging.

## Run a packaged Spring Boot application

Run

```
mvn clean package
```

Look for the generated JAR inside `target/` folder.

You can then run it using

```
java -jar target/employee-management-0.0.1-SNAPSHOT.jar
```

Use `mvn deploy` when you want to publish your built artifact to a remote Maven repository.

## Difference between mvn package, install & deploy

| mvn package | mvn install | mvn deploy |
|---|---|---|
| Creates the build artifact | Creates build artifact & installs it locally | Creates the build artifact & publishes it remotely |
| usually produces JAR in `target/` | It does previous & also places artifact to local Maven repo | It does previous (install) & also publishes artifact to remote repo |
| Does not normally install the artifact into your local repo | Makes it available to other local Maven projects | Makes it available to other projects that can access the remote repo |
