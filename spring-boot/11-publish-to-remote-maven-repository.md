# Publish your Spring Boot project to a remote Maven repository

In the previous example, you used `mvn install` to make `common-library` available to other projects on your own computer.

Now, imagine you want your teammates to use that library from their computers. You need to publish the JAR to a remote Maven repository.

For this example, we'll use a private repository such as Nexus Repository or JFrog Artifactory. The steps are similar for other Maven-compatible repositories.

## 1. Understand the difference

| Command | Repository | What it does |
|---|---|---|
| `mvn install` | Local repository | Stores your JAR in your computer's `.m2/repository` directory. Other projects on that computer can use it. |
| `mvn deploy` | Remote repository | Uploads your JAR and Maven metadata to a configured remote repository. Authorized developers and build servers can download it. |

The overall process is:

```
Project A — common-library
        ↓  mvn deploy
Remote Maven repository
(Nexus / Artifactory)
        ↓
Project B — employee-app
(Downloads the dependency using Maven)
```

## 2. Step 1 — Choose your remote repository

There are two common approaches:

| Repository | Best suited for |
|---|---|
| Nexus / Artifactory | Private libraries shared within a company or team |
| Maven Central | Public libraries that anyone can use |

For learning, I recommend starting with a private Nexus or Artifactory repository, because you can focus on the Maven deployment process without the additional requirements for publishing a public library.

If your company already has an internal Maven repository, use that rather than creating a new one.

## 3. Step 2 — Get the repository URL and credentials

Your repository administrator will provide:

- A deployment URL for releases.
- A deployment URL for snapshot versions, if supported.
- A username and password or access token.
- Permission to publish artifacts.

For example, we'll use these illustrative values:

```
Release URL:
https://repo.example.com/repository/maven-releases/

Repository ID:
company-releases
```

These are placeholders, not a real repository you can upload to.

## 4. Step 3 — Configure `pom.xml` in Project A

Add a `<distributionManagement>` section to your `common-library` project's `pom.xml`.

```xml
<distributionManagement>
    <repository>
        <id>company-releases</id>
        <url>
            https://repo.example.com/repository/maven-releases/
        </url>
    </repository>

    <snapshotRepository>
        <id>company-snapshots</id>
        <url>
            https://repo.example.com/repository/maven-snapshots/
        </url>
    </snapshotRepository>
</distributionManagement>
```

What do these settings mean?

- `<repository>` tells Maven where to upload release versions, such as `1.0.0`.
- `<snapshotRepository>` tells Maven where to upload development versions, such as `1.0.1-SNAPSHOT`.
- `<id>` identifies the repository configuration and connects it to the corresponding credentials in `settings.xml`.

The URLs and IDs must match the repositories configured on your server.

## 5. Step 4 — Configure credentials in `settings.xml`

Maven keeps repository credentials separately from your project's `pom.xml`. This avoids putting passwords directly into the source code that you commit to Git.

Open or create Maven's `settings.xml`. Its usual location is:

```
~/.m2/settings.xml
```

Add the following under the root `<settings>` element:

```xml
<settings>
    <servers>
        <server>
            <id>company-releases</id>
            <username>YOUR_USERNAME</username>
            <password>YOUR_ACCESS_TOKEN</password>
        </server>

        <server>
            <id>company-snapshots</id>
            <username>YOUR_USERNAME</username>
            <password>YOUR_ACCESS_TOKEN</password>
        </server>
    </servers>
</settings>
```

Replace the placeholders with credentials provided by your repository administrator. The server IDs must match the IDs in `distributionManagement`.

Security tip: Don't commit this file or your real credentials to Git. Prefer your company's approved credential-management approach, and use Maven's password encryption or environment-variable-based configuration where appropriate.

## 6. Step 5 — Publish Project A

Open the terminal in your `common-library` project directory and execute:

```bash
mvn clean deploy
```

Maven performs these steps:

1. Cleans previous build output.
2. Compiles your Java code.
3. Runs tests.
4. Packages the JAR.
5. Uploads the JAR, POM and associated artifacts to the configured remote repository.

This is the standard Maven deployment lifecycle.

After a successful deployment, your repository should contain an artifact identified by:

```
com.example:common-library:1.0.0
```

You can now share these coordinates with other developers.

## 7. Step 6 — Use the published library in Project B

In `employee-app/pom.xml`, declare the dependency just as you did previously:

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>common-library</artifactId>
    <version>1.0.0</version>
</dependency>
```

If the remote repository is private, configure Project B's Maven settings or repository configuration so Maven can download the artifact from it. For example, your company's repository manager might already be configured in the shared Maven `settings.xml`.

Then run:

```bash
mvn clean package
```

Maven resolves the dependency from the remote repository when it isn't already available locally, and Project B can use `GreetingService` as before.

## 8. Common mistakes to avoid

| Problem | What to check |
|---|---|
| `401 Unauthorized` or `403 Forbidden` | Credentials and deployment permissions |
| `No connector available` or connection errors | Repository URL, HTTPS and server configuration |
| Dependency cannot be resolved | Correct coordinates and repository access in Project B |
| Version already exists | Publish a new version rather than overwriting a release |
| Spring components aren't detected | Configure component scanning or Spring Boot auto-configuration where needed |

## One final distinction: private repository vs. Maven Central

If by "remote Maven repository" you specifically mean Maven Central, the process is different from deploying to a typical Nexus or Artifactory repository. Central has additional requirements, including namespace verification, artifact metadata and signed release artifacts; its current publishing workflow uses the Sonatype Central Publisher Portal. See the [official Maven Central publishing guide](https://central.sonatype.org/publish/publish-portal-maven/).
