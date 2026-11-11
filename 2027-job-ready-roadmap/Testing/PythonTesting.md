25. pytest Basics
26. Fixtures & conftest
27. Parametrized Tests
28. unittest & unittest.mock
29. pytest-mock
30. FastAPI / Flask API Testing (TestClient, httpx)
31. Django Testing
32. Testcontainers for Python
33. Hypothesis (Property-Based Testing)
34. Async Testing (pytest-asyncio)
35. Coverage (pytest-cov) & Mutation Testing (mutmut)
36. LLM & RAG Evaluation (ties to your AI Engineering module)




# Node.js & JavaScript Testing – Practical Notes

## 16. Node.js Built-in Test Runner

### What is Node.js Built-in Test Runner?

Node.js provides a built-in testing framework through the `node:test` module.

It allows you to write and execute tests without installing Jest or another testing framework.

**Use it for:**
- Testing Node.js functions.
- Testing utility modules.
- Testing business logic.
- Testing asynchronous functions.
- Testing backend services.

### 16.1 Basic Unit Testing

**calculator.js**

```javascript
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}
```

**calculator.test.js**

```javascript
import test from 'node:test';
import assert from 'node:assert';

import { add, subtract } from './calculator.js';

test('should add two numbers', () => {
    assert.strictEqual(add(10, 20), 30);
});

test('should subtract two numbers', () => {
    assert.strictEqual(subtract(20, 10), 10);
});
```

**Explanation:**
- `node:test` provides the `test()` function.
- `node:assert` provides assertion methods.
- `assert.strictEqual()` checks whether actual and expected values are strictly equal.

### 16.2 Testing Asynchronous Functions

```javascript
import test from 'node:test';
import assert from 'node:assert';

async function getUser() {
    return { id: 1, name: 'Adarsh' };
}

test('should return user details', async () => {
    const user = await getUser();

    assert.strictEqual(user.name, 'Adarsh');
});
```

### 16.3 Testing Exceptions

```javascript
test('should reject invalid input', () => {
    function withdraw(amount) {
        if (amount <= 0) {
            throw new Error('Invalid amount');
        }
    }

    assert.throws(
        () => withdraw(-100),
        {
            message: 'Invalid amount'
        }
    );
});
```

### 16.4 Mocking Functions

```javascript
import test from 'node:test';
import assert from 'node:assert';

test('should mock a function', (t) => {
    const mock = t.mock.fn(() => 100);

    const result = mock();

    assert.strictEqual(result, 100);
    assert.strictEqual(mock.mock.callCount(), 1);
});
```

### 16.5 Running Tests

```bash
node --test
```

Run a specific file:

```bash
node --test calculator.test.js
```

Watch for changes:

```bash
node --test --watch
```

### When to Use

- Small Node.js projects.
- Utility function testing.
- Backend business logic.
- Projects where you don't need an external testing framework.

