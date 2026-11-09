# Apache Maven & Enterprise Build Lifecycles

## 1. Introduction & Prerequisites
Apache Maven is a build automation and project management tool based on the Project Object Model (POM). It standardizes the build process, dependency resolution, version locking, and plugin execution for enterprise Java applications.

- **Prerequisites:** Java Fundamentals.
- **Estimated Study Time:** 6 hours
- **Difficulty:** Beginner

---

## 2. Detailed Learning Objectives
- Understand the 3 built-in Maven Lifecycles: Default, Clean, and Site.
- Master Dependency Scopes (`compile`, `provided`, `runtime`, `test`, `system`).
- Understand Transitive Dependency resolution and conflict rules (nearest definition wins).
- Use `dependencyManagement` and Bill of Materials (BOM) for centralized multi-module versioning.
- Configure essential Maven plugins: Compiler, Surefire, Failsafe, and Spring Boot Maven Plugin.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] POM Anatomy (`groupId`, `artifactId`, `version`, `packaging`)
- [ ] Maven Coordinates and Local/Remote Repositories (`~/.m2/repository`)
- [ ] Standard Maven Lifecycles: `validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`
- [ ] Dependency Scopes and Classpath Inclusion
- [ ] Transitive Dependencies & Dependency Tree Inspection (`mvn dependency:tree`)
- [ ] Excluding Transitive Dependencies (`<exclusions>`)
- [ ] Centralizing Versions via `<dependencyManagement>` and BOM Imports
- [ ] Maven Build Profiles (`<profiles>`) for Dev, QA, and Production
- [ ] Plugin Execution & Binding Phases (`maven-surefire-plugin`, `maven-failsafe-plugin`)
- [ ] Multi-Module Maven Projects (`<modules>`)

---

## 4. Free Documentation Resources
- **Apache Maven Official Documentation:** [https://maven.apache.org/guides/index.html](https://maven.apache.org/guides/index.html)
- **Baeldung — Guide to Maven:** [https://www.baeldung.com/maven](https://www.baeldung.com/maven)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Maven Tutorial for Beginners
- **Amigoscode:** Maven Crash Course

---

## 6. Important Concepts Explained

### Maven Dependency Scopes & Classpath Availability
```
┌───────────┬─────────────┬──────────────┬──────────────┬─────────────┐
│ Scope     │ Compile CP  │ Test CP      │ Runtime CP   │ Packaged?   │
├───────────┼─────────────┼──────────────┼──────────────┼─────────────┤
│ compile   │ Yes         │ Yes          │ Yes          │ Yes (JAR)   │
│ provided  │ Yes         │ Yes          │ No           │ No (Server) │
│ runtime   │ No          │ Yes          │ Yes          │ Yes         │
│ test      │ No          │ Yes          │ No           │ No          │
└───────────┴─────────────┴──────────────┴──────────────┴─────────────┘
```

---

## 7. Practical Code Examples

### Standard Enterprise `pom.xml` with Spring Boot BOM
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.roadmap</groupId>
    <artifactId>backend-service</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <java.version>21</java.version>
        <spring.boot.version>3.2.4</spring.boot.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- Spring Boot BOM import: eliminates explicit version tags on starters -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring.boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

---

## 8. Practice Assignments
1. **Assignment 1:** Run `mvn dependency:tree` on a sample Spring Boot project and identify any duplicated or conflicting transitive dependencies.
2. **Assignment 2:** Create a multi-module Maven project with `core-domain`, `data-access`, and `web-api` submodules.

---

## 9. Mini-Projects
- **Standardized Multi-Environment Build Archetype:** Configure build profiles (`dev`, `prod`) that inject environment-specific configuration files during the `package` phase.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between `<dependencies>` and `<dependencyManagement>` in Maven?
**Answer:** `<dependencies>` immediately downloads and adds the specified libraries to the current module's classpath. `<dependencyManagement>` specifies versions, configurations, and exclusions for dependencies without actually adding them to the classpath. Submodules or downstream POMs can then declare the dependency by `groupId` and `artifactId` without specifying a version, ensuring uniform dependency versions across a microservices repository.

### Q2: How does Maven resolve transitive dependency version conflicts?
**Answer:** Maven uses the "nearest definition" rule in the dependency tree. If dependency A depends on C (version 1.0) via path `Project -> A -> C`, and dependency B depends on C (version 2.0) via path `Project -> B -> D -> C`, Maven picks version 1.0 because the path through A is shorter. If both paths have equal depth, the first declared in the POM takes precedence.

---

## 11. Revision Notes
- Use `mvn clean verify` for full test execution and packaging.
- Use `mvn dependency:analyze` to discover unused declared dependencies.
- Never hardcode absolute file paths in `pom.xml`.

---

## 12. Topic Completion Checklist
- [ ] Understand Maven lifecycles and phases
- [ ] Configured dependencyManagement with a BOM
- [ ] Inspected dependencies using `mvn dependency:tree`
- [ ] Created a multi-module Maven build
