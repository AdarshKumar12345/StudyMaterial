# 01. Testing Fundamentals

> Learn why we test, what kinds of tests exist, and how to write tests that are fast, reliable and useful. Everything here applies to Java, Node.js and Python.

---

## 1. Why We Test

Automated tests are executable documentation and a safety net.

- **Catch bugs early**: a bug found in a unit test costs minutes; the same bug in production costs hours or days.
- **Enable refactoring**: you can change internals confidently when tests confirm behavior is unchanged.
- **Document behavior**: a good test shows how code is meant to be used.
- **Improve design**: code that is hard to test is usually too tightly coupled.
- **Speed up delivery**: CI/CD only works if you can trust an automated check to say "safe to deploy".

> Tests do not prove software is bug-free. They give you evidence that specific behaviors work.

---

## 2. Types of Tests

| Type | What it checks | Speed | Typical tools |
|---|---|---|---|
| **Unit** | One class or function in isolation | Milliseconds | JUnit, Jest/Vitest, pytest |
| **Integration** | Several pieces together (service + DB, service + queue) | Seconds | Spring slices, Testcontainers, Supertest |
| **Contract** | Agreement between a consumer and a provider API | Seconds | Pact, Spring Cloud Contract |
| **End-to-End (E2E)** | A full user flow through the real system | Slow | Playwright, Cypress, Selenium |
| **Smoke** | The app starts and critical paths respond | Fast | Simple scripts, health checks |
| **Regression** | Previously fixed bugs stay fixed | Varies | Any of the above |
| **Performance / Load** | Behavior under load | Slow | JMeter, k6, Gatling |
| **Security** | Known vulnerabilities and misconfigurations | Varies | OWASP ZAP, dependency scanners |

### Unit vs Integration (the most common confusion)

- A **unit test** does not touch the network, a database, the file system or the clock. Collaborators are replaced with test doubles.
- An **integration test** deliberately uses real collaborators (for example a real PostgreSQL in a container) to check the pieces work together.

---

## 3. The Test Pyramid

```
        /\
       /  \        E2E tests        few, slow, expensive
      /----\
     /      \      Integration      a moderate number
    /--------\
   /          \    Unit tests       many, fast, cheap
  /------------\
```

**Guideline:** write many unit tests, a moderate number of integration tests, and few E2E tests.

**Why this shape?**
- Lower levels run faster and fail with a precise location.
- Higher levels give more realistic confidence but are slower and more fragile.

**Anti-patterns**
- **Ice cream cone**: mostly manual or E2E tests with few unit tests. Slow feedback and flaky suites.
- **Hourglass**: lots of unit and E2E tests but nothing in the middle, so integration bugs slip through.

> Some teams (especially for microservices) prefer a "testing trophy" or "honeycomb" with more weight on integration tests. The principle is the same: pick the cheapest test that gives real confidence.

---

## 4. Anatomy of a Good Test

### 4.1 Arrange-Act-Assert (AAA)

1. **Arrange**: set up objects and inputs.
2. **Act**: call the code under test (usually one line).
3. **Assert**: verify the outcome.

### 4.2 Given-When-Then

Same idea, from BDD. Useful for describing behavior in plain language.

- **Given** an account with a balance of 100
- **When** the user withdraws 30
- **Then** the balance is 70

### Example in three languages

**Java (JUnit 5 + AssertJ)**

```java
import static org.assertj.core.api.Assertions.assertThat;
import org.junit.jupiter.api.Test;

class BankAccountTest {

    @Test
    void withdraw_reducesBalance() {
        // Arrange
        BankAccount account = new BankAccount(100);

        // Act
        account.withdraw(30);

        // Assert
        assertThat(account.getBalance()).isEqualTo(70);
    }
}
```

**JavaScript (Vitest or Jest)**