**Resources:**
- [Node.js Test Runner Documentation](https://nodejs.org/api/test.html)
- [GeeksforGeeks – Node.js Testing Resources](https://www.geeksforgeeks.org/?s=Node.js+Testing)

---

## 17. Jest

### What is Jest?

Jest is a JavaScript testing framework used to test Node.js applications and frontend applications.

It provides:
- Test execution.
- Assertions.
- Mock functions.
- Snapshot testing.
- Code coverage.
- Asynchronous testing.

**Use it for:** Testing Express APIs, utility functions, backend services and React applications.

### 17.1 Installation

```bash
npm install --save-dev jest
```

For ES Modules, configure Jest according to its ESM documentation and your Node.js version.

Example `package.json` configuration:

```json
{
  "type": "module",
  "scripts": {
    "test": "node --experimental-vm-modules node_modules/jest/bin/jest.js"
  }
}
```

### 17.2 Basic Unit Testing

**calculator.js**

```javascript
export function add(a, b) {
    return a + b;
}
```

**calculator.test.js**

```javascript
import { test, expect } from '@jest/globals';
import { add } from './calculator.js';

test('should add two numbers', () => {
    expect(add(10, 20)).toBe(30);
});
```

### 17.3 Important Jest Matchers

| Matcher | Purpose |
|---|---|
| `toBe()` | Strict equality |
| `toEqual()` | Deep object equality |
| `toBeTruthy()` | Checks truthy value |
| `toBeFalsy()` | Checks falsy value |
| `toContain()` | Checks array or string contents |
| `toHaveLength()` | Checks length |
| `toThrow()` | Checks exceptions |
| `toHaveBeenCalled()` | Checks whether a mock was called |
| `toHaveBeenCalledTimes()` | Checks invocation count |

### 17.4 Testing Objects and Arrays

```javascript
test('should return user details', () => {
    const user = {
        id: 1,
        name: 'Adarsh'
    };

    expect(user).toEqual({
        id: 1,
        name: 'Adarsh'
    });
});
```

```javascript
test('should contain Java skill', () => {
    const skills = ['Java', 'Node.js', 'React'];

    expect(skills).toContain('Node.js');
});
```

### 17.5 Mock Functions

```javascript
test('should send email', () => {
    const sendEmail = jest.fn();

    sendEmail('test@gmail.com');

    expect(sendEmail).toHaveBeenCalled();

    expect(sendEmail).toHaveBeenCalledWith(
        'test@gmail.com'
    );
});
```

### 17.6 Testing Async Functions

```javascript
test('should return user', async () => {
    const getUser = async () => ({
        id: 1,
        name: 'Adarsh'
    });

    const user = await getUser();

    expect(user.name).toBe('Adarsh');
});
```

### 17.7 Running Tests

```bash
npm test
```

Run a particular test:

```bash
npx jest calculator.test.js
```

Run with coverage:

```bash
npm test -- --coverage
```

### When to Use

- Testing backend services.
- Testing JavaScript business logic.
- Testing API-related functions.
- Testing React applications.
- Mocking external dependencies.

**Resources:**
- [Jest Official Documentation](https://jestjs.io/docs/getting-started)
- [GeeksforGeeks – Jest Testing](https://www.geeksforgeeks.org/?s=Jest+Testing)

---

## 18. Vitest

### What is Vitest?

Vitest is a modern JavaScript testing framework built for speed and designed to work particularly well with Vite projects.

It provides:
- Unit testing.
- Mocking.
- Assertions.
- Code coverage.
- Snapshot testing.
- Watch mode.

**Use it for:**
- React applications.
- Next.js projects (with appropriate configuration).
- JavaScript and TypeScript projects.
- Frontend utility functions.
- Backend business logic.

### 18.1 Installation

```bash
npm install -D vitest
```

Add a script in `package.json`:

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run"
  }
}
```

### 18.2 Basic Unit Testing

**calculator.js**

```javascript
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}
```

**calculator.test.js**

```javascript
import { describe, it, expect } from 'vitest';
import { add, multiply } from './calculator.js';

describe('Calculator', () => {

    it('should add numbers', () => {
        expect(add(10, 20)).toBe(30);
    });

    it('should multiply numbers', () => {
        expect(multiply(5, 4)).toBe(20);
    });

});
```

### 18.3 Mocking Functions

```javascript
import { test, expect, vi } from 'vitest';

test('should call email function', () => {
    const sendEmail = vi.fn();

    sendEmail('test@gmail.com');

    expect(sendEmail).toHaveBeenCalled();
    expect(sendEmail).toHaveBeenCalledWith(
        'test@gmail.com'
    );
});
```

### 18.4 Testing Async Functions

```javascript
test('should fetch user', async () => {
    const getUser = async () => ({
        id: 1,
        name: 'Adarsh'
    });

    const user = await getUser();

    expect(user.name).toBe('Adarsh');
});
```

### 18.5 Running Tests

```bash
npm test
```

Run once:

```bash
npm run test:run
```

Run with coverage:

```bash
npx vitest run --coverage
```

### Jest vs Vitest

| Jest | Vitest |
|---|---|
| Established testing framework | Modern testing framework |
| Popular in React projects | Popular in Vite projects |
| Requires ESM configuration in some setups | Native ESM support |
| Uses `jest.fn()` | Uses `vi.fn()` |
| Uses Jest configuration | Uses Vitest configuration |

### When to Use

- React applications.
- Vite projects.
- JavaScript and TypeScript applications.
- Fast unit testing.

**Resources:**
- [Vitest Documentation](https://vitest.dev/guide/)
- [GeeksforGeeks – Vitest Resources](https://www.geeksforgeeks.org/?s=Vitest)

---

## 19. Mocking (Sinon, MSW)

### What is Mocking?

Mocking means replacing a real function, object or external service with a controlled substitute during testing.

**Real-world example:**

Your application sends an email whenever a user registers.

You don't want to send real emails during testing.

Instead, mock the email service and verify whether it was called.

There are two useful tools:

1. Sinon – Function and object mocking.
2. MSW – HTTP request mocking.

### 19.1 Sinon

Sinon is a JavaScript library used for spies, stubs and mocks.

**Installation:**

```bash
npm install --save-dev sinon
```

### A. Spy

A Spy observes function calls without replacing the original functionality.

```javascript
import sinon from 'sinon';
import assert from 'node:assert';

function sendEmail(email) {
    console.log('Email sent:', email);
}

const spy = sinon.spy(sendEmail);

spy('test@gmail.com');

assert.equal(spy.calledOnce, true);
assert.equal(
    spy.firstCall.args[0],
    'test@gmail.com'
);
```

**Use case:** Verify whether a function was called and which arguments were passed.

### B. Stub

A Stub replaces a function and returns predefined data.

```javascript
import sinon from 'sinon';
import assert from 'node:assert';

const paymentService = {
    processPayment: () => 'FAILED'
};

const stub = sinon.stub(
    paymentService,
    'processPayment'
);

stub.returns('SUCCESS');

const result = paymentService.processPayment();

assert.equal(result, 'SUCCESS');

stub.restore();
```

**Use case:** Simulate successful or failed payment responses without using a real payment gateway.

### C. Mock

A Mock sets expectations about how an object should be used.

```javascript
import sinon from 'sinon';

const emailService = {
    send: () => {}
};

const mock = sinon.mock(emailService);

mock.expects('send')
    .once()
    .withArgs('test@gmail.com');

emailService.send('test@gmail.com');

mock.verify();
```

**Use case:** Verify expected method calls and arguments.

### 19.2 Sinon Quick Revision

| Method | Purpose |
|---|---|
| `sinon.spy()` | Observes function calls |
| `sinon.stub()` | Replaces function behavior |
| `sinon.mock()` | Defines interaction expectations |
| `returns()` | Returns predefined value |
| `resolves()` | Returns resolved Promise |
| `rejects()` | Returns rejected Promise |
| `restore()` | Restores original behavior |

### 19.3 MSW (Mock Service Worker)

MSW intercepts network requests and returns predefined responses.

Unlike Sinon, which primarily mocks functions and objects, MSW mocks HTTP requests at the network level.

**Example:**

Your frontend calls:

```text
GET http://localhost:8080/api/users
```

Instead of calling the actual backend, MSW returns fake user data.

**Installation:**

```bash
npm install --save-dev msw
```

### A. Create API Handler

**mocks/handlers.js**

```javascript
import { http, HttpResponse } from 'msw';

export const handlers = [

    http.get(
        'http://localhost:8080/api/users',
        () => {
            return HttpResponse.json([
                {
                    id: 1,
                    name: 'Adarsh'
                },
                {
                    id: 2,
                    name: 'Rahul'
                }
            ]);
        }
    )

];
```

### B. Configure Mock Server for Node.js Tests

**mocks/server.js**

```javascript
import { setupServer } from 'msw/node';
import { handlers } from './handlers.js';

export const server = setupServer(...handlers);
```

### C. Use in Tests

```javascript
import {
    beforeAll,
    afterAll,
    afterEach,
    test,
    expect
} from 'vitest';

import { server } from './mocks/server.js';

beforeAll(() => server.listen());

afterEach(() => server.resetHandlers());

afterAll(() => server.close());

test('should fetch users', async () => {

    const response = await fetch(
        'http://localhost:8080/api/users'
    );

    const users = await response.json();

    expect(users).toHaveLength(2);
    expect(users[0].name).toBe('Adarsh');

});
```

### Sinon vs MSW

| Sinon | MSW |
|---|---|
| Mocks functions and objects | Mocks network requests |
| Useful for unit tests | Useful for API integration and frontend tests |
| Stubs function responses | Returns fake HTTP responses |
| Works at function level | Works at network level |

### When to Use

- Sinon: Mock database methods and business logic dependencies.
- MSW: Mock backend APIs for frontend and API client testing.

**Resources:**
- [Sinon Documentation](https://sinonjs.org/)
- [MSW Documentation](https://mswjs.io/docs/)
- [GeeksforGeeks – Sinon Resources](https://www.geeksforgeeks.org/?s=Sinon+JavaScript)
- [GeeksforGeeks – MSW Resources](https://www.geeksforgeeks.org/?s=Mock+Service+Worker)

---

## 20. Supertest

### What is Supertest?

Supertest is a Node.js library used to test HTTP APIs without manually starting a server and sending requests through Postman.

It is commonly used with Express.js.

**Use it for:**
- Testing REST APIs.
- Testing GET, POST, PUT and DELETE endpoints.
- Testing HTTP status codes.
- Testing request validation.
- Testing API responses.

### 20.1 Installation

```bash
npm install --save-dev supertest
```

### 20.2 Create Express Application

**app.js**

```javascript
import express from 'express';

const app = express();

app.use(express.json());

app.get('/users/:id', (req, res) => {
    res.status(200).json({
        id: req.params.id,
        name: 'Adarsh'
    });
});

app.post('/users', (req, res) => {
    const { name } = req.body;

    if (!name) {
        return res.status(400).json({
            message: 'Name is required'
        });
    }

    res.status(201).json({
        message: 'User created',
        name
    });
});

export default app;
```

### 20.3 Write API Tests

**app.test.js**

```javascript
import request from 'supertest';
import { describe, it, expect } from 'vitest';

import app from './app.js';

describe('User API', () => {

    it('should get user', async () => {

        const response = await request(app)
            .get('/users/1');

        expect(response.status).toBe(200);

        expect(response.body.name)
            .toBe('Adarsh');

    });

    it('should create user', async () => {

        const response = await request(app)
            .post('/users')
            .send({
                name: 'Adarsh'
            });

        expect(response.status).toBe(201);

        expect(response.body.name)
            .toBe('Adarsh');

    });

    it('should reject invalid user', async () => {

        const response = await request(app)
            .post('/users')
            .send({});

        expect(response.status).toBe(400);

    });

});
```

### 20.4 Important Supertest Methods

| Method | Purpose |
|---|---|
| `request(app)` | Creates request agent |
| `.get()` | Sends GET request |
| `.post()` | Sends POST request |
| `.put()` | Sends PUT request |
| `.patch()` | Sends PATCH request |
| `.delete()` | Sends DELETE request |
| `.send()` | Sends request body |
| `.set()` | Sets HTTP headers |
| `.expect()` | Checks response |

### 20.5 Testing Authentication

```javascript
const response = await request(app)
    .get('/profile')
    .set('Authorization', `Bearer ${token}`)
    .expect(200);
```

Use a valid test token issued by your authentication setup.

### When to Use

- Express.js REST APIs.
- Authentication APIs.
- CRUD operations.
- Request validation.
- Integration testing.

**Resources:**
- [Supertest Documentation](https://github.com/ladjs/supertest)
- [GeeksforGeeks – Supertest Resources](https://www.geeksforgeeks.org/?s=Supertest)

---

## 21. Testcontainers for Node.js

### What is Testcontainers?

Testcontainers allows Node.js tests to start real external services inside Docker containers.

Examples:

- PostgreSQL.
- MongoDB.
- MySQL.
- Redis.
- Kafka.

**Real-world example:**

Your Node.js application uses MongoDB.

Instead of mocking database queries, you can start a real MongoDB container and test actual database operations.

### 21.1 Installation

For PostgreSQL:

```bash
npm install --save-dev @testcontainers/postgresql
```

Also install the PostgreSQL driver if your application needs it:

```bash
npm install pg
```

Prerequisites:
- Docker installed.
- Docker daemon running.
- Node.js project.

### 21.2 Start PostgreSQL Container

**database.test.js**

```javascript
import {
    PostgreSqlContainer
} from '@testcontainers/postgresql';

import {
    beforeAll,
    afterAll,
    test,
    expect
} from 'vitest';

import pg from 'pg';

const { Client } = pg;

let container;
let client;

beforeAll(async () => {

    container = await new PostgreSqlContainer(
        'postgres:16-alpine'
    ).start();

    client = new Client({
        connectionString: container.getConnectionUri()
    });

    await client.connect();

    await client.query(`
        CREATE TABLE users (
            id SERIAL PRIMARY KEY,
            name VARCHAR(100)
        )
    `);

});

afterAll(async () => {

    await client?.end();

    await container?.stop();

});

test('should insert user into database', async () => {

    const result = await client.query(
        'INSERT INTO users(name) VALUES($1) RETURNING *',
        ['Adarsh']
    );

    expect(result.rows[0].name).toBe('Adarsh');

});
```

### 21.3 How It Works

1. Test starts.
2. Testcontainers starts PostgreSQL in Docker.
3. Node.js connects to the temporary database.
4. Test executes actual SQL queries.
5. Assertions verify database results.
6. Container is stopped after testing.

### When to Use

- Testing real database queries.
- Testing database migrations.
- Testing MongoDB repositories.
- Testing Redis operations.
- Testing Kafka integrations.

### Testcontainers vs Mock Database

| Testcontainers | Mock Database |
|---|---|
| Uses a real database | Uses simulated behavior |
| Tests actual queries | Tests expected responses |
| More realistic | Faster |
| Requires Docker | Usually does not require Docker |

**Resources:**
- [Testcontainers for Node.js](https://node.testcontainers.org/)
- [GeeksforGeeks – Testcontainers Resources](https://www.geeksforgeeks.org/?s=Testcontainers)

---

## 22. React Testing Library

### What is React Testing Library?

React Testing Library (RTL) is used to test React components from the user's perspective.

Instead of testing internal implementation details, it tests what users see and interact with.

**Example:**

You have a login button.

You want to verify:
- Whether the button is displayed.
- Whether the user can click it.
- Whether the expected message appears.

### 22.1 Installation

```bash
npm install --save-dev \
@testing-library/react \
@testing-library/dom \
@testing-library/jest-dom \
@testing-library/user-event \
vitest jsdom
```

### 22.2 Create React Component

**Counter.jsx**

```jsx
import { useState } from 'react';

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <div>
            <h1>Count: {count}</h1>

            <button
                onClick={() => setCount(count + 1)}
            >
                Increment
            </button>
        </div>
    );
}

