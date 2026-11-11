4. JUnit 5
5. AssertJ
6. Mockito
7. Spring Boot Test Slices
8. Spring Security Testing
9. Testcontainers
10. REST Assured
11. WireMock
12. Awaitility
13. JaCoCo
14. Contract Testing (Pact / Spring Cloud Contract)
15. Mutation Testing (PIT)


# Java & Spring Boot Testing – Practical Notes

## 4. JUnit 5

### What is JUnit 5?

JUnit 5 is a Java testing framework used to write and execute automated tests.

**Use it for:** Testing service methods, utility functions, business logic, and exception handling.

### 4.1 Basic Unit Test

**Calculator.java**

    public class Calculator {
        public int add(int a, int b) {
            return a + b;
        }
    }

**CalculatorTest.java**

    import org.junit.jupiter.api.Test;
    import static org.junit.jupiter.api.Assertions.*;

    class CalculatorTest {

        @Test
        void shouldAddTwoNumbers() {
            Calculator calculator = new Calculator();
            int result = calculator.add(10, 20);
            assertEquals(30, result);
        }
    }

**Explanation:**
- `@Test` marks a method as a test.
- `assertEquals(expected, actual)` compares results.
- The test passes if the result is 30.

### 4.2 Important JUnit Annotations

| Annotation | Practical Use |
|---|---|
| `@Test` | Marks a test method |
| `@BeforeEach` | Runs before every test |
| `@AfterEach` | Runs after every test |
| `@BeforeAll` | Runs once before all tests |
| `@AfterAll` | Runs once after all tests |
| `@Disabled` | Skips a test |
| `@DisplayName` | Provides a readable test name |
| `@ParameterizedTest` | Runs the same test with different inputs |
| `@Nested` | Groups related tests |

### 4.3 Testing Exceptions

    @Test
    void shouldRejectInsufficientBalance() {
        BankAccount account = new BankAccount(500);

        assertThrows(
            IllegalArgumentException.class,
            () -> account.withdraw(1000)
        );
    }

**When to use:** Test invalid inputs, missing resources, duplicate records, and business-rule violations.

### 4.4 Parameterized Testing

    @ParameterizedTest
    @CsvSource({
        "2, 3, 5",
        "10, 20, 30",
        "-5, 5, 0"
    })
    void shouldAddNumbers(int a, int b, int expected) {
        Calculator calculator = new Calculator();
        assertEquals(expected, calculator.add(a, b));
    }

**Why useful?** One test method covers multiple scenarios.

### 4.5 Running JUnit Tests

    mvn test

Run a specific test class:

    mvn -Dtest=CalculatorTest test

Run a specific test method:

    mvn -Dtest=CalculatorTest#shouldAddTwoNumbers test

### Practice Tasks

- [ ] Test a calculator.
- [ ] Test a bank withdrawal method.
- [ ] Test invalid inputs.
- [ ] Write parameterized tests.
- [ ] Test exceptions.

