# 02. Test Doubles

> Learn how to replace real collaborators (databases, APIs, clocks, email services) with controlled stand-ins so unit tests stay fast, isolated and deterministic.

---

## 1. What Is a Test Double?

A **test double** is any object that stands in for a real dependency during a test. The name comes from a "stunt double" in movies.

**Why use them?**
- Avoid slow or unreliable things: network, database, file system.
- Control the situation: force an error, return specific data, simulate a timeout.
- Avoid side effects: do not send a real email or charge a real card.
- Isolate the unit so a failure points at one place.

---

## 2. The Five Types (Meszaros Taxonomy)

| Type | Purpose | Verifies behavior? | Has logic? |
|---|---|---|---|
| **Dummy** | Fills a parameter that is never actually used | No | No |
| **Stub** | Returns canned answers to calls | No | No |
| **Fake** | Working but simplified implementation | No | Yes |
| **Spy** | Records how it was called (and often wraps the real object) | Yes, afterwards | Sometimes |
| **Mock** | Pre-programmed with expectations about calls; verifies them | Yes | No |

> In everyday conversation people say "mock" for all of them. Tools like Mockito, Jest and `unittest.mock` create objects that can act as stubs, spies or mocks depending on how you use them.

### Example scenario used below

```
OrderService  --->  PaymentGateway   (external API: charge a card)
              --->  OrderRepository  (database)
              --->  EmailSender      (sends confirmation)
```

We want to test `OrderService.placeOrder(...)` without calling any real service.

---

## 3. Dummy

An object passed only to satisfy a method signature. It is never used, and if it is used the test should blow up.

**Java**

```java
// EmailSender is required by the constructor but irrelevant to this test
OrderService service = new OrderService(repository, gateway, null);
// or: mock(EmailSender.class) with no stubbing
```

**JavaScript**

```js
const service = new OrderService(repository, gateway, {}); // dummy emailSender
```

**Python**

```python
service = OrderService(repository, gateway, email_sender=None)
```

---

## 4. Stub

Returns fixed values so the code under test follows a chosen path. You assert on the **result**, not on the stub.

**Java (Mockito)**

```java
@Test
void placeOrder_paymentApproved_returnsConfirmedOrder() {
    PaymentGateway gateway = mock(PaymentGateway.class);
    when(gateway.charge(any(), anyDouble())).thenReturn(PaymentResult.APPROVED); // stub

    OrderService service = new OrderService(repo, gateway, emailSender);

    Order order = service.placeOrder(cart);

    assertThat(order.getStatus()).isEqualTo(OrderStatus.CONFIRMED);
}
```

**JavaScript (Vitest; Jest uses `jest.fn()`)**

```js
it('confirms the order when payment is approved', async () => {
  const gateway = { charge: vi.fn().mockResolvedValue({ status: 'APPROVED' }) }; // stub
  const service = new OrderService(repo, gateway, emailSender);

  const order = await service.placeOrder(cart);

  expect(order.status).toBe('CONFIRMED');
});
```

**Python (unittest.mock)**

```python
from unittest.mock import Mock

def test_place_order_payment_approved_returns_confirmed_order():
    gateway = Mock()
    gateway.charge.return_value = "APPROVED"  # stub

    service = OrderService(repo, gateway, email_sender)

    order = service.place_order(cart)

    assert order.status == "CONFIRMED"
```

### Stubbing errors

Stubs are the best way to test failure paths that are hard to trigger for real.

```java
when(gateway.charge(any(), anyDouble())).thenThrow(new GatewayTimeoutException());
assertThrows(OrderFailedException.class, () -> service.placeOrder(cart));
```

```js
gateway.charge = vi.fn().mockRejectedValue(new Error('timeout'));
await expect(service.placeOrder(cart)).rejects.toThrow('timeout');
```

```python
gateway.charge.side_effect = TimeoutError("timeout")
with pytest.raises(OrderFailedError):
    service.place_order(cart)
```

---

## 5. Fake

A lightweight but **working** implementation. Unlike a stub, it has real behavior, just simplified (for example in-memory storage).

**Java: in-memory repository**

```java
class InMemoryOrderRepository implements OrderRepository {
    private final Map<Long, Order> store = new HashMap<>();
    private long nextId = 1;

    @Override
    public Order save(Order order) {
        if (order.getId() == null) order.setId(nextId++);
        store.put(order.getId(), order);
        return order;
    }

    @Override
    public Optional<Order> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }
}
```

**JavaScript**

```js
class InMemoryOrderRepository {
  constructor() { this.store = new Map(); this.nextId = 1; }
  async save(order) {
    if (!order.id) order.id = this.nextId++;
    this.store.set(order.id, order);
    return order;
  }
  async findById(id) { return this.store.get(id) ?? null; }
}
```

**Python**

```python
class InMemoryOrderRepository:
    def __init__(self):
        self._store, self._next_id = {}, 1

    def save(self, order):
        if order.id is None:
            order.id, self._next_id = self._next_id, self._next_id + 1
        self._store[order.id] = order
        return order

    def find_by_id(self, order_id):
        return self._store.get(order_id)
```