export default Counter;
```

### 22.3 Write Component Test

**Counter.test.jsx**

```jsx
import {
    render,
    screen
} from '@testing-library/react';

import userEvent from '@testing-library/user-event';

import { describe, it, expect } from 'vitest';

import Counter from './Counter';

describe('Counter Component', () => {

    it('should display initial count', () => {

        render(<Counter />);

        expect(
            screen.getByText('Count: 0')
        ).toBeInTheDocument();

    });

    it('should increment count on click', async () => {

        const user = userEvent.setup();

        render(<Counter />);

        await user.click(
            screen.getByRole('button', {
                name: 'Increment'
            })
        );

        expect(
            screen.getByText('Count: 1')
        ).toBeInTheDocument();

    });

});
```

### 22.4 Important RTL Methods

| Method | Purpose |
|---|---|
| `render()` | Renders React component |
| `screen.getByText()` | Finds element by text |
| `screen.getByRole()` | Finds element by accessibility role |
| `screen.getByLabelText()` | Finds form element by label |
| `screen.queryByText()` | Returns null if not found |
| `screen.findByText()` | Waits for element to appear |
| `fireEvent` | Triggers DOM events |
| `userEvent` | Simulates user interactions |

### 22.5 Example: Login Form Testing

```jsx
const user = userEvent.setup();

