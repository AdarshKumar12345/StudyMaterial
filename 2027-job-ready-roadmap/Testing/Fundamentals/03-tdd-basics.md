# 03. TDD Basics

> Test-Driven Development (TDD) means writing a failing test **before** writing the code that makes it pass. This guide teaches the cycle, the rules, and a full worked example in Java, JavaScript and Python.

---

## 1. What Is TDD?

TDD is a design and development technique where tests drive the code, in very small steps.

Instead of: *write code, then maybe write tests later*
You do: *write a tiny failing test, make it pass, clean up, repeat*

**What TDD gives you**
- Code that is testable by design
- A growing safety net of regression tests
- Small steps, so bugs are caught within minutes
- Simpler designs, because you only build what a test demands
- Living documentation of intended behavior

**What TDD is not**
- Not "write all tests first, then all code"
- Not a replacement for integration or E2E tests
- Not a guarantee of good design (you still need to think)

---

## 2. The Red-Green-Refactor Cycle

```
        +-----------+
        |   RED     |  Write a failing test
        +-----+-----+
              |
              v
        +-----------+
        |  GREEN    |  Write the simplest code to pass
        +-----+-----+
              |
              v
        +-----------+
        | REFACTOR  |  Clean up, keep tests green
        +-----+-----+
              |
              +-----> back to RED with the next test
```

| Phase | Goal | Rules |
|---|---|---|
| **Red** | Describe the next small behavior as a test | The test must fail, and fail for the **right reason** (not a typo or compile error) |
| **Green** | Make the test pass as quickly as possible | Write the **minimum** code. Hard-coding is allowed here |
| **Refactor** | Improve structure, names, duplication | Behavior must not change. All tests stay green. No new features |

**Typical cycle length:** 1 to 10 minutes. If you are red for 30 minutes, the step was too big.

---

## 3. The Three Laws of TDD (Robert C. Martin)

1. You may not write production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail (and not compiling counts as failing).
3. You may not write more production code than is sufficient to pass the currently failing test.

These laws are strict on purpose. In daily work, treat them as training wheels that build the rhythm.

---

## 4. Worked Example: String Calculator Kata

**Goal:** `add(String numbers)` returns the sum of comma-separated numbers.

**Test list (plan first, check items off):**
- [ ] Empty string returns 0
- [ ] Single number returns that number
- [ ] Two numbers return their sum
- [ ] Any amount of numbers
- [ ] New lines between numbers are also separators
- [ ] Custom delimiter: `//;\n1;2`
- [ ] Negative numbers throw an exception listing them
- [ ] Numbers greater than 1000 are ignored

### Java walkthrough (JUnit 5 + AssertJ)

**Cycle 1, RED: empty string returns 0**

```java
class StringCalculatorTest {
    @Test
    void add_emptyString_returnsZero() {
        assertThat(new StringCalculator().add("")).isEqualTo(0);
    }
}
```

It does not even compile because `StringCalculator` does not exist. That counts as red. Create the class with a stub method, run it, and watch it fail.

**GREEN: simplest thing, even a constant**

```java
class StringCalculator {
    int add(String numbers) {
        return 0;
    }
}
```

**Cycle 2, RED: single number**

```java
@Test
void add_singleNumber_returnsThatNumber() {
    assertThat(new StringCalculator().add("5")).isEqualTo(5);
}
```

**GREEN**

```java
int add(String numbers) {
    if (numbers.isEmpty()) return 0;
    return Integer.parseInt(numbers);
}
```

**Cycle 3, RED: two numbers**

```java
@Test
void add_twoNumbers_returnsSum() {
    assertThat(new StringCalculator().add("1,2")).isEqualTo(3);
}
```

**GREEN**

```java
int add(String numbers) {
    if (numbers.isEmpty()) return 0;
    String[] parts = numbers.split(",");
    int sum = 0;
    for (String part : parts) sum += Integer.parseInt(part);
    return sum;
}
```

This already handles any amount of numbers, so the "any amount" test passes right away. That is fine; still add it as a regression test.

**REFACTOR** (tests stay green)

```java
int add(String numbers) {
    if (numbers.isEmpty()) return 0;
    return Arrays.stream(numbers.split(","))
                 .mapToInt(Integer::parseInt)
                 .sum();
}
```

**Cycle 4, RED: negative numbers throw**

```java
@Test
void add_negativeNumbers_throwsWithAllNegativesInMessage() {
    assertThatThrownBy(() -> new StringCalculator().add("1,-2,-5"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageContaining("-2")
        .hasMessageContaining("-5");
}
```

