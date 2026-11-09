# Spring Boot 3 REST APIs – Important Topics & Learning Resources

## 1. Spring Boot Basics
- [ ] What is Spring Boot and why use it
- [ ] Spring Initializr and project structure
- [ ] Starters (`spring-boot-starter-web`, etc.)
- [ ] `@SpringBootApplication` anatomy
- [ ] Auto-configuration and `@Conditional*` annotations
- [ ] Embedded servers (Tomcat, Jetty, Undertow)
- [ ] DevTools

## 2. Configuration
- [ ] `application.properties` vs `application.yml`
- [ ] Profiles (`application-dev.yml`)
- [ ] Environment variables and command-line args
- [ ] `@ConfigurationProperties`
- [ ] Externalized configuration order
- [ ] Secrets handling

## 3. REST API Development
- [ ] REST principles and HTTP methods
- [ ] Status codes and idempotency
- [ ] `@RestController`, `@RequestMapping`
- [ ] `@GetMapping`, `@PostMapping`, `@PutMapping`, `@PatchMapping`, `@DeleteMapping`
- [ ] `@PathVariable`, `@RequestParam`, `@RequestBody`, `@RequestHeader`
- [ ] `ResponseEntity`
- [ ] DTOs and Java Records
- [ ] Mapping with MapStruct / ModelMapper
- [ ] Pagination and sorting (`Pageable`)
- [ ] API versioning

## 4. Validation & Error Handling
- [ ] Jakarta Bean Validation (`@Valid`, `@NotNull`, `@NotBlank`, `@Size`, `@Email`)
- [ ] Custom validators
- [ ] `@RestControllerAdvice` and `@ExceptionHandler`
- [ ] RFC 7807 `ProblemDetail`
- [ ] Custom exceptions

## 5. Production Readiness
- [ ] Spring Boot Actuator (health, metrics, info)
- [ ] Custom health indicators
- [ ] Logging (SLF4J, Logback) and MDC
- [ ] OpenAPI / Swagger (springdoc)
- [ ] CORS configuration
- [ ] Testing: JUnit 5, Mockito, `@WebMvcTest`, `@SpringBootTest`, Testcontainers
- [ ] Dockerizing a Spring Boot app
- [ ] `RestClient` / `WebClient` for calling other services

## 6. Practical Tasks
- [ ] Product Catalog CRUD API with pagination and filters
- [ ] Global exception handler returning ProblemDetail
- [ ] Add Swagger UI and Actuator
- [ ] Write controller tests with `MockMvc`

## Important Learning Resources
- [Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/reference/)
- [Spring Guide – Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
- [Baeldung – Spring Boot](https://www.baeldung.com/spring-boot)
- [Baeldung – REST with Spring](https://www.baeldung.com/rest-with-spring-series)
- [springdoc-openapi](https://springdoc.org/)