render(<LoginForm />);

await user.type(
    screen.getByLabelText('Email'),
    'adarsh@example.com'
);

await user.type(
    screen.getByLabelText('Password'),
    'password123'
);

await user.click(
    screen.getByRole('button', {
        name: 'Login'
    })
);
```

You can then verify whether the expected success message or navigation state appears.

### Important Best Practice

Prefer queries in this order:

1. `getByRole()`
2. `getByLabelText()`
3. `getByPlaceholderText()`
4. `getByText()`
5. `getByTestId()` as a last resort.

### When to Use

- React component testing.
- Login forms.
- Buttons and user interactions.
- Form validation.
- Conditional rendering.
- Testing API-driven components.

**Resources:**
- [React Testing Library Documentation](https://testing-library.com/docs/react-testing-library/intro/)
- [GeeksforGeeks – React Testing Library](https://www.geeksforgeeks.org/?s=React+Testing+Library)

---

## 23. Playwright

### What is Playwright?

Playwright is a browser automation and end-to-end testing framework.

It tests applications by interacting with a real browser.

It supports:
- Chromium.
- Firefox.
- WebKit.

**Real-world example:**

You develop an e-commerce website.

You want to test the complete shopping workflow:

1. Open website.
2. Login.
3. Search for a product.
4. Add product to cart.
5. Complete checkout.
6. Verify order confirmation.

Playwright can automate this entire process.

### 23.1 Installation

```bash
npm init playwright@latest
```

Install browser dependencies when prompted.

### 23.2 Project Structure

```text
project/
├── tests/
│   ├── login.spec.js
│   └── checkout.spec.js
├── playwright.config.js
└── package.json
```

### 23.3 Basic Browser Test

**login.spec.js**

```javascript
import {
    test,
    expect
} from '@playwright/test';

