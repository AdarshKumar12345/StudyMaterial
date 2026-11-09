# Spring Boot 3 Architecture, REST APIs & Actuator

## 1. Introduction & Prerequisites
Spring Boot 3 opinionates the Spring ecosystem by providing convention-over-configuration, auto-configured dependencies via Starters, embedded servlet containers (Tomcat, Jetty), and production monitoring endpoints via Actuator.

- **Prerequisites:** Spring Core, HTTP/REST basics, Maven.
- **Estimated Study Time:** 20 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Understand `@SpringBootApplication` and auto-configuration mechanics (`@ConditionalOnClass`, `@ConditionalOnMissingBean`).
- Build production REST APIs adhering to RESTful conventions (HTTP verbs, idempotency, status codes).
- Validate inputs using Jakarta Bean Validation (`@Valid`, `@NotNull`, `@Size`, `@Email`).
- Implement centralized exception handling using `@RestControllerAdvice` and RFC 7807 Problem Details.
- Monitor production health, metrics, and thread dumps with Spring Boot Actuator.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Anatomy of `@SpringBootApplication`: `@Configuration`, `@EnableAutoConfiguration`, `@ComponentScan`
- [ ] Auto-Configuration discovery via `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`
- [ ] Conditional Annotations: `@ConditionalOnProperty`, `@ConditionalOnClass`, `@ConditionalOnMissingBean`
- [ ] Externalized Configuration: `application.yml`, Profiles, and Environment Variables
- [ ] `@RestController` vs `@Controller` + `@ResponseBody`
- [ ] HTTP Request Mapping: `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`
- [ ] Request Binding: `@RequestBody`, `@PathVariable`, `@RequestParam`, `@RequestHeader`
- [ ] Jakarta Validation: `@NotBlank`, `@Min`, `@Max`, `@Pattern`, custom validators
- [ ] Global Error Handling: `@RestControllerAdvice`, `ResponseEntityExceptionHandler`, ProblemDetails
- [ ] Spring Boot Actuator: `/actuator/health`, `/actuator/metrics`, `/actuator/prometheus`

---

## 4. Free Documentation Resources
- **Spring Boot 3 Official Reference Documentation:** [https://docs.spring.io/spring-boot/docs/current/reference/html/](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- **Spring Guides — Building a RESTful Web Service:** [https://spring.io/guides/gs/rest-service/](https://spring.io/guides/gs/rest-service/)
- **Baeldung — Spring Boot Tutorials:** [https://www.baeldung.com/spring-boot](https://www.baeldung.com/spring-boot)

---

## 5. Free Video Courses & Tutorials
- **Amigoscode:** Spring Boot 3 Full Course
- **Java Brains:** Spring Boot Quick Start
- **Dan Vega:** Spring Boot 3 Features and Best Practices

---

## 6. Important Concepts Explained

### Auto-Configuration Evaluation Lifecycle
When Spring Boot bootstraps, it scans registered auto-configuration classes and evaluates conditional expressions before creating beans:

```
Bootstrap Application
         │
         ▼
Scan AutoConfiguration.imports
         │
         ▼
Evaluate @Conditional Annotations:
  ├── @ConditionalOnClass(DataSource.class) -> Present? YES
  ├── @ConditionalOnMissingBean(DataSource.class) -> Custom DataSource exists? NO
  └── @ConditionalOnProperty(name = "spring.datasource.url") -> Configured? YES
         │
         ▼
Auto-configure HikariCP DataSource Bean automatically!
```

---

## 7. Practical Code Examples

### Production REST Controller with Validation and RFC 7807 Error Handling
```java
package com.roadmap.springboot.api;

import jakarta.validation.Valid;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import org.springframework.http.HttpStatus;
import org.springframework.http.ProblemDetail;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.*;

import java.net.URI;
import java.util.UUID;

// DTO with Jakarta Validation
public record CreateUserRequest(
    @NotBlank(message = "Username is required") String username,
    @Email(message = "Invalid email format") String email
) {}

public record UserResponse(String id, String username, String email) {}

@RestController
@RequestMapping("/api/v1/users")
public class UserController {

    @PostMapping
    public ResponseEntity<UserResponse> createUser(@Valid @RequestBody CreateUserRequest request) {
        String newId = UUID.randomUUID().toString();
        UserResponse response = new UserResponse(newId, request.username(), request.email());
        return ResponseEntity
                .created(URI.create("/api/v1/users/" + newId))
                .body(response);
    }
}

// Global Exception Handler generating RFC 7807 Problem Details
@RestControllerAdvice
class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ProblemDetail handleValidationErrors(MethodArgumentNotValidException ex) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
            HttpStatus.BAD_REQUEST, "Validation failed for one or more fields");
        problem.setTitle("Invalid Request Content");
        problem.setType(URI.create("https://api.roadmap.com/errors/validation"));

        var fieldErrors = ex.getBindingResult().getFieldErrors().stream()
            .map(err -> err.getField() + ": " + err.getDefaultMessage())
            .toList();
        problem.setProperty("invalidFields", fieldErrors);
        return problem;
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a CRUD REST API for a Product Catalog with pagination (`Pageable`), sorting (`Sort`), and search filters.
2. **Assignment 2:** Configure Spring Boot Actuator with custom health indicators that verify third-party payment gateway connectivity.

---

## 9. Mini-Projects
- **Customer Feedback API with OpenAPI/Swagger:** Build a RESTful feedback ingestion service complete with input validation, RFC 7807 problem details, Springdoc OpenAPI documentation, and Actuator metrics.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the purpose of Spring Boot Starters?
**Answer:** Starters are dependency descriptors that bundle related libraries and transitive dependencies into a single artifact. For example, `spring-boot-starter-web` automatically pulls in Spring MVC, Jackson (JSON parser), Hibernate Validator, and embedded Apache Tomcat. This eliminates dependency version conflicts and reduces manual POM configuration.

### Q2: What is the RFC 7807 Problem Details standard in Spring Boot 3?
**Answer:** RFC 7807 is an IETF standard defining a standardized JSON format for HTTP API error responses. Prior to Spring Boot 3, developers crafted custom error JSON structures. Spring 6 / Spring Boot 3 natively introduces `ProblemDetail`, which includes standardized fields: `type` (URI identifier), `title`, `status`, `detail`, `instance`, and arbitrary custom properties, providing consistent machine-readable error contracts.

---

## 11. Revision Notes
- Prefer YAML (`application.yml`) over `.properties` for hierarchical configs.
- Use `@ConfigurationProperties` for type-safe configuration binding.
- Never expose database entities directly through `@RestController`; always use DTOs.

---

## 12. Topic Completion Checklist
- [ ] Understand auto-configuration mechanics
- [ ] Built REST endpoints with `@Valid` and DTOs
- [ ] Configured RFC 7807 Problem Details
- [ ] Enabled and secured Spring Boot Actuator
