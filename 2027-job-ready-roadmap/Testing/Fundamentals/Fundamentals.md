Testing Fundamentals
Test Doubles
TDD Basics


# Software Testing – Complete Study Notes

## 1. Testing Fundamentals

### 1.1 What is Software Testing?

Software testing is the process of evaluating an application or software system to identify defects, errors, bugs, and verify that it behaves according to specified requirements.

**Objectives:**
- Identify bugs before software reaches production.
- Verify that software meets business requirements.
- Improve software quality and reliability.
- Ensure security, performance, and usability.
- Reduce the cost of fixing defects.

**Example:**

In a banking application, test the withdrawal functionality:

- Withdrawal amount less than balance.
- Withdrawal amount greater than balance.
- Withdrawal amount equal to balance.
- Zero withdrawal.
- Negative withdrawal.

### 1.2 Verification vs Validation

| Verification | Validation |
|---|---|
| Are we building the product right? | Are we building the right product? |
| Checks development artifacts against specifications. | Checks whether the product meets user needs. |
| Includes reviews and inspections. | Commonly involves executing software. |
| Does not necessarily require code execution. | Usually requires code execution. |

**Example:**
- Verification: Checking whether login implementation follows the design document.
- Validation: Checking whether users can successfully log in.

### 1.3 Error, Defect, Bug and Failure

| Term | Meaning |
|---|---|
| Error | A human mistake during development. |
| Defect | A flaw in software or implementation. |
| Bug | Commonly used as another name for a defect. |
| Failure | When software behaves incorrectly during execution. |

**Example:**
1. Developer writes an incorrect calculation (Error).
2. Incorrect calculation exists in code (Defect).
3. The issue is reported as a bug (Bug).
4. Application displays the wrong amount (Failure).

### 1.4 Types of Software Testing

#### A. Manual Testing

Testing performed manually by a human tester without automated test scripts.

Examples:
- Login testing.
- Form validation.
- User interface testing.
- Exploratory testing.

Advantages:
- Useful for exploratory testing.
- Helps evaluate usability.
- Does not require automation scripts.

Disadvantages:
- Time-consuming.
- Repetitive work.
- More susceptible to human error.

#### B. Automation Testing

Testing performed using tools and scripts that automatically execute test cases.

Popular tools:
- JUnit – Java testing.
- Mockito – Java mocking framework.
- Selenium – Browser automation.
- Cypress – Frontend testing.
- Jest – JavaScript testing.
- Playwright – Browser automation.

**JUnit Example:**

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @Test
    void testAddition() {
        int result = 10 + 20;
        assertEquals(30, result);
    }
}
```

### 1.5 Levels of Software Testing

#### 1. Unit Testing

Tests individual components or functions independently.

Examples:
- Test a calculator addition function.
- Test a Java service method.

#### 2. Integration Testing

Tests whether multiple modules or components work correctly together.

Examples:
- User Service communicating with Database.
- API communicating with Payment Service.

#### 3. System Testing

Tests the complete application as a whole.

Example:
- Testing login, cart, payment and order management in an e-commerce application.

#### 4. Acceptance Testing

Checks whether the application meets business requirements and is acceptable to the customer.

Example:
- Client verifies the complete online shopping workflow.

### 1.6 Testing Techniques

#### Black Box Testing

Testing software without knowing its internal implementation.

Focus:
- Input.
- Output.
- Functional requirements.

Example: Enter username and password and verify login.

#### White Box Testing

Testing based on knowledge of internal code structure.

Focus:
- Statements.
- Branches.
- Conditions.
- Execution paths.

Example: Testing both IF and ELSE branches.

#### Gray Box Testing

Combination of Black Box and White Box Testing.

The tester has partial knowledge of the internal implementation.

Example: Testing an API while knowing its database schema.

### 1.7 Important Testing Terminology

| Term | Explanation |
|---|---|
| Test Case | Inputs, conditions and expected results. |
| Test Suite | Collection of test cases. |
| Test Scenario | High-level description of what needs testing. |
| Test Plan | Document describing testing strategy and scope. |
| Test Data | Data used during testing. |
| Test Coverage | Measures how much code or requirements are tested. |
| Regression Testing | Ensures new changes have not broken existing functionality. |
| Smoke Testing | Basic checks to verify application stability. |
| Sanity Testing | Focused checks after a small change or bug fix. |
| Exploratory Testing | Testing by exploring the application without predefined test cases. |

### 1.8 Assertions

Assertions verify expected outcomes during testing.

Common JUnit assertions:

```java
assertEquals(100, actualBalance);
assertNotEquals(50, actualBalance);
assertTrue(isLoggedIn);
assertFalse(isBlocked);
assertNull(user);
assertNotNull(user);
assertThrows(IllegalArgumentException.class, () -> {});
```

---

## 2. Test Doubles

### 2.1 What are Test Doubles?

A Test Double is an object that replaces a real dependency during testing.

It is used when testing one component without depending on external systems such as:

- Databases.
- Payment gateways.
- Email services.
- Third-party APIs.
- Message queues.
- External microservices.

**Why use Test Doubles?**

Suppose an OrderService communicates with:

1. Payment Service.
2. Inventory Service.
3. Notification Service.
4. Database.

Using all real dependencies makes testing slow, expensive and difficult to maintain.

Test Doubles replace these dependencies with controlled alternatives.

### 2.2 Types of Test Doubles

There are five important types:

1. Dummy
2. Stub
3. Mock
4. Fake
5. Spy

### A. Dummy Objects

A Dummy object is passed to a method only because the method requires an argument.

It is not actually used by the test.

**Example:**

```java
class User {
    String name;