test('should open website', async ({ page }) => {

    await page.goto('http://localhost:3000');

    await expect(page).toHaveTitle(/My Website/);

});
```

### 23.4 Testing Login

```javascript
import {
    test,
    expect
} from '@playwright/test';

test('should login successfully', async ({ page }) => {

    await page.goto('http://localhost:3000/login');

    await page.getByLabel('Email')
        .fill('adarsh@example.com');

    await page.getByLabel('Password')
        .fill('password123');

    await page.getByRole('button', {
        name: 'Login'
    }).click();

    await expect(page)
        .toHaveURL(/dashboard/);

});
```

This assumes your login form has accessible labels and redirects successful users to a dashboard.

### 23.5 Testing Form Validation

```javascript
test('should show validation error', async ({ page }) => {

    await page.goto('http://localhost:3000/login');

    await page.getByRole('button', {
        name: 'Login'
    }).click();

    await expect(
        page.getByText('Email is required')
    ).toBeVisible();

});
```

### 23.6 Important Playwright Methods

| Method | Purpose |
|---|---|
| `page.goto()` | Opens a URL |
| `page.click()` | Clicks an element |
| `page.fill()` | Enters text |
| `page.locator()` | Selects an element |
| `page.getByRole()` | Finds element by role |
| `page.getByText()` | Finds element by text |
| `expect()` | Checks expected behavior |
| `toBeVisible()` | Checks element visibility |
| `toHaveURL()` | Checks current URL |
| `screenshot()` | Captures screenshot |

### 23.7 Running Tests

```bash
npx playwright test
```

Run in headed mode:

```bash
npx playwright test --headed
```

Run a specific test:

```bash
npx playwright test tests/login.spec.js
```

Open HTML report:

```bash
npx playwright show-report
```

### 23.8 Playwright vs React Testing Library

| Playwright | React Testing Library |
|---|---|
| End-to-end testing | Component testing |
| Uses real browsers | Usually uses simulated DOM |
| Tests complete workflows | Tests individual components |
| Can test frontend and backend together | Focuses on React UI behavior |
| Supports multiple browsers | Does not provide browser automation |

### When to Use

- Login and registration workflows.
- E-commerce checkout.
- Cross-browser testing.
- End-to-end application testing.
- Testing complete user journeys.

**Resources:**
- [Playwright Documentation](https://playwright.dev/docs/intro)
- [GeeksforGeeks – Playwright Resources](https://www.geeksforgeeks.org/?s=Playwright+Testing)

---

## 24. Coverage & Mutation Testing (c8, Stryker)

### What is Code Coverage?

Code coverage measures how much of your application code is executed by automated tests.

Example:

Suppose your application has 100 executable lines.

Your tests execute 80 lines.

Line coverage = 80%.

**Important:** High coverage does not necessarily mean your tests are effective.

### 24.1 c8

c8 is a JavaScript code coverage tool that uses V8's built-in coverage capabilities.

It works with Node.js applications and can be used alongside Node's built-in test runner.

### Installation

```bash
npm install --save-dev c8
```

### Run Coverage

```bash
npx c8 node --test
```

### Example

**calculator.js**

```javascript
export function checkAge(age) {

    if (age >= 18) {
        return 'Adult';
    }

    return 'Minor';
}
```

**calculator.test.js**

```javascript
import test from 'node:test';
import assert from 'node:assert';