```js
import { describe, it, expect } from 'vitest'; // omit this line in Jest
import { BankAccount } from './bankAccount.js';

describe('BankAccount', () => {
  it('reduces the balance when withdrawing', () => {
    // Arrange
    const account = new BankAccount(100);

    // Act
    account.withdraw(30);

    // Assert
    expect(account.balance).toBe(70);
  });
});
```

**Python (pytest)**

```python
from bank_account import BankAccount

def test_withdraw_reduces_balance():
    # Arrange
    account = BankAccount(100)

    # Act
    account.withdraw(30)

    # Assert
    assert account.balance == 70
```

---

## 5. The FIRST Principles

| Letter | Principle | Meaning |
|---|---|---|
| **F** | Fast | Tests run in milliseconds so you run them constantly |
| **I** | Isolated / Independent | No test depends on another test or on execution order |
| **R** | Repeatable | Same result every run, on any machine |
| **S** | Self-validating | Pass or fail automatically, with no manual inspection of output |
| **T** | Timely | Written close to (or before) the production code |

---

## 6. Naming Tests

A test name should say **what is being tested, in what situation, and what is expected**.

| Style | Example |
|---|---|
| `method_condition_expectedResult` | `withdraw_amountGreaterThanBalance_throwsException` |
| `should_..._when_...` | `should_throw_when_amount_exceeds_balance` |
| Plain sentence (JUnit `@DisplayName`, Jest `it('...')`) | `"rejects a withdrawal larger than the balance"` |

**Avoid:** `test1`, `testWithdraw`, `works`. When a test fails, the name should tell you what broke without reading the code.

---

## 7. What to Test (and What Not To)

### Test these
- Business rules and calculations
- Edge cases: empty, null/None/undefined, zero, negative, very large, boundaries (off-by-one)
- Error handling and validation
- Branching logic (every important `if`/`else`)
- Public behavior and contracts
- Bugs you have fixed (write the failing test first, then fix)

### Usually skip these
- Trivial getters and setters
- Framework or library code (do not test that Spring, Express or Django work)
- Private methods directly (test them through public behavior)
- Generated code
- Pure configuration with no logic

### Test behavior, not implementation

| Fragile (tests implementation) | Robust (tests behavior) |
|---|---|
| Verifies that a private helper was called 3 times | Verifies the final result is correct |
| Breaks when you rename or restructure internals | Survives refactoring |

> Rule of thumb: if you refactor and a test breaks while behavior is unchanged, that test was coupled to implementation.

---

## 8. Edge Cases and Boundary Values

For any input, ask: what are the smallest, largest and strangest values?

**Example: a discount function for order totals**

| Input | Why it matters |
|---|---|
| `0` | Lower boundary |
| `-1` | Invalid input |
| `99.99` and `100.00` | Just below and at a threshold |
| Very large number | Overflow or precision |
| `null` / `None` / `undefined` | Missing data |

Techniques:
- **Boundary value analysis**: test at, just below and just above each limit.
- **Equivalence partitioning**: group inputs that should behave the same and test one from each group.

---

## 9. Code Coverage

Coverage measures how much code your tests execute.

| Metric | Meaning |
|---|---|
| Line coverage | Percentage of lines run |
| Branch coverage | Percentage of `if`/`else` paths taken |
| Mutation score | Percentage of deliberately introduced bugs your tests catch |

**Tools:** JaCoCo (Java), c8 / Istanbul (Node.js), pytest-cov (Python).

**Important truths**
- High coverage does **not** mean good tests. A test with no assertions still raises coverage.
- Low coverage does mean there are untested areas.
- Use coverage to **find gaps**, not as a target to chase. A common practical range is 70-80% on business logic, but meaningful assertions matter more than the number.
- Mutation testing (PIT, Stryker, mutmut) measures test quality better than line coverage.

---

## 10. Flaky Tests

A flaky test passes and fails randomly without any code change. They destroy trust in the suite.

**Common causes and fixes**