    User(String name) {
        this.name = name;
    }
}

class UserService {
    void createUser(User user) {
        System.out.println("User created");
    }
}
```

Test:

```java
@Test
void testCreateUser() {

    User dummyUser = new User("Test");

    UserService service = new UserService();

    service.createUser(dummyUser);
}
```

**Use case:** When a method requires an object parameter but the test does not care about its value or behavior.

### B. Stub

A Stub provides predefined responses to method calls.

It does not perform real operations. It returns predetermined data.

**Example:**

```java
interface UserRepository {
    String findUserById(int id);
}
```

Stub implementation:

```java
class UserRepositoryStub implements UserRepository {

    public String findUserById(int id) {
        return "Adarsh";
    }
}
```

Service:

```java
class UserService {

    private UserRepository repository;

    UserService(UserRepository repository) {
        this.repository = repository;
    }

    String getUser(int id) {
        return repository.findUserById(id);
    }
}
```

Test:

```java
@Test
void testGetUser() {

    UserRepository stub = new UserRepositoryStub();

    UserService service = new UserService(stub);

    assertEquals("Adarsh", service.getUser(1));
}
```

**Use case:** When you need predictable responses from dependencies.

### C. Mock Objects

A Mock simulates a dependency and allows you to verify whether particular interactions happened.

**Example:**

```java
import static org.mockito.Mockito.*;

@Test
void testOrderNotification() {

    EmailService email = mock(EmailService.class);

    email.sendEmail("test@gmail.com");

    verify(email).sendEmail("test@gmail.com");
}
```

Explanation:
- `mock()` creates a mock object.
- `verify()` checks whether a method was called.
- `sendEmail()` represents the interaction being tested.

**Use case:** When you need to verify whether a dependency was called correctly.

### D. Fake Objects

A Fake object is a simplified but working implementation of a real dependency.

**Example:**

```java
class FakeUserRepository {

    private Map<Integer, String> users = new HashMap<>();

    void save(int id, String name) {
        users.put(id, name);
    }

    String findById(int id) {
        return users.get(id);
    }
}
```

Test:

```java
@Test
void testFakeRepository() {

    FakeUserRepository repository = new FakeUserRepository();

    repository.save(1, "Adarsh");

    assertEquals("Adarsh", repository.findById(1));
}
```

Here, the fake repository stores and retrieves data without a real database.

**Use case:** When you need a lightweight working replacement for a complex dependency.

### E. Spy Objects

A Spy observes interactions with an object.

It allows you to check which methods were called and how many times they were called.

**Mockito Example:**

```java
import static org.mockito.Mockito.*;

@Test
void testSpy() {

    List<String> list = new ArrayList<>();

    List<String> spyList = spy(list);

    spyList.add("Java");
    spyList.add("Spring Boot");

    verify(spyList).add("Java");

    verify(spyList, times(2)).add(anyString());

    assertEquals(2, spyList.size());
}
```

Explanation:
- `spy()` wraps a real object.
- Real methods execute by default.
- `verify()` checks method interactions.
- `times(2)` checks the number of invocations.

**Use case:** When you want to observe a real object while retaining its real functionality.

### 2.3 Difference Between Test Doubles

| Type | Main Purpose | Implementation |
|---|---|---|
| Dummy | Satisfies method arguments | Not required |
| Stub | Returns predefined responses | No |
| Mock | Verifies interactions | Simulated |
| Fake | Provides simplified working functionality | Yes, simplified |
| Spy | Observes real object interactions | Yes, usually |

**Easy way to remember:**

- Dummy = Just pass an object.
- Stub = Returns fixed data.
- Mock = Checks whether a method was called.
- Fake = Simplified working implementation.
- Spy = Observes a real object.

### 2.4 Mockito Framework

Mockito is a Java mocking framework used to create and manage test doubles.

Important Mockito methods:

| Method | Purpose |
|---|---|
| `mock()` | Creates a mock object. |
| `spy()` | Creates a spy around a real object. |
| `when()` | Defines mock behavior. |
| `thenReturn()` | Defines the response to return. |
| `verify()` | Verifies method interactions. |
| `times()` | Verifies invocation count. |
| `never()` | Checks that a method was never called. |
| `doThrow()` | Configures an exception. |
| `any()` | Matches any compatible argument. |

**Example: Stubbing with Mockito**

```java
UserRepository repository = mock(UserRepository.class);

