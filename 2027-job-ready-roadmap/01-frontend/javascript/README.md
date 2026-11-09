# Modern JavaScript (ES6+), Event Loop & Asynchronous Architecture

## 1. Introduction & Prerequisites
JavaScript is the runtime engine of modern web architectures. Understanding execution context, call stack behavior, microtask vs macrotask scheduling, closures, prototype chains, and asynchronous programming with Promises and `async`/`await` is non-negotiable for enterprise engineering.

- **Prerequisites:** HTML5 DOM structure, basic CSS concepts.
- **Estimated Study Time:** 24 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Deeply understand the JavaScript Execution Context, Scope Chain, and Closures.
- Master the JavaScript Event Loop, Call Stack, Web APIs, Microtask Queue (Promises), and Macrotask Queue (setTimeout).
- Write clean asynchronous code with native Promises, `Promise.allSettled`, and `async`/`await`.
- Manipulate the DOM efficiently using event delegation and modern DOM querying APIs.
- Master ES6+ features: Destructuring, Spread/Rest, Modules, Optional Chaining, Nullish Coalescing.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Variables & Scopes (`var` vs `let` vs `const`, Temporal Dead Zone, Block Scope)
- [ ] Execution Context, Hoisting, and Lexical Scope
- [ ] Closures & Practical Encapsulation Patterns
- [ ] Prototype Chain, Inheritance, and ES6 Classes
- [ ] The Event Loop: Call Stack, Web APIs, Microtasks (`Promise`), Macrotasks (`setTimeout`)
- [ ] Asynchronous JavaScript: Callbacks, Promises, `async`/`await`, Error Handling (`try/catch/finally`)
- [ ] Array Methods: `map`, `filter`, `reduce`, `flatMap`, `some`, `every`, `find`
- [ ] Modern DOM Manipulation & Event Delegation (`addEventListener`, `event.target`)
- [ ] Native ES Modules (`import`, `export`, `<script type="module">`)
- [ ] Web Storage APIs (`localStorage`, `sessionStorage`, `IndexedDB`)

---

## 4. Free Documentation Resources
- **MDN Web Docs — JavaScript Guide:** [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- **JavaScript.info (The Modern JavaScript Tutorial):** [https://javascript.info/](https://javascript.info/)
- **You Don't Know JS (Book Series):** [https://github.com/getify/You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS)

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** JavaScript Programming Full Course
- **Lydia Hallie:** JavaScript Visualized: Event Loop, Scope, and Promises
- **Traversy Media:** JavaScript Modern ES6+ Tutorial

---

## 6. Important Concepts Explained

### The Event Loop Architecture
JavaScript is single-threaded. Asynchronous tasks (like network fetches or timers) are delegated to browser Web APIs.
- **Microtask Queue:** Promise callbacks (`.then()`, `catch()`, `await` resumptions). Microtasks are processed immediately when the call stack empties, before any rendering or macrotasks.
- **Macrotask Queue:** `setTimeout`, `setInterval`, I/O, UI events.

---

## 7. Practical Code Examples

### Modern Async Fetch with AbortController, Timeout, and Error Handling
```javascript
export async function fetchWithTimeout(url, timeoutMs = 5000) {
  const controller = new AbortController();
  const timer = setTimeout(() => controller.abort(), timeoutMs);

  try {
    const response = await fetch(url, {
      signal: controller.signal,
      headers: { 'Accept': 'application/json' }
    });

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
    }

    return await response.json();
  } catch (error) {
    if (error.name === 'AbortError') {
      throw new Error(`Request timed out after ${timeoutMs}ms: ${url}`);
    }
    throw error;
  } finally {
    clearTimeout(timer);
  }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Implement your own custom `Promise.all` polyfill that handles an array of promises, resolves when all succeed, or rejects immediately on the first error.
2. **Assignment 2:** Implement a high-performance debounced search input with an auto-suggest dropdown using vanilla JavaScript event delegation.

---

## 9. Mini-Projects
- **Offline-First Task Tracker:** Build a Kanban task application with vanilla ES Modules that persists all tasks to `localStorage` with JSON import/export.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is a Closure in JavaScript and what is a common practical use case?
**Answer:** A closure is the combination of a function bundled together with references to its surrounding lexical environment. It gives an inner function access to an outer function's scope even after the outer function has returned. Common use cases include data privacy/encapsulation, memoization caches, and event handler configuration.

### Q2: What is the output order of `setTimeout` vs `Promise.resolve`?
**Answer:** Microtasks (Promise resolutions) always execute before Macrotasks (`setTimeout`). Even with `setTimeout(..., 0)`, the Promise callback runs first as soon as synchronous execution finishes.

---

## 11. Revision Notes
- Use `const` by default; use `let` only when reassigning; never use `var`.
- Use optional chaining (`user?.profile?.avatar`) and nullish coalescing (`value ?? defaultValue`).
- Always wrap `await` calls in `try...catch` blocks.
- Use `structuredClone(obj)` for deep copying objects instead of `JSON.parse(JSON.stringify(obj))`.

---

## 12. Topic Completion Checklist
- [ ] Understand Call Stack, Microtasks, and Macrotasks
- [ ] Implemented an asynchronous function with timeout cancellation
- [ ] Mastered closures and lexical scoping
- [ ] Built an interactive UI using native ES Modules