import { checkAge } from './calculator.js';

test('should return adult', () => {
    assert.strictEqual(checkAge(25), 'Adult');
});
```

Run:

```bash
npx c8 node --test
```

The report shows that the `Minor` branch was not executed.

Add another test:

```javascript
test('should return minor', () => {
    assert.strictEqual(checkAge(15), 'Minor');
});
```

Now both branches are covered.

### 24.2 Coverage Metrics

| Metric | Meaning |
|---|---|
| Statements | Executed statements |
| Branches | Executed decision branches |
| Functions | Executed functions |
| Lines | Executed source lines |

### 24.3 Generate HTML Report

```bash
npx c8 --reporter=html node --test
```

Open:

```text
coverage/index.html
```

### 24.4 Stryker Mutation Testing

Stryker is a mutation testing framework for JavaScript and TypeScript.

It modifies your source code intentionally to check whether your tests detect incorrect behavior.

**Example:**

Original code:

```javascript
export function isAdult(age) {
    return age >= 18;
}
```

Stryker may generate a mutation:

```javascript
export function isAdult(age) {
    return age > 18;
}
```

If your tests detect this change, the mutation is killed.

If your tests still pass, the mutation survives.

### 24.5 Install Stryker

For a JavaScript project using Node's test runner:

```bash
npm install --save-dev \
@stryker-mutator/core \
@stryker-mutator/node-runner \
@stryker-mutator/javascript-mutator
```

Initialize configuration:

```bash
npx stryker init
```

Select the Node test runner and configure the test files and source files.

### 24.6 Example Tests

```javascript
import test from 'node:test';
import assert from 'node:assert';