| Cause | Fix |
|---|---|
| Real time (`now()`, `Date.now()`, sleeps) | Inject a clock; use fake timers |
| Randomness | Seed the generator or inject it |
| Shared state between tests | Reset state in setup/teardown; avoid global mutable state |
| Test order dependency | Make each test self-contained; run in random order to find issues |
| Real network or external services | Use fakes, WireMock, MSW or Testcontainers |
| Async timing | Wait for conditions (Awaitility, `waitFor`), not fixed sleeps |
| Shared database data | Use unique data per test or roll back after each test |

**Policy:** never ignore a flaky test. Fix it, quarantine it with a ticket, or delete it.

---

## 11. Test Smells (Things to Avoid)

- **Giant tests**: one test checking ten behaviors. Split them.
- **Logic in tests**: loops and conditionals make tests hard to trust.
- **Hidden setup**: the reader must jump across files to understand the scenario.
- **Over-mocking**: so many mocks that the test only verifies the mocks.
- **Testing the framework**: asserting that the library does what its docs say.
- **Commented-out or `@Disabled` tests** left forever.
- **Duplicate tests** that fail together for the same reason.
- **Order-dependent tests.**
- **Assertion-free tests.**

---

## 12. Organizing Test Code

| Ecosystem | Convention |
|---|---|
| **Java (Maven/Gradle)** | `src/test/java/...` mirrors `src/main/java/...`; class `OrderService` is tested by `OrderServiceTest` |
| **Node.js** | `*.test.js` or `*.spec.js` next to the source file, or in a `__tests__/` folder |
| **Python** | `tests/` folder with files named `test_*.py`; functions named `test_*` |

Separate fast unit tests from slower integration tests so developers can run unit tests constantly (for example by tag, folder or naming convention such as `*IT` in Java).

---

## 13. Quick Comparison of Tools

| | Java | Node.js | Python |
|---|---|---|---|
| Test runner | JUnit 5 | Jest / Vitest / `node:test` | pytest |
| Assertions | AssertJ, JUnit assertions | `expect` | plain `assert` |
| Mocking | Mockito | `jest.fn` / `vi.fn` | `unittest.mock`, pytest-mock |
| Coverage | JaCoCo | c8 / Istanbul | pytest-cov |

---

## 14. Practice Exercises

1. **Identify the type.** For each, say whether it is unit, integration or E2E:
   a) Testing a `calculateTax()` function with plain numbers.
   b) Testing a repository against a real PostgreSQL container.
   c) Using Playwright to log in and place an order.
2. **Fix the name.** Rename `test1()` for a function that rejects an empty email into a descriptive name.
3. **Boundaries.** List test inputs for `isAdult(age)` where adult means 18 or older.
4. **Spot the smell.** A test mocks the repository, the mapper and the validator, then only verifies those mocks were called. What is wrong?
5. **Write it.** Implement a `PasswordValidator` (min 8 chars, one digit, one uppercase) and write at least 6 tests in your preferred language, using AAA.
6. **Flaky hunt.** A test fails once in 20 runs and uses `Thread.sleep(500)` / `setTimeout`. List two ways to remove the flakiness.

---

## 15. Key Takeaways

- Many fast unit tests, fewer integration tests, very few E2E tests.
- Use Arrange-Act-Assert and give tests descriptive names.
- Follow FIRST: Fast, Isolated, Repeatable, Self-validating, Timely.
- Test behavior, not implementation details.
- Coverage finds gaps; it does not prove quality.
- Fix or remove flaky tests right away.

---

## 16. Further Reading

- *Unit Testing Principles, Practices, and Patterns* by Vladimir Khorikov
- *xUnit Test Patterns* by Gerard Meszaros
- *Growing Object-Oriented Software, Guided by Tests* by Steve Freeman and Nat Pryce
- Martin Fowler's articles on the Test Pyramid and Mocks Aren't Stubs (martinfowler.com)
- Official docs: [JUnit 5](https://junit.org/junit5/docs/current/user-guide/), [Jest](https://jestjs.io/docs/getting-started), [Vitest](https://vitest.dev/guide/), [pytest](https://docs.pytest.org/)
