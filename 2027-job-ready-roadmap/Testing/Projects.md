38. Java Capstone Test Suite
39. Node.js API Test Suite
40. Python API Test Suite
41. Full-Stack E2E Suite
42. TDD Katas (Java, JavaScript, Python)


# Capstone Test Suites and TDD Katas

> **Goal:** stop learning testing in theory and build real test suites. Topics 38 to 41 each give you a project to test with the tools from earlier topics. Topic 42 builds the habit with small daily katas.

**How to use this section**
1. Pick a project (or reuse a capstone from the Production Capstones module)
2. Follow the steps in order and tick the checklists
3. Push to GitHub with the CI workflow from topic 37 so every push runs your tests

---

# 38. Java Capstone Test Suite

## What you will build
A complete test suite for a **URL Shortener** (Spring Boot 3 + JPA + PostgreSQL), covering all four layers:

| Layer | Tool | What it tests | Speed |
|---|---|---|---|
| Unit | JUnit 5 + Mockito + AssertJ | `UrlService`, `ShortCodeGenerator` | Milliseconds |
| Web slice | `@WebMvcTest` + MockMvc | `UrlController` (status codes, JSON, redirect) | Fast |
| Data slice | `@DataJpaTest` + Testcontainers | `UrlRepository` against real PostgreSQL | Seconds |
| Full flow | `@SpringBootTest` + Testcontainers + REST Assured | Create a short link, then follow it | Slower |

## Before you start
- [ ] Docker is installed and running (Testcontainers needs it)
- [ ] Java 21 and Maven 3.9+
- [ ] URL Shortener project builds with `mvn verify`

## Step 1: Add test dependencies (`pom.xml`)

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-testcontainers</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.testcontainers</groupId>
  <artifactId>junit-jupiter</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.testcontainers</groupId>
  <artifactId>postgresql</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>io.rest-assured</groupId>
  <artifactId>rest-assured</artifactId>
  <scope>test</scope>
</dependency>
```

`spring-boot-starter-test` already includes JUnit 5, Mockito and AssertJ. Spring Boot manages the versions of these, so you usually do not write `<version>`.

## Step 2: Folder structure

```
src/test/java/com/example/shortener/
├── service/
│   ├── UrlServiceTest.java            # unit (Mockito)
│   └── ShortCodeGeneratorTest.java    # unit (plain)
├── web/
│   └── UrlControllerTest.java         # @WebMvcTest
├── repository/
│   └── UrlRepositoryTest.java         # @DataJpaTest + Testcontainers
└── UrlShortenerIT.java                # full flow (name ends with IT)
```

## Step 3: Unit tests (start here, they are the fastest)

```java
@ExtendWith(MockitoExtension.class)
class UrlServiceTest {

    @Mock UrlRepository repository;
    @Mock ShortCodeGenerator generator;
    @InjectMocks UrlService service;

    @Test
    void shorten_validUrl_savesAndReturnsCode() {
        when(generator.generate()).thenReturn("abc123");
        when(repository.save(any(ShortUrl.class))).thenAnswer(inv -> inv.getArgument(0));

        String code = service.shorten("https://example.com");

        assertThat(code).isEqualTo("abc123");
        verify(repository).save(any(ShortUrl.class));
    }

    @Test
    void shorten_invalidUrl_throwsAndSavesNothing() {
        assertThatThrownBy(() -> service.shorten("not-a-url"))
            .isInstanceOf(IllegalArgumentException.class);

        verifyNoInteractions(repository);
    }
}
```

## Step 4: Controller slice test

```java
@WebMvcTest(UrlController.class)
class UrlControllerTest {

    @Autowired MockMvc mvc;
    @MockitoBean UrlService service;   // use @MockBean on Spring Boot older than 3.4

    @Test
    void redirect_existingCode_returns302WithLocation() throws Exception {
        when(service.resolve("abc123")).thenReturn("https://example.com");

        mvc.perform(get("/abc123"))
           .andExpect(status().isFound())
           .andExpect(header().string("Location", "https://example.com"));
    }