**When to prefer a fake:** several tests share the same dependency and need realistic state (save, then find). **Caution:** a fake can drift from the real implementation, so back it with a few integration tests (for example Testcontainers) against the real thing.

---

## 6. Spy

Records interactions so you can check them afterwards. A **partial spy** wraps a real object and lets you override only some methods.

**Java (Mockito `@Spy`)**

```java
List<String> list = spy(new ArrayList<>());
list.add("a");
verify(list).add("a");
assertThat(list).hasSize(1); // real behavior preserved
```

**JavaScript**

```js
const spy = vi.spyOn(emailSender, 'send'); // Jest: jest.spyOn
await service.placeOrder(cart);
expect(spy).toHaveBeenCalledTimes(1);
```

**Python**

```python
from unittest.mock import patch

with patch.object(email_sender, "send", wraps=email_sender.send) as spy:
    service.place_order(cart)
    spy.assert_called_once()
```

---

## 7. Mock

An object with **expectations about interactions**. The test passes only if the expected calls happened. Use mocks for side effects that have no return value to assert on (sending an email, publishing an event, writing an audit log).

**Java (Mockito)**

```java
@Test
void placeOrder_success_sendsConfirmationEmail() {
    when(gateway.charge(any(), anyDouble())).thenReturn(PaymentResult.APPROVED);

    service.placeOrder(cart);

    verify(emailSender).sendConfirmation(eq("user@example.com"), any(Order.class));
    verify(emailSender, times(1)).sendConfirmation(any(), any());
}

@Test
void placeOrder_paymentDeclined_neverSendsEmail() {
    when(gateway.charge(any(), anyDouble())).thenReturn(PaymentResult.DECLINED);

    assertThrows(PaymentDeclinedException.class, () -> service.placeOrder(cart));

    verifyNoInteractions(emailSender);
}
```

**JavaScript**

```js
it('sends a confirmation email on success', async () => {
  await service.placeOrder(cart);
  expect(emailSender.send).toHaveBeenCalledWith('user@example.com', expect.any(Object));
});

it('does not send an email when payment is declined', async () => {
  gateway.charge.mockResolvedValue({ status: 'DECLINED' });
  await expect(service.placeOrder(cart)).rejects.toThrow();
  expect(emailSender.send).not.toHaveBeenCalled();
});
```

**Python**

```python
def test_place_order_success_sends_confirmation_email():
    service.place_order(cart)
    email_sender.send_confirmation.assert_called_once_with("user@example.com", ANY)

def test_place_order_declined_never_sends_email():
    gateway.charge.return_value = "DECLINED"
    with pytest.raises(PaymentDeclinedError):
        service.place_order(cart)
    email_sender.send_confirmation.assert_not_called()
```

### Capturing arguments

When you need to inspect what was passed, capture it.

```java
ArgumentCaptor<Order> captor = ArgumentCaptor.forClass(Order.class);
verify(repo).save(captor.capture());
assertThat(captor.getValue().getTotal()).isEqualTo(250.0);
```

```js
const [savedOrder] = repo.save.mock.calls[0];
expect(savedOrder.total).toBe(250);
```

```python
saved_order = repo.save.call_args.args[0]
assert saved_order.total == 250
```

---

## 8. Stub vs Mock: State vs Behavior Verification

| | Stub | Mock |
|---|---|---|
| Question it answers | "What should this return when called?" | "Was this called correctly?" |
| What you assert on | Output or final state of the unit | The interaction itself |
| Typical use | Inputs and data feeding the unit | Outgoing side effects |

**Guideline:** prefer **state verification** (assert on results) wherever possible. Use **behavior verification** (verify calls) only for side effects that cannot be observed otherwise.

---

## 9. Library Cheat Sheet

| Concept | Java (Mockito) | JavaScript (Jest / Vitest) | Python (`unittest.mock` / pytest-mock) |
|---|---|---|---|
| Create mock | `mock(Class.class)` / `@Mock` | `jest.fn()` / `vi.fn()` | `Mock()` / `MagicMock()` |
| Stub return | `when(m.x()).thenReturn(v)` | `fn.mockReturnValue(v)` | `m.x.return_value = v` |
| Stub async | `thenReturn(CompletableFuture...)` | `mockResolvedValue(v)` | `AsyncMock(return_value=v)` |
| Stub error | `thenThrow(e)` | `mockRejectedValue(e)` / `mockImplementation(() => { throw e })` | `side_effect = e` |
| Verify called | `verify(m).x()` | `expect(fn).toHaveBeenCalled()` | `m.x.assert_called()` |
| Verify args | `verify(m).x(eq(1))` | `toHaveBeenCalledWith(1)` | `assert_called_with(1)` |
| Verify count | `times(n)` | `toHaveBeenCalledTimes(n)` | `call_count == n` |
| Never called | `never()` / `verifyNoInteractions` | `not.toHaveBeenCalled()` | `assert_not_called()` |
| Spy on real object | `spy(obj)` / `@Spy` | `spyOn(obj, 'method')` | `patch.object(obj, 'm', wraps=...)` |
| Replace a module | `mockStatic` (use sparingly) | `vi.mock('./mod')` / `jest.mock` | `patch('pkg.mod.name')` |
| Capture args | `ArgumentCaptor` | `fn.mock.calls` | `m.call_args` |