Then GREEN, REFACTOR, and continue down the list one test at a time.

### JavaScript version (Vitest)

```js
import { describe, it, expect } from 'vitest';
import { add } from './stringCalculator.js';

describe('add', () => {
  it('returns 0 for an empty string', () => {
    expect(add('')).toBe(0);
  });
  it('returns the number for a single value', () => {
    expect(add('5')).toBe(5);
  });
  it('sums two numbers', () => {
    expect(add('1,2')).toBe(3);
  });
  it('throws and lists negatives', () => {
    expect(() => add('1,-2,-5')).toThrow(/-2.*-5/);
  });
});
```

```js
// stringCalculator.js after a few cycles
export function add(numbers) {
  if (numbers === '') return 0;
  const values = numbers.split(',').map(Number);
  const negatives = values.filter(n => n < 0);
  if (negatives.length) throw new Error(`negatives not allowed: ${negatives.join(', ')}`);
  return values.reduce((a, b) => a + b, 0);
}
```

### Python version (pytest)

```python
import pytest
from string_calculator import add

def test_empty_string_returns_zero():
    assert add("") == 0

def test_single_number():
    assert add("5") == 5

def test_two_numbers():
    assert add("1,2") == 3

def test_negatives_raise_with_all_listed():
    with pytest.raises(ValueError, match="-2.*-5"):
        add("1,-2,-5")
```

```python
# string_calculator.py after a few cycles
def add(numbers: str) -> int:
    if not numbers:
        return 0
    values = [int(n) for n in numbers.split(",")]
    negatives = [n for n in values if n < 0]
    if negatives:
        raise ValueError(f"negatives not allowed: {negatives}")
    return sum(values)
```

---

## 5. Techniques for Going from Red to Green

| Technique | How it works | When to use |
|---|---|---|
| **Fake it** | Return a hard-coded constant, then generalize as more tests arrive | When you are unsure of the design; keeps steps tiny |
| **Triangulation** | Add a second example that forces the real logic | After faking, to remove the constant |
| **Obvious implementation** | Just write the real code | When it is simple and you are confident |

**Triangulation example**

```
Test 1: add("1")   -> 1    (fake: return 1)
Test 2: add("5")   -> 5    (forces: return Integer.parseInt(numbers))
```

If the obvious implementation surprises you with a failing test, step back and take smaller steps.

---

## 6. Refactoring Rules While in Green

Refactor only when all tests pass. Common moves:
- Rename variables and methods for clarity
- Extract method / extract variable
- Remove duplication (in production **and** test code)
- Replace magic numbers with named constants
- Simplify conditionals

After each small change, rerun the tests. If something turns red, undo the last change.

---

## 7. Choosing the Next Test

Pick the test that teaches you something new with the smallest code step:

1. Degenerate cases first (empty, zero, null)
2. Simplest "happy path" example
3. Slight variations (two items, then many)
4. Edge cases and boundaries
5. Error conditions

Keep a **test list** in a scratch file or comment. When you think of a new case mid-cycle, add it to the list instead of switching context.

---

## 8. TDD for Bug Fixes

1. Reproduce the bug with a **failing test** (red).
2. Fix the code until the test passes (green).
3. Refactor if needed.
4. The test stays forever as a regression guard.

This is the highest-value, lowest-friction way to start using TDD on an existing codebase.

---

## 9. Two Styles: Inside-Out vs Outside-In

| | Inside-Out (Classicist / Chicago) | Outside-In (London / Mockist) |
|---|---|---|
| Start at | Core domain logic | The outer edge (controller, API, use case) |
| Collaborators | Real objects where possible | Test doubles for not-yet-built layers |
| Strength | Solid domain model, less mocking | Drives interfaces from real use cases |
| Risk | May build unneeded pieces | More mocks, coupled to interactions |

Beginners usually find inside-out easier. See *Test Doubles* (02) for the supporting techniques.

---

## 10. TDD in a Spring Boot / API Context

Do not start with the full application context. Climb layers:

1. **Domain/service logic:** pure unit tests (fast TDD loop).
2. **Controller:** `@WebMvcTest` with `MockMvc`; mock the service.
3. **Repository:** `@DataJpaTest` or Testcontainers.
4. **One end-to-end happy path** at the end to confirm wiring.