    @Test
    void redirect_unknownCode_returns404() throws Exception {
        when(service.resolve("nope")).thenThrow(new UrlNotFoundException("nope"));

        mvc.perform(get("/nope"))
           .andExpect(status().isNotFound());
    }
}
```

> This assumes you have an `@ControllerAdvice` that maps `UrlNotFoundException` to 404. If not, write the failing test first and then add the handler (TDD).

## Step 5: Repository test with a real PostgreSQL (Testcontainers)

```java
@DataJpaTest
@Testcontainers
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class UrlRepositoryTest {

    @Container
    @ServiceConnection
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @Autowired UrlRepository repository;

    @Test
    void findByCode_existing_returnsEntity() {
        repository.save(new ShortUrl("abc123", "https://example.com"));

        Optional<ShortUrl> found = repository.findByCode("abc123");

        assertThat(found).isPresent();
        assertThat(found.get().getOriginalUrl()).isEqualTo("https://example.com");
    }

    @Test
    void findByCode_missing_returnsEmpty() {
        assertThat(repository.findByCode("missing")).isEmpty();
    }
}
```

`@ServiceConnection` (Spring Boot 3.1+) wires the container's URL, username and password into Spring automatically.

## Step 6: Full-flow integration test

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Testcontainers
class UrlShortenerIT {

    @Container
    @ServiceConnection
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @LocalServerPort int port;

    @BeforeEach
    void setUp() {
        RestAssured.port = port;
    }

    @Test
    void createThenRedirect_fullFlow() {
        String code = given()
                .contentType(ContentType.JSON)
                .body("{\"url\":\"https://example.com\"}")
            .when().post("/api/urls")
            .then().statusCode(201)
                .extract().path("code");

        given().redirects().follow(false)
            .when().get("/" + code)
            .then().statusCode(302)
                .header("Location", "https://example.com");
    }
}
```

Static imports you need: `io.restassured.RestAssured.given`, `org.assertj.core.api.Assertions.*`, `org.mockito.Mockito.*`, and the MockMvc request/result builders.

## Step 7: Add coverage and CI
- Add JaCoCo with a 70% line check (see topic 37, section 4.2)
- Add the GitHub Actions workflow from topic 37 (`mvn -B verify`)
- Add the build badge to your README

## Test case checklist for a URL Shortener

**Unit**
- [ ] Valid URL returns a short code
- [ ] Invalid, empty and null URL are rejected
- [ ] Code collision triggers a retry
- [ ] Same URL submitted twice (decide the rule, then test it)
- [ ] Expired link is not resolved

**Web**
- [ ] `POST /api/urls` returns 201 and JSON with `code`
- [ ] Invalid body returns 400 with an error message
- [ ] Unknown code returns 404
- [ ] Redirect returns 302 with the `Location` header

**Repository / integration**
- [ ] Save then find by code
- [ ] Unique constraint on `code` is enforced
- [ ] Click counter increments on redirect
- [ ] Full flow: create, then redirect

## Done when
- [ ] `mvn test` finishes in seconds (unit tests only)
- [ ] `mvn verify` runs everything and is green
- [ ] Coverage is at least 70% on the service package
- [ ] CI is green on GitHub and the badge is in your README

