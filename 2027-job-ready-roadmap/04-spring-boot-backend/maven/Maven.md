# Apache Maven – Important Topics & Learning Resources

## 1. Maven Basics
- [ ] What is Maven and why use a build tool
- [ ] Installing Maven and `mvn -v`
- [ ] Standard Directory Layout (`src/main/java`, `src/test/java`, `resources`)
- [ ] Project Object Model (pom.xml)
- [ ] Coordinates: groupId, artifactId, version, packaging
- [ ] Creating a project with archetypes / Spring Initializr

## 2. Build Lifecycle
- [ ] Default, Clean and Site lifecycles
- [ ] Phases: validate, compile, test, package, verify, install, deploy
- [ ] Goals vs Phases
- [ ] Common commands: `mvn clean install`, `mvn test`, `mvn package -DskipTests`

## 3. Dependency Management
- [ ] Declaring dependencies
- [ ] Dependency scopes (compile, provided, runtime, test)
- [ ] Transitive dependencies
- [ ] Dependency conflicts and mediation (nearest wins)
- [ ] Exclusions
- [ ] `dependencyManagement` and BOMs (`spring-boot-dependencies`)
- [ ] `mvn dependency:tree`
- [ ] Properties and version variables

## 4. Repositories
- [ ] Local (~/.m2), Central and Remote repositories
- [ ] `settings.xml`
- [ ] Private repositories (Nexus, Artifactory, GitHub Packages)

## 5. Plugins & Advanced Topics
- [ ] maven-compiler-plugin (Java version)
- [ ] spring-boot-maven-plugin (executable jar)
- [ ] maven-surefire / failsafe (unit vs integration tests)
- [ ] Profiles (dev, test, prod)
- [ ] Multi-module projects, parent POM and inheritance
- [ ] Maven Wrapper (`mvnw`)
- [ ] Maven vs Gradle

## 6. Practical Tasks
- [ ] Create a multi-module project (api, service, common)
- [ ] Resolve a dependency version conflict using `dependency:tree`
- [ ] Add a profile that switches the Spring active profile
- [ ] Build an executable Spring Boot jar and run it

## Important Learning Resources
- [Maven – Getting Started Guide](https://maven.apache.org/guides/getting-started/)
- [Maven – Introduction to the Build Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
- [Maven – Dependency Mechanism](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)
- [Baeldung – Maven Tutorial](https://www.baeldung.com/maven)
- [Spring Initializr](https://start.spring.io/)