Example first test for a REST endpoint (red):

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired MockMvc mvc;
    @MockitoBean UserService service; // @MockBean on older Spring Boot versions

    @Test
    void getUser_existing_returns200AndBody() throws Exception {
        when(service.find(1L)).thenReturn(new UserDto(1L, "Asha"));

        mvc.perform(get("/users/1"))
           .andExpect(status().isOk())
           .andExpect(jsonPath("$.name").value("Asha"));
    }
}
```

Same idea in Node.js (Supertest) and Python (FastAPI `TestClient`): write the failing request test first, then add the route.

---

## 11. Benefits and Honest Trade-offs

**Benefits**
- Fewer defects and less debugging time
- Confidence to refactor
- Naturally modular, loosely coupled code
- Tests double as examples

**Costs and limits**
- Slower at first while you build the habit
- Needs discipline and testable design
- Poor fit for exploratory spikes, UI pixel-tweaking, or throwaway scripts (spike first, then rewrite test-first)
- Bad tests can create false confidence

> TDD is a tool, not a religion. Use it where it pays off: business logic, algorithms, bug fixes and APIs.

---

## 12. Common Mistakes

| Mistake | Better approach |
|---|---|
| Writing production code before a failing test | Always see red first |
| Steps too big | Shrink the test; aim for minutes per cycle |
| Skipping the refactor step | Refactor every cycle, even if tiny |
| Test passes immediately on first run | Suspicious; check it can actually fail (temporarily break the code) |
| Testing private methods | Test through public behavior |
| Asserting on implementation details | Assert observable outcomes |
| Over-mocking | Prefer real objects and fakes where practical |
| Gold-plating the green step | Only write what the test requires |
| Ignoring slow tests | Keep the TDD loop under a few seconds |

---

## 13. Tooling Tips for a Fast Loop

- **Java:** run tests from the IDE (IntelliJ shortcut), use Maven/Gradle test filters, and keep unit tests free of Spring context.
- **Node.js:** `vitest` or `jest --watch` reruns affected tests on save.
- **Python:** `pytest -x -q` (stop on first failure), plus `pytest-watch` or `pytest-testmon`.
- Use **continuous test runners** so feedback arrives in seconds.
- Commit on green. A common rhythm is: red, green, refactor, **commit**.

---

## 14. Katas to Practice

Do each in Java, JavaScript and Python if possible, and repeat them until the cycle feels natural.

| Kata | Skills practiced |
|---|---|
| **FizzBuzz** | The basic cycle, triangulation |
| **String Calculator** | Incremental requirements, exceptions |
| **Roman Numerals** | Algorithm emerging from tests |
| **Bank Account** | State, validation, statements |
| **Prime Factors** | Tiny steps, refactoring |
| **Bowling Game** | Domain modeling, edge cases |
| **Tennis Score** | Rules and state transitions |
| **Gilded Rose** | Refactoring legacy code with tests |
| **Mars Rover** | Commands, boundaries, design |

---

## 15. Practice Exercises

1. **Cycle drill.** Do FizzBuzz with a commit after every green. You should end with 8 to 12 small commits.
2. **Fake it.** Implement Roman Numerals starting with `1 -> "I"` returning a constant, then triangulate with `2`, `3`, `4`, `5`.
3. **Bug-first.** Take any function you wrote earlier, find one edge-case bug, write the failing test, then fix it.
4. **Test list.** Before coding a `PasswordValidator`, write a list of at least 10 test cases ordered from simplest to hardest, then implement it test by test.
5. **Outside-in.** Build `POST /users` by starting with a failing controller test, then driving the service with a mock, then the real service.
6. **Reflect.** After one kata, write 3 sentences: where did you take steps too large, where did tests guide your design, and what would you do differently?

---

## 16. Key Takeaways

- **Red, Green, Refactor**: failing test, minimal code, clean up.
- Keep steps tiny and keep a test list.
- Always watch the test fail first, and for the right reason.
- Refactor on green only; commit often.
- Use TDD for logic-heavy code and bug fixes; spike first when you are exploring.
- Practice with katas until the rhythm is automatic.

---

## 17. Further Reading

- Kent Beck, *Test-Driven Development: By Example*
- Steve Freeman and Nat Pryce, *Growing Object-Oriented Software, Guided by Tests*
- Robert C. Martin, *Clean Code* (TDD chapters) and the "Three Laws of TDD"
- Roy Osherove, *The Art of Unit Testing*
- Martin Fowler, *TestDrivenDevelopment* (martinfowler.com/bliki/TestDrivenDevelopment.html)
- Kata collections: [cyber-dojo.org](https://cyber-dojo.org), [codingdojo.org](https://codingdojo.org)
