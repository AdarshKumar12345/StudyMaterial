# Functional Programming in Java

## 1. Introduction & Prerequisites
Modern Java embraces functional programming paradigms. By treating functions as first-class citizens, Java code becomes more concise, declarative, and less error-prone. This module covers lambda expressions, method references, functional interfaces, and pure function concepts.

- **Prerequisites:** Java OOP, Interfaces.
- **Estimated Study Time:** 10 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Understand Lambda calculus fundamentals and syntax in Java.
- Master `@FunctionalInterface` contract and built-in standard functional interfaces:
  - `Predicate<T>` (boolean evaluation)
  - `Function<T, R>` (transformation)
  - `Consumer<T>` (side-effect action)
  - `Supplier<T>` (factory generation)
  - `UnaryOperator<T>` and `BinaryOperator<T>`
- Understand Method References (`ClassName::methodName`) across all 4 categories.
- Learn function composition using `andThen()` and `compose()`.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Anonymous Inner Classes vs Lambda Expressions (bytecode invokedynamic)
- [ ] Functional Interface Definition (`@FunctionalInterface`)
- [ ] Standard Built-in Interfaces in `java.util.function`
- [ ] Primitive Specializations (`IntPredicate`, `LongFunction`, `DoubleConsumer`) to prevent boxing
- [ ] Method References: Static, Instance of arbitrary object, Instance of specific object, Constructor
- [ ] Variable Capture: Effectively Final requirement in Lambdas
- [ ] Higher-Order Functions: Functions that accept or return functions
- [ ] Currying and Partial Application in Java
- [ ] Monads in Java: Deep dive into `Optional<T>` and avoiding `NullPointerException`

---

## 4. Free Documentation Resources
- **Java Functional Interfaces API Reference:** [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/function/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/function/package-summary.html)
- **Baeldung — Functional Interfaces in Java 8:** [https://www.baeldung.com/java-8-functional-interfaces](https://www.baeldung.com/java-8-functional-interfaces)
- **Dev.java — Lambda Expressions:** [https://dev.java/learn/lambdas/](https://dev.java/learn/lambdas/)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Java 8 Functional Programming Full Course
- **Venkat Subramaniam:** Functional Programming with Java (Devoxx)
- **Amigoscode:** Java Functional Programming with Lambdas

---

## 6. Important Concepts Explained

### Standard Functional Interfaces Cheat Sheet
```
┌─────────────────────┬───────────────────┬──────────────────────┬─────────────────────────┐
│ Interface           │ Method Signature  │ Input -> Output      │ Typical Use Case        │
├─────────────────────┼───────────────────┼──────────────────────┼─────────────────────────┤
│ `Predicate<T>`      │ `boolean test(T)` │ `T -> boolean`       │ Filtering conditions    │
│ `Function<T, R>`    │ `R apply(T)`      │ `T -> R`             │ Value transformation    │
│ `Consumer<T>`       │ `void accept(T)`  │ `T -> void`          │ Printing, logging, save │
│ `Supplier<T>`       │ `T get()`         │ `() -> T`            │ Factories, lazy values  │
│ `BiFunction<T,U,R>` │ `R apply(T, U)`   │ `(T, U) -> R`        │ Combining two inputs    │
└─────────────────────┴───────────────────┴──────────────────────┴─────────────────────────┘
```

---

## 7. Practical Code Examples

### Composing Functions for Business Validation Pipelines
```java
package com.roadmap.functional;

import java.util.function.Predicate;
import java.util.function.Function;

public class UserValidationPipeline {

    public record User(String username, String email, int age) {}

    // Reusable, composable predicates
    public static final Predicate<User> isAdult = u -> u.age() >= 18;
    public static final Predicate<User> hasValidEmail = u -> u.email() != null && u.email().contains("@");
    public static final Predicate<User> isEligible = isAdult.and(hasValidEmail);

    // Transforming user to audit log entry
    public static final Function<User, String> toAuditLog = 
        u -> String.format("AUDIT: User %s (%s) registered successfully", u.username(), u.email());

    public static void main(String[] args) {
        User validUser = new User("adarsh", "adarsh@example.com", 24);
        User minorUser = new User("alex", "alex@school.org", 15);

        if (isEligible.test(validUser)) {
            System.out.println(toAuditLog.apply(validUser));
        }

        System.out.println("Is minor eligible? " + isEligible.test(minorUser));
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a dynamic query filter pipeline using `Predicate.and()` and `Predicate.or()` that filters products by category, min price, and max price.
2. **Assignment 2:** Implement an `Optional<T>` pipeline that extracts a deeply nested property (`Order -> Customer -> Address -> ZipCode`) without any `if (obj != null)` checks.

---

## 9. Mini-Projects
- **Configurable Notification Dispatcher:** Create a dispatcher system where routing strategies, transformations, and formatting rules are configured purely as injected higher-order functional interfaces.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why must local variables referenced inside a lambda expression be final or effectively final?
**Answer:** A lambda expression may execute asynchronously on a different thread long after the method enclosing the local variable has finished and its stack frame has been destroyed. To prevent invalid memory reads, Java makes a copy (captures) of the local variable. If the variable were mutable, the copy and the original could diverge, leading to concurrency and state inconsistency bugs.

### Q2: What is the difference between `Function.compose()` and `Function.andThen()`?
**Answer:** `f.andThen(g)` executes `f` first and passes its result to `g`: equivalent to $g(f(x))$. `f.compose(g)` executes `g` first and passes its result to `f`: equivalent to $f(g(x))$.

---

## 11. Revision Notes
- Use `Optional.map()`, `flatMap()`, and `orElseGet()` instead of `Optional.get()`.
- Use primitive specializations (`IntPredicate`) to avoid boxing overhead in high-throughput loops.
- Keep lambdas short (1-3 lines); extract longer logic to private methods and use method references.

---

## 12. Topic Completion Checklist
- [ ] Mastered Predicate, Function, Consumer, Supplier
- [ ] Used Method References in all 4 forms
- [ ] Avoided NullPointerExceptions using `Optional`
- [ ] Built composable validation pipelines