**Resources:**
- [JUnit 5 Official Documentation](https://junit.org/junit5/docs/current/user-guide/)
- [GeeksforGeeks – JUnit Tutorial](https://www.geeksforgeeks.org/software-testing/junit-tutorial/)

---

## 5. AssertJ

### What is AssertJ?

AssertJ is a Java library that makes test assertions more readable and expressive.

JUnit assertion:

    assertEquals("Adarsh", user.getName());

AssertJ assertion:

    assertThat(user.getName()).isEqualTo("Adarsh");

Both verify the same result.

### 5.1 Basic Assertions

    import static org.assertj.core.api.Assertions.*;

    @Test
    void testUser() {
        String name = "Adarsh";
        int age = 21;

        assertThat(name).isEqualTo("Adarsh");
        assertThat(name).startsWith("Ada");
        assertThat(age).isGreaterThan(18);
        assertThat(age).isBetween(18, 30);
    }

### 5.2 Testing Lists

    @Test
    void testSkills() {
        List<String> skills = List.of(
            "Java", "Spring Boot", "Docker"
        );

        assertThat(skills)
            .hasSize(3)
            .contains("Java", "Docker")
            .doesNotContain("Python");
    }

### 5.3 Testing Objects

    @Test
    void testUserDetails() {
        User user = new User("Adarsh", 21);

        assertThat(user)
            .extracting("name", "age")
            .containsExactly("Adarsh", 21);
    }

### 5.4 Testing Exceptions

    @Test
    void shouldRejectNegativeAmount() {
        assertThatThrownBy(() -> account.deposit(-100))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessage("Invalid deposit amount");
    }

### When to Use AssertJ

- Checking collections.
- Comparing objects.
- Checking exception messages.
- Writing readable assertions.
- Testing complex responses.

**Resources:**
- [AssertJ Official Documentation](https://assertj.github.io/doc/)
- [GeeksforGeeks – AssertJ Tutorials](https://www.geeksforgeeks.org/?s=AssertJ)

---

## 6. Mockito

### What is Mockito?

Mockito is used to replace real dependencies with mock objects.

**Real-world example:**

Your `UserService` calls `UserRepository`.

You want to test the service without connecting to a real database.

### 6.1 Example Project Structure

    src/
    ├── main/java/com/example/
    │   ├── User.java
    │   ├── UserRepository.java
    │   └── UserService.java
    └── test/java/com/example/
        └── UserServiceTest.java

### 6.2 Service Implementation

Assume User has a constructor accepting an ID and name, plus a `getName()` method.

**UserRepository.java**

    public interface UserRepository {
        Optional<User> findById(Long id);
    }

**UserService.java**

    public class UserService {

        private final UserRepository repository;

        public UserService(UserRepository repository) {
            this.repository = repository;
        }

        public User getUser(Long id) {
            return repository.findById(id)
                .orElseThrow(() ->
                    new RuntimeException("User not found")
                );
        }
    }

### 6.3 Mocking Repository

    @ExtendWith(MockitoExtension.class)
    class UserServiceTest {

        @Mock
        UserRepository repository;

        @InjectMocks
        UserService userService;

        @Test
        void shouldReturnUser() {
            User user = new User(1L, "Adarsh");

            when(repository.findById(1L))
                .thenReturn(Optional.of(user));

            User result = userService.getUser(1L);

            assertEquals("Adarsh", result.getName());

            verify(repository).findById(1L);
        }
    }

Required imports include Mockito's `MockitoExtension`, annotations and methods, and JUnit's `@ExtendWith`.

### 6.4 Important Mockito Methods

| Method | Purpose |
|---|---|
| `@Mock` | Creates mock dependency |
| `@InjectMocks` | Injects mocks into object under test |
| `when()` | Defines mock behavior |
| `thenReturn()` | Returns predefined response |
| `verify()` | Checks method calls |
| `times(2)` | Checks number of calls |
| `never()` | Checks method was never called |
| `any()` | Matches compatible arguments |
| `ArgumentCaptor` | Captures method arguments |

### 6.5 Test User Not Found

    @Test
    void shouldThrowWhenUserNotFound() {
        when(repository.findById(99L))
            .thenReturn(Optional.empty());

        assertThrows(
            RuntimeException.class,
            () -> userService.getUser(99L)
        );

        verify(repository).findById(99L);
    }

### Practice Tasks

- [ ] Mock UserRepository.
- [ ] Test user retrieval.
- [ ] Test user not found.
- [ ] Verify repository calls.
- [ ] Test a service that sends an email.

**Resources:**
- [Mockito Official Documentation](https://site.mockito.org/)
- [GeeksforGeeks – Mockito Tutorials](https://www.geeksforgeeks.org/?s=Mockito)

---

## 7. Spring Boot Test Slices

### What are Test Slices?

Spring Boot Test Slices allow you to test only a specific part of your application instead of loading the complete Spring Boot application.

**Example:** To test a REST Controller, you don't need to start the entire application or connect to a database.

### 7.1 Important Annotations

| Annotation | What It Tests |
|---|---|
| `@WebMvcTest` | REST controllers |
| `@DataJpaTest` | JPA repositories |
| `@JsonTest` | JSON serialization/deserialization |
| `@RestClientTest` | REST client code |
| `@SpringBootTest` | Full Spring application context |

### 7.2 Testing a REST Controller

**UserController.java**

    @RestController
    @RequestMapping("/users")
    public class UserController {

        @GetMapping("/{id}")
        public String getUser(@PathVariable Long id) {
            return "Adarsh";
        }
    }

**UserControllerTest.java**

    @WebMvcTest(UserController.class)
    class UserControllerTest {

        @Autowired
        MockMvc mockMvc;

        @Test
        void shouldReturnUser() throws Exception {
            mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(content().string("Adarsh"));
        }
    }

**Explanation:**
- `@WebMvcTest` loads the MVC components needed for the controller test.
- `MockMvc` simulates HTTP requests.
- `status().isOk()` checks HTTP 200.
- `content().string()` verifies the response.

For a controller with a service dependency, provide a mock service bean. Spring Boot 3.4+ supports `@MockitoBean`.

### 7.3 Testing a JPA Repository

Assume UserRepository extends JpaRepository and User is a valid JPA entity.

    @DataJpaTest
    class UserRepositoryTest {

        @Autowired
        UserRepository repository;

        @Test
        void shouldSaveUser() {
            User user = new User();
            user.setName("Adarsh");

            User saved = repository.save(user);

            assertThat(saved.getId()).isNotNull();
            assertThat(saved.getName()).isEqualTo("Adarsh");
        }
    }

`@DataJpaTest` commonly uses an embedded database when available. Use Testcontainers if you need actual PostgreSQL behavior.

### When to Use

- `@WebMvcTest` → Test controller request/response behavior.
- `@DataJpaTest` → Test repository queries.
- `@SpringBootTest` → Test multiple application layers together.

**Resources:**
- [GeeksforGeeks – Spring Boot MockMvc](https://www.geeksforgeeks.org/springboot/spring-boot-mockmvc-example/)
- [Spring Boot Testing Documentation](https://docs.spring.io/spring-boot/reference/testing/)

---

## 8. Spring Security Testing

### What is Spring Security Testing?

It verifies whether your application's authentication and authorization rules work correctly.

**Practical scenarios:**
- Unauthenticated users cannot access protected APIs.
- Admins can access admin endpoints.
- Regular users cannot access admin endpoints.
- Invalid credentials are rejected.

### 8.1 Testing Protected APIs

Suppose `/admin` is accessible only to users with the `ADMIN` role.

    @RestController
    class AdminController {

        @PreAuthorize("hasRole('ADMIN')")
        @GetMapping("/admin")
        public String admin() {
            return "Admin Dashboard";
        }
    }

Enable method security using `@EnableMethodSecurity` in your security configuration.

### 8.2 Testing with Mock Users

    @SpringBootTest
    @AutoConfigureMockMvc
    class SecurityTest {

        @Autowired
        MockMvc mockMvc;

        @Test
        @WithMockUser(roles = "ADMIN")
        void adminShouldAccess() throws Exception {
            mockMvc.perform(get("/admin"))
                .andExpect(status().isOk());
        }

        @Test
        @WithMockUser(roles = "USER")
        void userShouldBeForbidden() throws Exception {
            mockMvc.perform(get("/admin"))
                .andExpect(status().isForbidden());
        }
    }

**Explanation:**
- `@WithMockUser` simulates an authenticated user.
- `roles = "ADMIN"` gives the user the admin role.
- `403 Forbidden` means the authenticated user lacks permission.

### 8.3 Testing Without Authentication

    @Test
    void anonymousUserShouldBeRejected() throws Exception {
        mockMvc.perform(get("/admin"))
            .andExpect(status().isUnauthorized());
    }

This expects HTTP 401 when your security configuration uses a 401 authentication entry point. Some configurations redirect anonymous users to a login page instead.

### 8.4 Important Security Testing Tools

| Tool | Purpose |
|---|---|
| `@WithMockUser` | Simulates authenticated user |
| `@WithAnonymousUser` | Simulates anonymous user |
| `@WithUserDetails` | Loads user from UserDetailsService |
| `jwt()` | Simulates JWT authentication |
| `csrf()` | Adds CSRF token for testing |

### Practice

- [ ] Test public endpoints.
- [ ] Test protected endpoints.
- [ ] Test admin-only endpoints.
- [ ] Test invalid authentication.
- [ ] Test JWT-protected endpoints.
- [ ] Test CSRF protection.

**Resources:**
- [Spring Security Testing Documentation](https://docs.spring.io/spring-security/reference/servlet/test/index.html)
- [GeeksforGeeks – Spring Security Testing](https://www.geeksforgeeks.org/?s=Spring+Security+Testing)

---

## 9. Testcontainers

### What is Testcontainers?

Testcontainers allows your tests to start real services such as PostgreSQL, MySQL, Redis or Kafka inside Docker containers.

**Example:**

Your application uses PostgreSQL.

Instead of mocking the database, Testcontainers starts a real PostgreSQL instance for testing.

### 9.1 Prerequisites

- Java and Maven.
- Docker running.
- Spring Boot project.
- Testcontainers dependencies.

Add the appropriate Testcontainers JUnit Jupiter and PostgreSQL modules, PostgreSQL JDBC driver, and Spring Boot test dependencies. Use versions compatible with your project.

### 9.2 PostgreSQL Integration Test

    @Testcontainers
    @SpringBootTest
    class DatabaseIntegrationTest {

        @Container
        static PostgreSQLContainer<?> postgres =
            new PostgreSQLContainer<>("postgres:16-alpine");

        @DynamicPropertySource
        static void configure(DynamicPropertyRegistry registry) {
            registry.add(
                "spring.datasource.url",
                postgres::getJdbcUrl
            );

            registry.add(
                "spring.datasource.username",
                postgres::getUsername
            );

            registry.add(
                "spring.datasource.password",
                postgres::getPassword
            );
        }

        @Autowired
        UserRepository repository;

        @Test
        void shouldSaveUser() {
            User user = new User();
            user.setName("Adarsh");

            User saved = repository.save(user);

            assertThat(saved.getId()).isNotNull();
        }
    }

This example uses the Testcontainers 1.x PostgreSQL module API.

### How It Works

1. JUnit starts the test.
2. Testcontainers starts PostgreSQL in Docker.
3. Spring Boot connects to the container.
4. The repository saves data.
5. The test checks the result.
6. Testcontainers cleans up the container.

### When to Use

- Database integration testing.
- Kafka integration testing.
- Redis integration testing.
- Testing actual SQL queries.
- Testing database migrations.

**Resources:**
- [Testcontainers Documentation](https://java.testcontainers.org/)
- [GeeksforGeeks – Spring Boot Testing](https://www.geeksforgeeks.org/springboot/spring-boot-starter-test/)

---

## 10. REST Assured

### What is REST Assured?

REST Assured is a Java library used to test REST APIs.

It sends real HTTP requests and verifies responses.

### 10.1 Testing a GET API

Suppose your Spring Boot application runs at `localhost:8080`.

Endpoint:

    GET /users/1

Expected response:

    {
      "id": 1,
      "name": "Adarsh"
    }

Test:

    import static io.restassured.RestAssured.*;
    import static org.hamcrest.Matchers.*;

    @Test
    void shouldGetUser() {
        given()
            .baseUri("http://localhost:8080")
        .when()
            .get("/users/1")
        .then()
            .statusCode(200)
            .body("name", equalTo("Adarsh"))
            .body("id", equalTo(1));
    }

### 10.2 Testing POST API

    @Test
    void shouldCreateUser() {
        String requestBody = """
            {
              "name": "Adarsh",
              "email": "adarsh@example.com"
            }
            """;

        given()
            .baseUri("http://localhost:8080")
            .contentType("application/json")
            .body(requestBody)
        .when()
            .post("/users")
        .then()
            .statusCode(201)
            .body("name", equalTo("Adarsh"));
    }

The API must implement these endpoints and return the expected response.

### 10.3 Testing Authentication

    @Test
    void shouldAccessProtectedApi() {
        given()
            .baseUri("http://localhost:8080")
            .auth()
            .oauth2("your-test-jwt-token")
        .when()
            .get("/profile")
        .then()
            .statusCode(200);
    }

Use a valid JWT issued by your test authentication setup.

### Important REST Assured Methods

| Method | Purpose |
|---|---|
| `given()` | Configure request |
| `when()` | Execute request |
| `then()` | Validate response |
| `statusCode()` | Verify HTTP status |
| `body()` | Verify response body |
| `header()` | Verify response header |
| `contentType()` | Set or verify content type |
| `auth()` | Configure authentication |

### Practice

- [ ] Test GET API.
- [ ] Test POST API.
- [ ] Test PUT API.
- [ ] Test DELETE API.
- [ ] Test invalid requests.
- [ ] Test authenticated requests.

**Resources:**
- [REST Assured Documentation](https://rest-assured.io/)
- [GeeksforGeeks – REST Assured](https://www.geeksforgeeks.org/?s=REST+Assured)

---

## 11. WireMock

### What is WireMock?

WireMock simulates external HTTP APIs during testing.

**Real-world example:**

Your Order Service calls an external Payment Service.

You don't want to make real payments during testing.

WireMock acts as a fake HTTP payment server.

### 11.1 Example

Suppose the real payment API returns:

    {
      "status": "SUCCESS"
    }

WireMock can return the same response without calling the real payment gateway.

### 11.2 Start WireMock

    WireMockServer wireMockServer =
        new WireMockServer(8089);

    wireMockServer.start();

### 11.3 Configure Fake Payment API

    wireMockServer.stubFor(
        post(urlEqualTo("/payment"))
            .willReturn(
                aResponse()
                    .withStatus(200)
                    .withHeader(
                        "Content-Type",
                        "application/json"
                    )
                    .withBody("""
                        {"status":"SUCCESS"}
                        """)
            )
    );

Now a POST request to `http://localhost:8089/payment` returns the predefined response.

### 11.4 Test the Response

    @Test
    void shouldReturnPaymentSuccess() {
        given()
            .baseUri("http://localhost:8089")
            .contentType("application/json")
            .body("{}")
        .when()
            .post("/payment")
        .then()
            .statusCode(200)
            .body("status", equalTo("SUCCESS"));
    }

Start WireMock and register the stub before running this test, then stop the server afterward.

For a proper integration test, configure your application's payment client to call WireMock and verify your application's behavior.

### 11.5 Testing API Failures

    wireMockServer.stubFor(
        post(urlEqualTo("/payment"))
            .willReturn(
                aResponse().withStatus(500)
            )
    );

Use this to test how your application handles payment server failures.

### When to Use

- Payment API testing.
- External authentication API testing.
- Third-party service failures.
- HTTP timeouts.
- API retry logic.

**Resources:**
- [WireMock Documentation](https://wiremock.org/docs/)
- [GeeksforGeeks – WireMock](https://www.geeksforgeeks.org/software-testing/introduction-to-wiremock/)
- [GeeksforGeeks – WireMock Stubbing](https://www.geeksforgeeks.org/software-testing/wiremock-stubbing-with-junit-test/)

---

## 12. Awaitility

### What is Awaitility?

Awaitility is used to test asynchronous operations.

**Example:**

Your application sends a message to Kafka.

A consumer processes the message after some time.

You cannot immediately assert that processing is complete.

### 12.1 Without Awaitility

    Thread.sleep(5000);

    assertEquals("COMPLETED", order.getStatus());

**Problem:** The test always waits 5 seconds, even if processing finishes sooner. It may also fail if processing takes longer.

### 12.2 With Awaitility

    import static org.awaitility.Awaitility.*;

    await()
        .atMost(Duration.ofSeconds(10))
        .untilAsserted(() -> {
            assertEquals(
                "COMPLETED",
                orderService.getStatus(orderId)
            );
        });

**Explanation:**

- `atMost()` sets maximum waiting time.
- `untilAsserted()` repeatedly checks the assertion.
- The test passes as soon as the condition becomes true.
- The test fails if the timeout is reached.

### 12.3 Polling Interval

    await()
        .pollInterval(Duration.ofMillis(500))
        .atMost(Duration.ofSeconds(10))
        .until(() -> orderService.isCompleted(orderId));

This checks the condition periodically rather than waiting a fixed duration.

### When to Use

- Kafka consumer testing.
- RabbitMQ testing.
- Background job testing.
- Asynchronous database updates.
- Event-driven microservices.

**Resources:**
- [Awaitility Documentation](https://github.com/awaitility/awaitility)
- [GeeksforGeeks – Awaitility](https://www.geeksforgeeks.org/?s=Awaitility)

---

## 13. JaCoCo

### What is JaCoCo?

JaCoCo measures how much Java code is executed by your automated tests.

**Example:**

Suppose your application has 100 lines of executable code.

Your tests execute 80 of those lines.

Approximate line coverage = 80%.

### 13.1 Maven Configuration

Add JaCoCo to `pom.xml`:

    <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.13</version>
        <executions>
            <execution>
                <goals>
                    <goal>prepare-agent</goal>
                </goals>
            </execution>
            <execution>
                <id>report</id>
                <phase>verify</phase>
                <goals>
                    <goal>report</goal>
                </goals>
            </execution>
        </executions>
    </plugin>

### 13.2 Generate Coverage Report

    mvn clean verify

Open:

    target/site/jacoco/index.html

### 13.3 Understanding Coverage

| Metric | Meaning |
|---|---|
| Line Coverage | Executed source lines |
| Branch Coverage | Executed decision branches |
| Method Coverage | Executed methods |
| Instruction Coverage | Executed bytecode instructions |

### 13.4 Practical Example

    public String checkAge(int age) {
        if (age >= 18) {
            return "Adult";
        }
        return "Minor";
    }

If you test only age 20, the other branch is not covered.

Add another test:

    @Test
    void shouldReturnMinor() {
        assertEquals("Minor", service.checkAge(15));
    }

**Important:** High coverage does not guarantee correct tests.

### When to Use

- Measuring test coverage.
- Identifying untested branches.
- CI/CD quality checks.
- Finding areas needing additional tests.

**Resources:**
- [JaCoCo Documentation](https://www.jacoco.org/jacoco/)
- [GeeksforGeeks – JUnit 5 JaCoCo Code Coverage](https://www.geeksforgeeks.org/software-testing/junit-5-jacoco-code-coverage/)

---

## 14. Contract Testing (Pact / Spring Cloud Contract)

### What is Contract Testing?

Contract testing verifies that two services agree on how an API should behave.

**Example:**

Order Service calls Payment Service.

Order Service expects:

    {
      "paymentId": 101,
      "status": "SUCCESS"
    }

If Payment Service suddenly changes the field from `status` to `paymentStatus`, Order Service might break.

Contract testing helps detect such breaking changes.

### 14.1 Consumer and Provider

| Component | Responsibility |
|---|---|
| Consumer | Service calling the API |
| Provider | Service providing the API |
| Contract | Agreed request and response behavior |

### 14.2 Pact Example

Suppose Order Service expects a payment API response.

    @Pact(consumer = "order-service")
    V4Pact paymentPact(PactDslWithProvider builder) {
        return builder
            .given("payment exists")
            .uponReceiving("get payment status")
            .path("/payments/101")
            .method("GET")
            .willRespondWith()
            .status(200)
            .body(
                newJsonBody(body -> {
                    body.integerType("paymentId", 101);
                    body.stringType("status", "SUCCESS");
                }).build()
            )
            .toPact(V4Pact.class);
    }

This defines an expected interaction using Pact JVM's V4 DSL. A complete consumer test also executes the HTTP request against Pact's mock server, and the provider later verifies the generated contract.

### 14.3 Spring Cloud Contract Example

A Groovy contract can define expected API behavior:

    Contract.make {
        request {
            method 'GET'
            url '/payments/101'
        }
        response {
            status 200
            headers {
                contentType(applicationJson())
            }
            body(
                paymentId: 101,
                status: 'SUCCESS'
            )
        }
    }

Spring Cloud Contract can generate tests and stubs from this definition when the relevant build plugin and dependencies are configured.

### Pact vs Spring Cloud Contract

| Pact | Spring Cloud Contract |
|---|---|
| Consumer-driven contracts | Supports consumer- and producer-driven workflows |
| Works across multiple languages | Strong Spring ecosystem integration |
| Uses Pact contracts | Uses contract DSL definitions |
| Useful for polyglot microservices | Useful for Spring-based microservices |

### When to Use

- Microservice communication.
- API versioning.
- Preventing breaking API changes.
- Independent service deployments.

**Resources:**
- [Pact Documentation](https://docs.pact.io/)
- [Spring Cloud Contract Documentation](https://docs.spring.io/spring-cloud-contract/reference/)
- [GeeksforGeeks – Spring Cloud Contract](https://www.geeksforgeeks.org/advance-java/spring-cloud-contract/)

---

## 15. Mutation Testing (PIT)

### What is Mutation Testing?

Mutation testing checks whether your tests are strong enough to detect incorrect code.

**Example:**

Original code:

    public boolean isAdult(int age) {
        return age >= 18;
    }

PIT may automatically change it to:

    public boolean isAdult(int age) {
        return age > 18;
    }

This is called a mutation.

If your tests detect the change, the mutation is **killed**.

If your tests still pass, the mutation **survives**.

### 15.1 Weak Test

    @Test
    void shouldReturnAdult() {
        assertTrue(service.isAdult(25));
    }

Problem: This test passes for both `>= 18` and `> 18`.

### 15.2 Strong Test

    @Test
    void shouldReturnAdultAt18() {
        assertTrue(service.isAdult(18));
    }

    @Test
    void shouldReturnMinorAt17() {
        assertFalse(service.isAdult(17));
    }

Now changing `>=` to `>` causes the first test to fail.

### 15.3 Maven Configuration

Add PIT to your Maven plugins:

    <plugin>
        <groupId>org.pitest</groupId>
        <artifactId>pitest-maven</artifactId>
        <version>1.19.1</version>
        <dependencies>
            <dependency>
                <groupId>org.pitest</groupId>
                <artifactId>pitest-junit5-plugin</artifactId>
                <version>1.2.2</version>
            </dependency>
        </dependencies>
    </plugin>

Check plugin compatibility with your JUnit and Java versions before using this configuration.

### 15.4 Run Mutation Testing

    mvn test
    mvn org.pitest:pitest-maven:mutationCoverage

PIT typically generates reports inside:

    target/pit-reports/

### 15.5 Mutation Score

    Mutation Score =
    Killed Mutations / Total Relevant Mutations × 100

Example:

- Total mutations: 100
- Killed mutations: 85
- Survived mutations: 15

Mutation score = 85%.

Actual reports may also classify mutations as no coverage, timed out, or other statuses.

### JaCoCo vs PIT

| JaCoCo | PIT |
|---|---|
| Measures executed code | Measures ability to detect code changes |
| Focuses on coverage | Focuses on test effectiveness |
| Faster | Usually slower |
| Identifies untested code | Identifies weak tests |

### When to Use

- Improving unit test quality.
- Testing important business rules.
- Finding missing boundary tests.
- Evaluating test suite effectiveness.

**Resources:**
- [PIT Mutation Testing Documentation](https://pitest.org/)
- [GeeksforGeeks – Mutation Testing](https://www.geeksforgeeks.org/?s=Mutation+Testing)

---

## 16. Practical Learning Roadmap

| Order | Tool | What to Build |
|---|---|---|
| 1 | JUnit 5 | Calculator and BankAccount tests |
| 2 | AssertJ | Object and collection assertions |
| 3 | Mockito | UserService tests with mocked repository |
| 4 | Spring Boot Test Slices | Controller and repository tests |
| 5 | Spring Security Testing | Role-based API access tests |
| 6 | Testcontainers | PostgreSQL integration tests |
| 7 | REST Assured | CRUD API tests |
| 8 | WireMock | Fake payment gateway |
| 9 | Awaitility | Async order processing tests |
| 10 | JaCoCo | Test coverage reports |
| 11 | Contract Testing | Order–Payment Service contract |
| 12 | PIT | Mutation testing for business logic |

---

## 17. Final Practice Project – E-Commerce Order Service

Build one Spring Boot application and apply all these testing tools.

### Features

- User registration.
- Login with JWT.
- Product management.
- Order creation.
- Payment processing.
- Order status updates.

### Testing Checklist

- [ ] JUnit: Test order price calculation.
- [ ] AssertJ: Verify order details and collections.
- [ ] Mockito: Mock ProductRepository and PaymentClient.
- [ ] WebMvcTest: Test OrderController.
- [ ] DataJpaTest: Test OrderRepository.
- [ ] Spring Security Test: Verify admin and user permissions.
- [ ] Testcontainers: Test PostgreSQL integration.
- [ ] REST Assured: Test complete Order API.
- [ ] WireMock: Simulate payment gateway success and failure.
- [ ] Awaitility: Verify asynchronous order processing.
- [ ] JaCoCo: Generate coverage reports.
- [ ] Pact: Verify Order Service and Payment Service API contracts.
- [ ] PIT: Check whether unit tests catch incorrect business logic.

---

## 18. Quick Revision

| Tool | Remember |
|---|---|
| JUnit 5 | Write and run tests |
| AssertJ | Make assertions readable |
| Mockito | Mock Java dependencies |
| Test Slices | Test selected Spring components |
| Security Testing | Verify authentication and permissions |
| Testcontainers | Test using real Docker services |
| REST Assured | Test HTTP APIs |
| WireMock | Simulate external HTTP APIs |
| Awaitility | Wait for asynchronous results |
| JaCoCo | Measure test coverage |
| Pact / Spring Cloud Contract | Verify API agreements |
| PIT | Check how strong your tests are |

**Recommended Priority:**

1. Master JUnit 5, AssertJ and Mockito.
2. Learn Spring Boot Test Slices and Spring Security Testing.
3. Practise REST Assured and Testcontainers.
4. Learn WireMock and Awaitility.
5. Finally, study Contract Testing, JaCoCo and PIT.

**Goal:** Build confidence in testing real-world Spring Boot applications and microservices.