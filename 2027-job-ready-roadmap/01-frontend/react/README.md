# React 19 & Modern Component-Driven Architecture

## 1. Introduction & Prerequisites
React is the industry-dominant library for building component-driven user interfaces. Modern React emphasizes declarative functional components, native hooks (`useState`, `useEffect`, `useCallback`, `useMemo`, `useRef`), centralized Context, and concurrent rendering features.

- **Prerequisites:** Modern JavaScript (ES6+), DOM APIs, Promises, and Destructuring.
- **Estimated Study Time:** 26 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Master the declarative component mental model, JSX syntax, and props contract.
- Understand state immutability, reconciliation, and Virtual DOM diffing algorithms.
- Master React Hooks: `useState`, `useEffect` dependency array rules, `useRef`, `useCallback`, and `useMemo`.
- Avoid unnecessary re-renders and memory leaks.
- Build custom hooks for reusable business logic (e.g., `useLocalStorage`, `useDebounce`, `useFetch`).
- Manage global application state using React Context with custom Provider patterns.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] JSX Internals, Babel/SWC Transpilation & `React.createElement`
- [ ] Functional Components, Props, and Default Values
- [ ] State Immutability & `useState` Batching
- [ ] Component Lifecycle & `useEffect` Cleanup Functions
- [ ] References with `useRef` (DOM access & mutable instance values)
- [ ] Memoization with `useMemo` and `useCallback` (When and when NOT to optimize)
- [ ] React Context API for Global State Management
- [ ] Custom Hooks Design Patterns
- [ ] Error Boundaries (`componentDidCatch` & fallback UI)
- [ ] Modern React 19 primitives: `use()` hook, Server Components, Action hooks

---

## 4. Free Documentation Resources
- **React Official Documentation (react.dev):** [https://react.dev/](https://react.dev/)
- **React TypeScript Cheatsheet:** [https://react-typescript-cheatsheet.netlify.app/](https://react-typescript-cheatsheet.netlify.app/)
- **Overreacted (Blog by Dan Abramov):** [https://overreacted.io/](https://overreacted.io/) *(A Complete Guide to useEffect)*

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** *React Course for Beginners (2024/2025 Edition)*
- **Jack Herrington:** *Mastering React Hooks & State Management*
- **Web Dev Simplified:** *React Hooks Explained Simply*

---

## 6. Important Concepts Explained

### Component Lifecycle & Hook Dependency Mental Model
In modern React, `useEffect` is not merely a lifecycle method (`componentDidMount`/`Update`); it is a synchronization mechanism between the React state and an external system (such as browser APIs, timers, or network sockets).

```
   Component Render (State / Props changed)
              │
              ▼
       DOM Updated / Painted
              │
              ▼
   Clean up previous effect (if re-running)
              │
              ▼
       Run Effect Callback
```

---

## 7. Practical Code Examples

### Production-Ready Custom Hook: `useLocalStorage`
```javascript
import { useState, useEffect } from 'react';

/**
 * Custom React hook for synchronizing state with browser localStorage.
 * @param {string} key 
 * @param {any} initialValue 
 */
export function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(storedValue));
    } catch (error) {
      console.error(`Error saving localStorage key "${key}":`, error);
    }
  }, [key, storedValue]);

  return [storedValue, setStoredValue];
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a multi-step onboarding wizard with validation, back/next navigation, and summary review using React state.
2. **Assignment 2:** Implement a searchable, filterable data table with sorting, pagination, and a debounced search input using `useMemo` and custom hooks.

---

## 9. Mini-Projects
- **Interactive Roadmap Study Tracker App:** A React application displaying learning modules, completed checkboxes, progress bars, and JSON export/import using React Context and `useLocalStorage`.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why must state in React never be mutated directly?
**Answer:** React relies on referential equality (`Object.is`) during its reconciliation phase to determine whether an object or array has changed. If you mutate an existing array or object directly (e.g., `todos.push(newTodo)`), the memory reference remains identical, causing React to skip re-rendering the UI. Always return a new copy (e.g., `setTodos(prev => [...prev, newTodo])`).

### Q2: What is the difference between `useMemo` and `useCallback`?
**Answer:** `useMemo` caches the **result** of a calculation (`const cachedValue = useMemo(() => computeExpensively(a, b), [a, b])`). `useCallback` caches the **function instance itself** between renders (`const cachedFn = useCallback(() => handleClick(id), [id])`), which prevents unnecessary re-renders of child components wrapped in `React.memo`.

---

## 11. Revision Notes
- Never call hooks inside loops, conditions, or nested functions.
- Always declare all reactive variables in the `useEffect` dependency array.
- Clean up side effects (event listeners, intervals, AbortControllers) in the `return () => {}` cleanup callback.
- Keep state local to where it is used; lift state up only when multiple siblings need it.

---

## 12. Topic Completion Checklist
- [ ] Understand JSX compilation and Virtual DOM diffing
- [ ] Built components using `useState` and `useEffect`
- [ ] Created and used a custom hook
- [ ] Managed shared state using React Context