when(repository.findUserById(1))
        .thenReturn("Adarsh");

assertEquals(
    "Adarsh",
    repository.findUserById(1)
);
```

### 2.5 When Should You Use Test Doubles?

| Situation | Recommended Double |
|---|---|
| Method requires an unused parameter | Dummy |
| Need predefined output | Stub |
| Need to verify method calls | Mock |
| Need simplified working database | Fake |
| Need to monitor a real object | Spy |

**Best Practices:**
- Do not mock every dependency.
- Keep mock configurations simple.
- Avoid testing internal implementation details unnecessarily.
- Prefer real objects when lightweight and reliable.
- Use integration tests to verify real interactions.

---

## 3. Test-Driven Development (TDD) Basics

### 3.1 What is TDD?

Test-Driven Development (TDD) is a software development approach in which developers write automated test cases before writing the actual implementation code.

Traditional development:

1. Write code.
2. Test code.
3. Fix bugs.

TDD:

1. Write a test.
2. Write code to pass the test.
3. Refactor the code.

The main idea is to make tests drive the development process.

### 3.2 Red-Green-Refactor Cycle

This is the most important concept in TDD.

#### Phase 1: RED – Write a Failing Test

First, write a test for the functionality you want to develop.

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @Test
    void testAddition() {

        Calculator calculator = new Calculator();

        assertEquals(30, calculator.add(10, 20));
    }
}
```

At this stage, the Calculator class or add() method might not exist.

The test fails because the implementation is missing.

**RED:** Test fails as expected.

#### Phase 2: GREEN – Write Minimum Code

Write the minimum implementation required to pass the test.

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }
}
```

Run the test again.

Expected result:

```text
Test: testAddition()
Expected: 30
Actual: 30

BUILD SUCCESS
```

**GREEN:** Test passes.

#### Phase 3: REFACTOR – Improve the Code

Improve the implementation without changing its external behavior.

Examples:
- Remove duplicate code.
- Improve variable names.
- Simplify logic.
- Improve readability.
- Improve maintainability.

After refactoring, execute the tests again.

**REFACTOR:** Improve code while keeping tests green.

### 3.3 Complete TDD Example Using Java and JUnit

Let's develop a simple BankAccount application using TDD.

**Requirements:**

1. Deposit money.
2. Withdraw money.
3. Check account balance.
4. Prevent negative deposits.
5. Prevent withdrawals exceeding the balance.

#### Step 1: Write Tests First

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class BankAccountTest {

    @Test
    void testDeposit() {

        BankAccount account = new BankAccount();

        account.deposit(1000);

        assertEquals(1000, account.getBalance());
    }

    @Test
    void testWithdraw() {

        BankAccount account = new BankAccount();

        account.deposit(1000);

        account.withdraw(300);

        assertEquals(700, account.getBalance());
    }

    @Test
    void testNegativeDeposit() {

        BankAccount account = new BankAccount();

        assertThrows(
            IllegalArgumentException.class,
            () -> account.deposit(-100)
        );
    }

    @Test
    void testInsufficientBalance() {

        BankAccount account = new BankAccount();

        account.deposit(500);

        assertThrows(
            IllegalArgumentException.class,
            () -> account.withdraw(1000)
        );
    }
}
```

Initially, these tests fail because the implementation does not exist.

#### Step 2: Implement BankAccount

```java
class BankAccount {

    private int balance = 0;

    public void deposit(int amount) {

        if (amount <= 0) {
            throw new IllegalArgumentException(
                "Invalid deposit amount"
            );
        }

        balance += amount;
    }

    public void withdraw(int amount) {

        if (amount <= 0 || amount > balance) {
            throw new IllegalArgumentException(
                "Invalid withdrawal"
            );
        }

        balance -= amount;
    }

    public int getBalance() {
        return balance;
    }
}
```

#### Step 3: Execute Tests

| Test | Expected Result |
|---|---|
| Deposit ₹1,000 | Pass |
| Withdraw ₹300 | Pass |
| Deposit negative amount | Pass |
| Withdraw more than balance | Pass |

#### Step 4: Refactor

Improve the code if necessary without changing its behavior.

Then execute the entire test suite again.

This completes one Red-Green-Refactor cycle.

### 3.4 Benefits of TDD