import { isAdult } from './user.js';

test('should accept age 18', () => {
    assert.strictEqual(isAdult(18), true);
});

test('should reject age 17', () => {
    assert.strictEqual(isAdult(17), false);
});
```

Run mutation testing:

```bash
npx stryker run
```

### 24.7 Understanding Mutation Score

```text
Mutation Score =
Killed Mutations / Valid Mutations × 100
```

Example:

- Total valid mutations: 100
- Killed mutations: 85
- Survived mutations: 15

Mutation score = 85%.

Some mutations may be classified as timed out, no coverage or other statuses.

### c8 vs Stryker

| c8 | Stryker |
|---|---|
| Measures code coverage | Measures test effectiveness |
| Shows executed code | Changes code intentionally |
| Identifies untested areas | Identifies weak tests |
| Relatively fast | Usually slower |
| Uses V8 coverage | Uses mutation testing |

### When to Use

**c8:**
- Measuring test coverage.
- Identifying untested functions.
- Checking branch coverage.
- CI/CD quality checks.

**Stryker:**
- Testing important business logic.
- Finding weak assertions.
- Checking boundary conditions.
- Improving test suite quality.

**Resources:**
- [c8 Documentation](https://github.com/bcoe/c8)
- [Stryker Documentation](https://stryker-mutator.io/)
- [GeeksforGeeks – Code Coverage Resources](https://www.geeksforgeeks.org/?s=JavaScript+Code+Coverage)
- [GeeksforGeeks – Mutation Testing](https://www.geeksforgeeks.org/?s=Mutation+Testing)

---

## 25. Practical Learning Roadmap

| Order | Tool | What to Build |
|---|---|---|
| 1 | Node Built-in Test Runner | Calculator and utility tests |
| 2 | Jest | Unit tests for backend services |
| 3 | Vitest | Test JavaScript and React utilities |
| 4 | Sinon | Mock functions and dependencies |
| 5 | MSW | Mock external HTTP APIs |
| 6 | Supertest | Test Express CRUD APIs |
| 7 | Testcontainers | PostgreSQL integration tests |
| 8 | React Testing Library | Test React components |
| 9 | Playwright | Automate login and checkout |
| 10 | c8 | Generate coverage reports |
| 11 | Stryker | Perform mutation testing |

---

## 26. Final Practice Project – MarketX Testing Suite

Build tests for a marketplace application using Node.js, Express.js, Next.js and MongoDB.

### Application Features

- User registration and login.
- JWT authentication.
- Product listing.
- Product search.
- Add to cart.
- Order placement.
- Payment processing.
- Notifications.

### Testing Checklist

- [ ] Node Test Runner: Test utility functions.
- [ ] Jest: Test backend business logic.
- [ ] Vitest: Test frontend utility functions.
- [ ] Sinon: Mock payment and email functions.
- [ ] MSW: Mock backend APIs in frontend tests.
- [ ] Supertest: Test Express REST APIs.
- [ ] Testcontainers: Test MongoDB integration.
- [ ] React Testing Library: Test login and product components.
- [ ] Playwright: Test complete shopping workflow.
- [ ] c8: Generate backend code coverage reports.
- [ ] Stryker: Check test effectiveness for business logic.

---

## 27. Quick Revision

| Tool | Remember |
|---|---|
| Node Built-in Test Runner | Built-in Node.js testing |
| Jest | JavaScript testing framework |
| Vitest | Fast testing for modern JS/TS projects |
| Sinon | Spies, stubs and mocks |
| MSW | Mock HTTP requests |
| Supertest | Test Express APIs |
| Testcontainers | Run real services in Docker |
| React Testing Library | Test React components |
| Playwright | Browser automation and E2E testing |
| c8 | Measure JavaScript code coverage |
| Stryker | Mutation testing |

### Recommended Priority

1. Master Node.js Built-in Test Runner, Jest and Vitest.
2. Learn Sinon and MSW.
3. Practise Supertest for Express APIs.
4. Learn Testcontainers for database integration.
5. Learn React Testing Library.
6. Master Playwright for end-to-end testing.
7. Finally, learn c8 and Stryker.

**Goal:** Become comfortable testing real-world Node.js backends, React frontends and complete full-stack applications.