> **Python patching rule:** patch the name **where it is used**, not where it is defined. If `orders.py` does `from payments import charge`, patch `orders.charge`.

---

## 10. Handling Time, Randomness and IDs

These are classic hidden dependencies. Turn them into injectable test doubles.

**Time**

```java
// Java: inject java.time.Clock
Clock fixed = Clock.fixed(Instant.parse("2027-01-01T10:00:00Z"), ZoneOffset.UTC);
Service service = new Service(fixed);
```

```js
// Vitest / Jest fake timers
vi.useFakeTimers();
vi.setSystemTime(new Date('2027-01-01T10:00:00Z'));
// ...
vi.useRealTimers();
```

```python
# Python: freezegun, or inject a clock function
from freezegun import freeze_time

@freeze_time("2027-01-01 10:00:00")
def test_something(): ...
```

**Randomness / UUIDs:** inject a generator interface (`IdGenerator`) and use a fixed one in tests.

---

## 11. Best Practices

1. **Mock roles and boundaries, not everything.** Real value objects, DTOs and simple helpers should stay real.
2. **Don't mock what you don't own.** Wrap third-party APIs in your own adapter, mock the adapter, and test the adapter separately with an integration test (WireMock, MSW, Testcontainers).
3. **Prefer fakes for stateful collaborators** such as repositories.
4. **Verify outcomes first**; verify interactions only when the interaction *is* the outcome.
5. **Keep stubbing minimal.** Stub only what the scenario needs.
6. **Avoid mocking the class under test.** A partial spy on the unit itself is a design smell.
7. **Do not verify every call.** Over-verification locks tests to implementation.
8. **Use dependency injection** so doubles can be passed in. Constructor injection is the simplest.
9. **Back doubles with integration tests.** A mocked repository cannot catch a broken SQL query.
10. **Reset mocks between tests** (`@ExtendWith(MockitoExtension.class)`, `afterEach(vi.clearAllMocks)`, pytest fixtures).

---

## 12. Common Pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| **Over-mocking** | Test only proves that mocks were called | Use fakes or real objects; assert on results |
| **Brittle verification** | Refactor breaks tests with no behavior change | Verify fewer interactions |
| **Mocks that lie** | Mock behaves differently from the real thing, so tests pass but production fails | Add integration/contract tests |
| **Mocking static or final code** | Heavy tooling, hard-to-read tests | Wrap in an injectable interface |
| **Mock setup longer than the test** | Many `when(...)` lines | The unit has too many dependencies; refactor |
| **Wrong patch target (Python/JS)** | Real code still runs | Patch where the name is looked up |
| **Shared mock state** | Tests pass alone but fail together | Reset or recreate mocks per test |

### Two schools of thought

- **Classicist (Detroit/Chicago):** use real objects when practical; mock only awkward dependencies. Tests are sturdier against refactoring.
- **Mockist (London):** isolate every unit and mock its collaborators. Drives design outside-in but couples tests more to interactions.

Neither is "wrong". Many teams mix them: fakes and real objects inside a module, doubles at module boundaries.

---

## 13. Practice Exercises

1. **Classify.** Name each double: (a) `null` passed for an unused logger, (b) a repository returning a hard-coded user, (c) an in-memory map implementing `UserRepository`, (d) an object that asserts `send()` was called once.
2. **Stub the failure.** Write a test where `PaymentGateway.charge` throws a timeout and `placeOrder` must return a friendly error and save nothing.
3. **Mock the side effect.** Test that a declined payment never triggers an email.
4. **Build a fake.** Create an `InMemoryUserRepository` and use it to test `registerUser` rejecting duplicate emails.
5. **Fix the clock.** Write a `isOfferValid()` method that depends on the current time, then test "valid yesterday, expired tomorrow" using a fixed clock.
6. **Refactor over-mocking.** Take a test that mocks 5 collaborators and verifies 8 calls; rewrite it with a fake and a single outcome assertion.
7. **Repeat in a second language** for at least one exercise (Java and Python, or JavaScript).

---

## 14. Key Takeaways

- Five kinds of doubles: **dummy, stub, fake, spy, mock**.
- Stubs feed data in; mocks check side effects going out; fakes provide simplified working behavior.
- Prefer verifying **results**; verify **interactions** only for side effects.
- Wrap third-party code and don't mock what you don't own.
- Inject time, randomness and IDs so tests stay deterministic.
- Doubles speed up unit tests but never replace integration tests.

---

## 15. Further Reading

- Martin Fowler, *Mocks Aren't Stubs* (martinfowler.com/articles/mocksArentStubs.html)
- Gerard Meszaros, *xUnit Test Patterns* (the origin of the taxonomy)
- Mockito docs: https://site.mockito.org/
- Jest mock functions: https://jestjs.io/docs/mock-functions
- Vitest mocking: https://vitest.dev/guide/mocking
- Python `unittest.mock`: https://docs.python.org/3/library/unittest.mock.html