1. **Early Bug Detection:** Bugs are detected during development.
2. **Better Code Quality:** Encourages modular and maintainable code.
3. **Improved Design:** Developers think about requirements before implementation.
4. **Regression Protection:** Existing tests identify problems caused by new changes.
5. **Documentation:** Tests demonstrate expected component behavior.
6. **Confidence:** Developers can refactor code with automated checks.

### 3.5 Disadvantages of TDD

- Requires additional time initially.
- Developers need testing knowledge.
- Writing tests for poorly designed systems can be difficult.
- Frequent requirement changes may require test updates.
- Poorly written tests can create false confidence.
- TDD does not eliminate integration, system or acceptance testing.

### 3.6 TDD vs Traditional Development

| TDD | Traditional Development |
|---|---|
| Tests are written first. | Code is written first. |
| Test-first approach. | Implementation-first approach. |
| Follows Red-Green-Refactor. | Usually follows Code-Test-Fix. |
| Encourages testable design. | Design may evolve after implementation. |
| Provides continuous test feedback. | Testing may happen later. |

### 3.7 TDD vs BDD vs ATDD

| Approach | Full Form | Focus |
|---|---|---|
| TDD | Test-Driven Development | Code and unit-level behavior |
| BDD | Behavior-Driven Development | Application behavior using understandable scenarios |
| ATDD | Acceptance Test-Driven Development | Acceptance criteria and customer requirements |

**Example: Login System**

- TDD: Test whether the validatePassword() method works.
- BDD: Given a valid user, when they log in, then they reach the dashboard.
- ATDD: Define acceptance tests with the customer for valid login, invalid login and account lockout.

---

## 4. Practical Tools to Learn

| Tool | Purpose | Language |
|---|---|---|
| JUnit 5 | Unit testing | Java |
| Mockito | Mocking dependencies | Java |
| Spring Boot Test | Spring application testing | Java |
| AssertJ | Fluent assertions | Java |
| Jest | Unit testing | JavaScript |
| Supertest | API testing | Node.js |
| Vitest | Unit testing | JavaScript |

### Official Documentation

- JUnit 5: https://junit.org/junit5/docs/current/user-guide/
- Mockito: https://site.mockito.org/
- Spring Boot Testing: https://docs.spring.io/spring-boot/reference/testing/
- Jest: https://jestjs.io/docs/getting-started

---

## 5. GeeksforGeeks Learning Resources

### Testing Fundamentals

1. [Introduction to Software Testing](https://www.geeksforgeeks.org/software-testing-introduction/)
2. [Complete Software Testing Tutorial](https://www.geeksforgeeks.org/software-testing-tutorial/)
3. [Levels of Software Testing](https://www.geeksforgeeks.org/software-testing-levels/)
4. [Software Testing Techniques](https://www.geeksforgeeks.org/software-testing-techniques/)

### Test Doubles

1. [Mocking and Test Doubles](https://www.geeksforgeeks.org/)
2. [Mockito Tutorial](https://www.geeksforgeeks.org/mockito-tutorial/)

### Test-Driven Development

1. [Test-Driven Development (TDD)](https://www.geeksforgeeks.org/test-driven-development-tdd/)
2. [TDD Concepts and Practices](https://www.geeksforgeeks.org/)

*Note: GeeksforGeeks article URLs can change. If a specific link does not open, search its title on GeeksforGeeks.*

---

## 6. Interview Preparation – Important Questions

1. What is software testing and why is it important?
2. What is the difference between verification and validation?
3. What is the difference between unit testing and integration testing?
4. What are Test Doubles?
5. What is the difference between a Stub and a Mock?
6. What is the difference between a Fake and a Mock?
7. What is Mockito?
8. What is TDD?
9. Explain Red-Green-Refactor.
10. What is regression testing?
11. What is the difference between TDD and BDD?
12. What is the difference between manual and automation testing?
13. What are assertions in JUnit?
14. What is the difference between a Spy and a Mock?
15. What is test coverage?

---

## 7. Recommended Study Order

- [ ] Understand software testing fundamentals.
- [ ] Learn testing levels and testing types.
- [ ] Understand test cases and assertions.
- [ ] Learn unit testing using JUnit.
- [ ] Understand Dummy, Stub, Mock, Fake and Spy.
- [ ] Practise Mockito.
- [ ] Understand TDD.
- [ ] Implement Red-Green-Refactor using JUnit.
- [ ] Write unit tests for a Spring Boot REST API.
- [ ] Practise mocking a repository and external API.

## 8. Practical Assignment

Build a small Spring Boot application with:

1. User Entity.
2. User Repository.
3. User Service.
4. User Controller.

**Testing tasks:**

- Write unit tests for UserService using JUnit.
- Mock UserRepository using Mockito.
- Test user creation.
- Test user retrieval.
- Test user not found.
- Test duplicate email.
- Test invalid user input.
- Write integration tests for REST endpoints.

**Goal:** Understand how testing works in a real-world Spring Boot microservice.

---

**End of Notes**