## Learn more
- GFG: [Unit Testing in Spring Boot using Mockito and JUnit](https://www.geeksforgeeks.org/unit-testing-in-spring-boot-project-using-mockito-and-junit/)
- GFG: [Spring Boot MockMvc Testing with Example Project](https://www.geeksforgeeks.org/spring-boot-mockmvc-testing-with-example-project/)
- GFG: [Testing in Spring Boot](https://www.geeksforgeeks.org/testing-in-spring-boot/)
- Official: [Spring Boot testing reference](https://docs.spring.io/spring-boot/reference/testing/index.html)
- Official: [Testcontainers guides](https://testcontainers.com/guides/)
- Official: [REST Assured](https://rest-assured.io/)

---

# 39. Node.js API Test Suite

## What you will build
A test suite for an **Express + PostgreSQL Notes API** using **Vitest**, **Supertest** and **Testcontainers**.

| Layer | Tool | What it tests |
|---|---|---|
| Unit | Vitest + `vi.fn()` | Service logic with a fake repository |
| API (integration) | Supertest + real PostgreSQL in Docker | Routes, validation, status codes, database |
| Coverage | `@vitest/coverage-v8` | Gaps in your tests |

Prefer Jest? The syntax is almost identical: replace `vi` with `jest` and drop the Vitest imports.

## Before you start
- [ ] Node.js 20 or newer, Docker running
- [ ] A small Express API with `GET/POST /notes` (or build one first)

## Step 1: Install

```bash
npm install express pg
npm install -D vitest supertest @testcontainers/postgresql @vitest/coverage-v8
```

`package.json`:

```json
{
  "type": "module",
  "scripts": {
    "start": "node src/server.js",
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```

## Step 2: Structure your app so it can be tested

**Golden rule:** the Express `app` and the server `listen` must be in different files. Supertest needs the app without a running server.

```
src/
├── app.js            # createApp({ pool }) returns the Express app, NO listen()
├── server.js         # imports app and calls listen()
├── db.js             # createPool(connectionString)
├── noteService.js
└── noteRepository.js
tests/
├── unit/
│   └── noteService.test.js
└── integration/
    └── notes.api.test.js
```

## Step 3: Unit test with a fake repository

```js
import { describe, it, expect, vi } from 'vitest';
import { NoteService } from '../../src/noteService.js';

describe('NoteService', () => {
  it('rejects an empty title and does not touch the database', async () => {
    const repo = { create: vi.fn() };
    const service = new NoteService(repo);

    await expect(service.create({ title: '' })).rejects.toThrow('title is required');
    expect(repo.create).not.toHaveBeenCalled();
  });

  it('saves a valid note', async () => {
    const repo = { create: vi.fn().mockResolvedValue({ id: 1, title: 'Buy milk' }) };
    const service = new NoteService(repo);

    const note = await service.create({ title: 'Buy milk' });

    expect(note.id).toBe(1);
    expect(repo.create).toHaveBeenCalledWith({ title: 'Buy milk' });
  });
});
```

## Step 4: API test with Supertest + real PostgreSQL

```js
import { describe, it, expect, beforeAll, afterAll, beforeEach } from 'vitest';
import request from 'supertest';
import { PostgreSqlContainer } from '@testcontainers/postgresql';
import { createApp } from '../../src/app.js';
import { createPool } from '../../src/db.js';

let container, pool, app;

beforeAll(async () => {
  container = await new PostgreSqlContainer('postgres:16').start();
  pool = createPool(container.getConnectionUri());
  await pool.query('CREATE TABLE notes (id SERIAL PRIMARY KEY, title TEXT NOT NULL)');
  app = createApp({ pool });
}, 60_000);                       // starting Docker can take a while

afterAll(async () => {
  await pool.end();
  await container.stop();
});

beforeEach(async () => {
  await pool.query('TRUNCATE notes RESTART IDENTITY');   // clean state for every test
});

describe('POST /notes', () => {
  it('creates a note and returns 201', async () => {
    const res = await request(app).post('/notes').send({ title: 'Buy milk' });

    expect(res.status).toBe(201);
    expect(res.body).toMatchObject({ id: 1, title: 'Buy milk' });
  });

  it('returns 400 when title is missing', async () => {
    const res = await request(app).post('/notes').send({});

    expect(res.status).toBe(400);
    expect(res.body.error).toMatch(/title/i);
  });
});

describe('GET /notes/:id', () => {
  it('returns 404 for an unknown id', async () => {
    const res = await request(app).get('/notes/999');
    expect(res.status).toBe(404);
  });
});
```

## Step 5: Coverage threshold

`vitest.config.js`:

```js
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    coverage: {
      provider: 'v8',
      thresholds: { lines: 80 }
    }
  }
});
```

Run `npm run test:coverage`. The build fails if coverage drops below 80%.

## Test case checklist

- [ ] `POST /notes` valid input returns 201
- [ ] `POST /notes` missing or empty title returns 400
- [ ] `GET /notes` returns an empty array at first, then your notes
- [ ] `GET /notes/:id` found returns 200, unknown returns 404
- [ ] `PUT/PATCH` and `DELETE` work (and 404 for unknown ids)
- [ ] Protected routes return 401 without a token and 403 with the wrong role (if you added JWT)
- [ ] Malformed JSON returns 400, not 500
- [ ] Error handler hides stack traces
- [ ] A database failure returns 500 with a safe message

## Done when
- [ ] `npm test` is green and every test is independent (run them in any order)
- [ ] Unit tests finish in under a second
- [ ] Coverage is at least 80%
- [ ] CI is green (Node workflow from topic 37)

## Common mistakes
| Mistake | Fix |
|---|---|
| `app.listen()` inside `app.js` | Move it to `server.js` |
| Tests depend on each other's data | `TRUNCATE` in `beforeEach` |
| Timeout while the container starts | Give `beforeAll` a longer timeout (60 s) |
| Forgetting `await` before `request(app)` | Always `await` Supertest calls |

## Learn more
- GFG: [Testing with Jest](https://www.geeksforgeeks.org/javascript/testing-with-jest)
- Official: [Vitest guide](https://vitest.dev/guide/)
- Official: [Supertest](https://github.com/ladjs/supertest)
- Official: [Testcontainers for Node.js](https://node.testcontainers.org/)

---

# 40. Python API Test Suite

## What you will build
A test suite for a **FastAPI + SQLAlchemy + PostgreSQL Items API** using **pytest**, **TestClient** and **Testcontainers**.

| Layer | Tool | What it tests |
|---|---|---|
| Unit | pytest + `pytest-mock` | Service functions with mocked dependencies |
| API (integration) | `TestClient` + real PostgreSQL in Docker | Endpoints, validation, database |
| Quality | `pytest-cov` | Coverage gaps |

## Before you start
- [ ] Python 3.11 or newer, Docker running
- [ ] A FastAPI app with `POST /items` and `GET /items/{id}`

## Step 1: Install

```bash
pip install fastapi uvicorn sqlalchemy psycopg2-binary
pip install pytest pytest-cov pytest-mock httpx "testcontainers[postgres]"
```

`httpx` is required by FastAPI's `TestClient`.

## Step 2: Structure

```
app/
├── main.py            # FastAPI app, uses Depends(get_db)
├── db.py              # engine, Base, get_db()
├── models.py
└── service.py
tests/
├── conftest.py        # shared fixtures
├── unit/
│   └── test_service.py
└── integration/
    └── test_items_api.py
pytest.ini
```

`pytest.ini`:

```ini
[pytest]
pythonpath = .
testpaths = tests
markers =
    integration: tests that need Docker
```

## Step 3: Fixtures in `tests/conftest.py`

```python
import pytest
from fastapi.testclient import TestClient
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from testcontainers.postgres import PostgresContainer

from app.main import app
from app.db import Base, get_db


@pytest.fixture(scope="session")
def engine():
    with PostgresContainer("postgres:16") as pg:      # starts Docker once per test run
        engine = create_engine(pg.get_connection_url())
        Base.metadata.create_all(engine)
        yield engine
        engine.dispose()


@pytest.fixture
def db_session(engine):
    """Each test runs inside a transaction that is rolled back = clean database."""
    connection = engine.connect()
    transaction = connection.begin()
    session = sessionmaker(
        bind=connection, join_transaction_mode="create_savepoint"
    )()
    yield session
    session.close()
    transaction.rollback()
    connection.close()


@pytest.fixture
def client(db_session):
    def override_get_db():
        yield db_session

    app.dependency_overrides[get_db] = override_get_db
    yield TestClient(app)
    app.dependency_overrides.clear()
```

## Step 4: Unit test with a mock

```python
# tests/unit/test_service.py
import pytest
from app.service import create_item


def test_create_item_rejects_negative_price(mocker):
    repo = mocker.Mock()

    with pytest.raises(ValueError, match="price"):
        create_item(repo, name="Pen", price=-1)

    repo.save.assert_not_called()


def test_create_item_saves_valid_item(mocker):
    repo = mocker.Mock()
    repo.save.return_value = {"id": 1, "name": "Pen", "price": 10}

    item = create_item(repo, name="Pen", price=10)

    assert item["id"] == 1
    repo.save.assert_called_once()
```

## Step 5: API tests with parametrize

```python
# tests/integration/test_items_api.py
import pytest

pytestmark = pytest.mark.integration


def test_create_item_returns_201(client):
    r = client.post("/items", json={"name": "Pen", "price": 10})

    assert r.status_code == 201
    assert r.json()["name"] == "Pen"


@pytest.mark.parametrize(
    "payload",
    [
        {},                              # missing everything
        {"name": "", "price": 10},       # empty name
        {"name": "Pen", "price": -1},    # negative price
        {"name": "Pen", "price": "abc"}, # wrong type
    ],
)
def test_create_item_invalid_returns_422(client, payload):
    r = client.post("/items", json=payload)
    assert r.status_code == 422


def test_get_unknown_item_returns_404(client):
    assert client.get("/items/9999").status_code == 404


def test_create_then_get_roundtrip(client):
    created = client.post("/items", json={"name": "Pen", "price": 10}).json()

    r = client.get(f"/items/{created['id']}")

    assert r.status_code == 200
    assert r.json() == created
```

For the 422 tests to pass, your Pydantic model needs constraints, for example `name: str = Field(min_length=1)` and `price: float = Field(gt=0)`. That is TDD in action: write the test, watch it fail, add the constraint.

## Step 6: Run it

```bash
pytest                                   # everything
pytest -m "not integration"              # fast tests only (no Docker)
pytest --cov=app --cov-report=term-missing --cov-fail-under=80
pytest -x -q                             # stop at first failure, short output
```

## Test case checklist

- [ ] Create item: valid returns 201
- [ ] Create item: missing, empty and wrong-type fields return 422 (parametrized)
- [ ] Get item: found returns 200, missing returns 404
- [ ] List items: empty list, then several items, pagination
- [ ] Update and delete work, and 404 for unknown ids
- [ ] Auth: missing token returns 401 (if you added it)
- [ ] Each test leaves no data behind (rollback fixture works)
- [ ] Async endpoints tested with `pytest-asyncio` + `httpx.AsyncClient` (bonus)

## Done when
- [ ] `pytest -m "not integration"` runs in under 2 seconds
- [ ] `pytest` is green with Docker
- [ ] Coverage is at least 80%
- [ ] CI is green (Python workflow from topic 37)

## Common mistakes
| Mistake | Fix |
|---|---|
| Tests leave data in the database | Use the rollback fixture above |
| `ModuleNotFoundError: app` | Add `pythonpath = .` to `pytest.ini` |
| Patching the wrong place | Patch where the name is used, not where it is defined |
| Starting a new container per test | Keep the container fixture `scope="session"` |

## Learn more
- GFG: [Testing FastAPI Application](https://www.geeksforgeeks.org/testing-fastapi-application/)
- GFG: [Getting Started with Pytest](https://www.geeksforgeeks.org/python/getting-started-with-pytest)
- GFG: [Pytest Tutorial: Unit Testing in Python](https://www.geeksforgeeks.org/?p=1201410)
- Official: [FastAPI testing](https://fastapi.tiangolo.com/tutorial/testing/)
- Official: [pytest docs](https://docs.pytest.org/)
- Official: [Testcontainers for Python](https://testcontainers-python.readthedocs.io/)

---

# 41. Full-Stack E2E Suite

## What you will build
End-to-end tests for a **React frontend + your backend** (URL Shortener or Notes app) using **Playwright**, plus a few **React Testing Library** component tests.

| Layer | Tool | What it tests |
|---|---|---|
| Component | Vitest + React Testing Library | One component in isolation (form, list) |
| E2E | Playwright | A real browser clicking through the real app |

**Rule:** keep E2E tests few. Test only the 3 to 5 most important user journeys, because they are slow and the hardest to keep stable.

## Before you start
- [ ] Frontend and backend run locally
- [ ] You know the 3 to 5 key user journeys, for example:
  1. Sign up and log in
  2. Create an item (shorten a URL, add a note)
  3. See it in the list
  4. Delete it
  5. See an error message for invalid input

## Step 1: Install Playwright

```bash
npm init playwright@latest
```

Choose TypeScript (or JavaScript), put tests in `e2e`, and say yes to the GitHub Actions workflow.

## Step 2: Configure it (`playwright.config.ts`)

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  retries: process.env.CI ? 2 : 0,
  reporter: [['html'], ['list']],
  use: {
    baseURL: 'http://localhost:5173',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [{ name: 'chromium', use: { ...devices['Desktop Chrome'] } }],

  // Playwright starts your apps for you
  webServer: [
    {
      command: 'npm run start:api',
      url: 'http://localhost:8080/actuator/health',
      reuseExistingServer: !process.env.CI,
    },
    {
      command: 'npm run dev',
      url: 'http://localhost:5173',
      reuseExistingServer: !process.env.CI,
    },
  ],
});
```

Change the commands and URLs to match your project.

## Step 3: Your first E2E test

```ts
// e2e/shorten.spec.ts
import { test, expect } from '@playwright/test';

test('user can shorten a URL', async ({ page }) => {
  await page.goto('/');

  await page.getByLabel('Long URL').fill('https://example.com/a/very/long/path');
  await page.getByRole('button', { name: 'Shorten' }).click();

  await expect(page.getByTestId('short-link')).toBeVisible();
});

test('shows an error for an invalid URL', async ({ page }) => {
  await page.goto('/');

  await page.getByLabel('Long URL').fill('not-a-url');
  await page.getByRole('button', { name: 'Shorten' }).click();

  await expect(page.getByText('Please enter a valid URL')).toBeVisible();
});
```

## Step 4: Write stable locators

| Use first | Avoid |
|---|---|
| `getByRole('button', { name: 'Save' })` | `.btn-primary > span:nth-child(2)` |
| `getByLabel('Email')` | XPath |
| `getByText('Welcome')` | Fixed `waitForTimeout(3000)` |
| `getByTestId('short-link')` (add `data-testid` when nothing else fits) | Selectors tied to CSS classes |

Playwright **waits automatically** for elements, so you almost never need manual waits.

## Step 5: Page Object pattern (keeps tests readable)

```ts
// e2e/pages/HomePage.ts
import { Page, expect } from '@playwright/test';

export class HomePage {
  constructor(private page: Page) {}

  async open() {
    await this.page.goto('/');
  }

  async shorten(url: string) {
    await this.page.getByLabel('Long URL').fill(url);
    await this.page.getByRole('button', { name: 'Shorten' }).click();
  }

  async expectShortLinkVisible() {
    await expect(this.page.getByTestId('short-link')).toBeVisible();
  }
}
```

```ts
test('shorten with page object', async ({ page }) => {
  const home = new HomePage(page);
  await home.open();
  await home.shorten('https://example.com');
  await home.expectShortLinkVisible();
});
```

## Step 6: Log in once, reuse everywhere

Logging in through the UI before every test is slow. Do it once and save the session.

```ts
// e2e/auth.setup.ts
import { test as setup } from '@playwright/test';

setup('log in', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('test@example.com');
  await page.getByLabel('Password').fill('Password123!');
  await page.getByRole('button', { name: 'Log in' }).click();
  await page.waitForURL('/dashboard');
  await page.context().storageState({ path: 'e2e/.auth/user.json' });
});
```

In `playwright.config.ts`, replace `projects` with:

```ts
projects: [
  { name: 'setup', testMatch: /auth\.setup\.ts/ },
  {
    name: 'chromium',
    use: { ...devices['Desktop Chrome'], storageState: 'e2e/.auth/user.json' },
    dependencies: ['setup'],
  },
],
```

Add `e2e/.auth/` to `.gitignore`.

## Step 7: Prepare test data through the API (fast), not the UI (slow)

```ts
test('user can delete a note', async ({ page, request }) => {
  await request.post('http://localhost:8080/api/notes', { data: { title: 'Temp note' } });

  await page.goto('/notes');
  await page.getByRole('button', { name: 'Delete' }).first().click();

  await expect(page.getByText('Temp note')).toHaveCount(0);
});
```

## Step 8: React component tests (fast, run on every save)

```bash
npm i -D vitest jsdom @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

```jsx
// src/components/ShortenForm.test.jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { vi, test, expect } from 'vitest';
import ShortenForm from './ShortenForm';

test('submits the typed URL', async () => {
  const onSubmit = vi.fn();
  render(<ShortenForm onSubmit={onSubmit} />);

  await userEvent.type(screen.getByLabelText(/long url/i), 'https://example.com');
  await userEvent.click(screen.getByRole('button', { name: /shorten/i }));

  expect(onSubmit).toHaveBeenCalledWith('https://example.com');
});
```

Vitest needs `environment: 'jsdom'` in `vitest.config`.

## Step 9: Debug like a pro

| Command | What it does |
|---|---|
| `npx playwright test --ui` | Visual runner with time travel |
| `npx playwright test --headed` | Shows the browser |
| `npx playwright test --debug` | Step-by-step debugger |
| `npx playwright codegen http://localhost:5173` | Records your clicks as test code |
| `npx playwright show-report` | Opens the HTML report |
| `npx playwright show-trace trace.zip` | Replays a failed CI run |

## Step 10: Run in GitHub Actions

```yaml
name: E2E

on:
  pull_request:

jobs:
  e2e:
    runs-on: ubuntu-latest
    timeout-minutes: 20
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npx playwright test
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: playwright-report
          path: playwright-report/
```

If your backend is Java or Python, add steps to start it (or run it with Docker Compose) before `npx playwright test`.

## Test case checklist

**E2E (Playwright)**
- [ ] Happy path: the main feature works from start to finish
- [ ] Login works, and logged-out users are redirected
- [ ] Validation errors are shown to the user
- [ ] Deleting or updating an item is reflected in the UI
- [ ] Page reload keeps the user logged in

**Component (RTL)**
- [ ] Form submits correct values
- [ ] Empty state, loading state and error state render
- [ ] Buttons are disabled while submitting

## Done when
- [ ] 3 to 5 E2E tests pass 10 times in a row (no flaky tests)
- [ ] Tests use roles and labels, not CSS classes
- [ ] No `waitForTimeout` in the code
- [ ] CI runs E2E on pull requests and uploads the report

## Learn more
- GFG: [Introduction to GitHub Actions](https://www.geeksforgeeks.org/git/introduction-to-github-actions) (to run E2E in CI)
- Official: [Playwright getting started](https://playwright.dev/docs/intro)
- Official: [Testing Library for React](https://testing-library.com/docs/react-testing-library/intro/)

---

# 42. TDD Katas (Java, JavaScript, Python)

## What is a kata?
A kata is a small coding exercise you repeat to build a habit, like practicing scales in music. The goal is **not** to finish fast. The goal is to practice **Red, Green, Refactor** until it feels natural.

## The routine for every kata (20 to 30 minutes)

1. Read the problem, then write a **test list** (plain text) before any code
2. Pick the simplest test, write it, and **watch it fail** (Red)
3. Write the **minimum** code to pass (Green)
4. Clean up (Refactor), and rerun all tests
5. **Commit** after every green
6. Repeat until the list is done
7. Write 2 lines: what went well, what felt hard

## Minimal setup per language

**Java (Maven, JUnit 5)** add to `pom.xml` (use the latest versions):

```xml
<dependencies>
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.11.0</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.assertj</groupId>
    <artifactId>assertj-core</artifactId>
    <version>3.26.0</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

Run: `mvn test`

**JavaScript (Vitest)**

```bash
npm init -y
npm i -D vitest
```

Add `"type": "module"` and `"test": "vitest"` (watch mode) to `package.json`. Name files `fizzbuzz.js` and `fizzbuzz.test.js`.

**Python (pytest)**

```bash
pip install pytest pytest-watch
```

Name files `fizzbuzz.py` and `test_fizzbuzz.py`. Run `pytest -q` (or `ptw` to rerun on save).

## Folder layout for your repo

```
katas/
├── 01-fizzbuzz/
│   ├── java/
│   ├── javascript/
│   └── python/
├── 02-string-calculator/
├── 03-roman-numerals/
...
```

## The katas (do them in this order)

| # | Kata | Level | Time | You practice |
|---|---|---|---|---|
| 1 | FizzBuzz | Beginner | 15 min | The basic cycle, small steps |
| 2 | String Calculator | Beginner | 30 min | Growing requirements, exceptions |
| 3 | Roman Numerals | Beginner | 30 min | Letting the algorithm emerge from tests |
| 4 | Prime Factors | Beginner | 20 min | Tiny steps, refactoring |
| 5 | Bank Account | Intermediate | 45 min | State, validation, test doubles |
| 6 | Bowling Game | Intermediate | 60 min | Domain rules, edge cases |
| 7 | Tennis Score | Intermediate | 45 min | State transitions |
| 8 | Mars Rover | Intermediate | 60 min | Commands, boundaries, design |
| 9 | Gilded Rose | Advanced | 90 min | Refactoring legacy code safely |

## Starter test lists (write these in a comment, then go one by one)

### Kata 1: FizzBuzz
Return "Fizz" for multiples of 3, "Buzz" for multiples of 5, "FizzBuzz" for both, else the number as text.
- [ ] 1 returns "1"
- [ ] 2 returns "2"
- [ ] 3 returns "Fizz"
- [ ] 5 returns "Buzz"
- [ ] 6 returns "Fizz"
- [ ] 10 returns "Buzz"
- [ ] 15 returns "FizzBuzz"
- [ ] Generating 1 to 100 gives a list of 100 items

### Kata 2: String Calculator
`add("1,2")` returns 3.
- [ ] Empty string returns 0
- [ ] One number returns itself
- [ ] Two numbers return the sum
- [ ] Any amount of numbers
- [ ] New lines also separate numbers (`"1\n2,3"` returns 6)
- [ ] Custom delimiter (`"//;\n1;2"` returns 3)
- [ ] Negative numbers throw an error listing all negatives
- [ ] Numbers greater than 1000 are ignored

### Kata 3: Roman Numerals
Convert a number to a Roman numeral.
- [ ] 1 is I, 2 is II, 3 is III
- [ ] 4 is IV, 5 is V, 9 is IX
- [ ] 10 is X, 40 is XL, 50 is L, 90 is XC
- [ ] 100 is C, 400 is CD, 500 is D, 900 is CM, 1000 is M
- [ ] 1994 is MCMXCIV
- [ ] 3999 is MMMCMXCIX
- [ ] 0 and 4000 are rejected

### Kata 4: Prime Factors
Return the prime factors of a number.
- [ ] 1 returns `[]`
- [ ] 2 returns `[2]`
- [ ] 3 returns `[3]`
- [ ] 4 returns `[2, 2]`
- [ ] 6 returns `[2, 3]`
- [ ] 8 returns `[2, 2, 2]`
- [ ] 9 returns `[3, 3]`
- [ ] 100 returns `[2, 2, 5, 5]`

### Kata 5: Bank Account
- [ ] New account has balance 0
- [ ] Deposit increases the balance
- [ ] Withdraw decreases the balance
- [ ] Withdraw more than the balance is rejected
- [ ] Zero or negative amounts are rejected
- [ ] Statement lists transactions with date, amount and running balance
- [ ] Use a fake clock so dates are predictable (practice test doubles)

### Kata 6: Bowling Game
Score a game of ten frames.
- [ ] All gutter balls scores 0
- [ ] All ones scores 20
- [ ] One spare followed by 3 scores 16 for the first two frames' total (spare bonus counts the next roll)
- [ ] One strike followed by 3 and 4 scores 24
- [ ] Perfect game (12 strikes) scores 300
- [ ] All 5s (spare every frame, final bonus 5) scores 150

### Kata 7: Tennis Score
- [ ] 0-0 is "Love-All"
- [ ] 1-0 is "Fifteen-Love"
- [ ] 1-1 is "Fifteen-All"
- [ ] 3-3 is "Deuce"
- [ ] 4-0 is "Win for player 1"
- [ ] 4-3 is "Advantage player 1"
- [ ] Advantage then the other player scores returns to "Deuce"

### Kata 8: Mars Rover
A rover has a position (x, y) and a direction (N, E, S, W). Commands: `L`, `R` turn, `M` moves forward.
- [ ] Starts at 0,0 facing N
- [ ] `R` turns to E, `L` turns to W
- [ ] `M` facing N moves y up by 1
- [ ] `RRRR` returns to N
- [ ] `MMRMM` ends at 2,2 facing E
- [ ] Wrap around the grid edge (or stop at an obstacle, your choice)

### Kata 9: Gilded Rose (refactoring)
You get messy legacy code and must add a feature safely.
1. Write **characterization tests** that lock in what the code does today (run it with many inputs and record the output)
2. Refactor in tiny steps with the tests as your safety net
3. Add the new feature test-first

## Progress tracker (copy into your repo README)

| Kata | Java | JavaScript | Python |
|---|:---:|:---:|:---:|
| FizzBuzz | [ ] | [ ] | [ ] |
| String Calculator | [ ] | [ ] | [ ] |
| Roman Numerals | [ ] | [ ] | [ ] |
| Prime Factors | [ ] | [ ] | [ ] |
| Bank Account | [ ] | [ ] | [ ] |
| Bowling Game | [ ] | [ ] | [ ] |
| Tennis Score | [ ] | [ ] | [ ] |
| Mars Rover | [ ] | [ ] | [ ] |
| Gilded Rose | [ ] | [ ] | [ ] |

## Suggested schedule

| Week | Katas | Focus |
|---|---|---|
| 1 | FizzBuzz, String Calculator | Learn the rhythm in your strongest language |
| 2 | Roman Numerals, Prime Factors | Tiny steps and triangulation |
| 3 | Bank Account, Bowling Game | Test doubles and domain rules |
| 4 | Tennis, Mars Rover | Design emerging from tests |
| 5 | Gilded Rose | Refactoring with tests |
| 6 | Redo 3 katas in a second language | Build speed and confidence |

## Self-check after each kata

- [ ] Did I see every test fail first, and for the right reason?
- [ ] Were my steps small (a few minutes each)?
- [ ] Did I refactor on every green?
- [ ] Do my commits show the red-green-refactor rhythm?
- [ ] Which test taught me something about the design?

## Reflection template (write 3 lines per kata)

```
Kata:
Where I took a step that was too big:
What I would do differently next time:
```

## Common mistakes

| Mistake | Fix |
|---|---|
| Writing the full solution first | Hard-code the first answer, then triangulate |
| Skipping Refactor | Always spend 1 minute cleaning up |
| Not running the test before coding | Always watch it fail |
| Jumping to a hard test too early | Pick the next simplest case |
| Chasing a perfect solution | The habit is the goal, not the code |

## Done when
- [ ] Katas 1 to 8 are done in at least one language
- [ ] At least 3 katas are done in all three languages
- [ ] Every kata folder has a commit history showing small steps
- [ ] You can explain Red-Green-Refactor and triangulation without notes

## Learn more
- GFG: [Test Driven Development (TDD)](https://www.geeksforgeeks.org/?p=381024)
- GFG: [The Importance of Test-Driven Development (TDD)](https://www.geeksforgeeks.org/?p=1105942)
- Practice online: [Cyber-Dojo](https://cyber-dojo.org) (kata in your browser, many languages)
- Kata descriptions: [Coding Dojo](https://codingdojo.org/kata/)
- Guided practice: [Exercism](https://exercism.org)