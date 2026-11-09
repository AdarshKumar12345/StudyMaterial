# Object-Oriented Programming (OOP) & SOLID Design Principles

## 1. Introduction & Prerequisites
Object-Oriented Programming is more than just classes and objects; it is an engineering discipline for managing complexity in large-scale enterprise codebases. Mastering the 4 OOP pillars and the 5 SOLID design principles allows you to write decoupled, testable, and maintainable systems.

- **Prerequisites:** Java Fundamentals.
- **Estimated Study Time:** 16 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Master the 4 Pillars: Encapsulation, Abstraction, Inheritance, and Polymorphism.
- Distinguish between Composition vs Inheritance (prefer composition).
- Understand and apply all 5 SOLID Principles:
  - **S:** Single Responsibility Principle (SRP)
  - **O:** Open/Closed Principle (OCP)
  - **L:** Liskov Substitution Principle (LSP)
  - **I:** Interface Segregation Principle (ISP)
  - **D:** Dependency Inversion Principle (DIP)
- Implement essential GoF Design Patterns: Factory Method, Strategy, Observer, Decorator, and Builder.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Encapsulation & Data Hiding (Access Modifiers: `private`, `protected`, `public`, package-private)
- [ ] Polymorphism: Compile-Time (Overloading) vs Runtime (Overriding with `@Override`)
- [ ] Abstract Classes vs Interfaces (Default methods, static methods, private interface methods)
- [ ] Composition vs Inheritance ("Is-a" vs "Has-a" relationship pitfalls)
- [ ] Single Responsibility Principle (SRP) & High Cohesion
- [ ] Open/Closed Principle (OCP) via Strategy Pattern
- [ ] Liskov Substitution Principle (LSP) & Contract Violations
- [ ] Interface Segregation Principle (ISP) & Role Interfaces
- [ ] Dependency Inversion Principle (DIP) & Inversion of Control
- [ ] Core Creational & Behavioral Patterns: Builder, Singleton (Enum-based), Factory, Strategy

---

## 4. Free Documentation Resources
- **Refactoring Guru — Design Patterns:** [https://refactoring.guru/design-patterns](https://refactoring.guru/design-patterns)
- **Baeldung — SOLID Principles in Java:** [https://www.baeldung.com/solid-principles](https://www.baeldung.com/solid-principles)
- **Oracle Java Tutorials — Interfaces & Inheritance:** [https://docs.oracle.com/javase/tutorial/java/IandI/index.html](https://docs.oracle.com/javase/tutorial/java/IandI/index.html)

---

## 5. Free Video Courses & Tutorials
- **Christopher Okhravi:** SOLID Principles Detailed Series (YouTube)
- **freeCodeCamp:** Design Patterns in Java Tutorial
- **Amigoscode:** Software Architecture & Clean Code

---

## 6. Important Concepts Explained

### SOLID Principles Architectural Overview
```
┌─────────────────────────────────────────────────────────────┐
│                    THE SOLID PRINCIPLES                     │
├───────┬─────────────────────────────────────────────────────┤
│ **S** │ Single Responsibility: One class, one reason to change.
│ **O** │ Open/Closed: Open for extension, closed for modification.
│ **L** │ Liskov Substitution: Subtypes must be substitutable for base types.
│ **I** │ Interface Segregation: Clients should not depend on unused interfaces.
│ **D** │ Dependency Inversion: Depend on abstractions, not concretions.
└───────┴─────────────────────────────────────────────────────┘
```

---

## 7. Practical Code Examples

### Applying the Strategy Pattern (Open/Closed Principle)
```java
package com.roadmap.oops;

import java.math.BigDecimal;

// Strategy Interface
public interface DiscountStrategy {
    BigDecimal applyDiscount(BigDecimal totalAmount);
}

// Concrete Strategies
public class FestivalDiscount implements DiscountStrategy {
    @Override
    public BigDecimal applyDiscount(BigDecimal totalAmount) {
        return totalAmount.multiply(BigDecimal.valueOf(0.85)); // 15% off
    }
}

public class VipCustomerDiscount implements DiscountStrategy {
    @Override
    public BigDecimal applyDiscount(BigDecimal totalAmount) {
        return totalAmount.multiply(BigDecimal.valueOf(0.70)); // 30% off
    }
}

// Context Class adhering to Open/Closed Principle
public class CheckoutService {
    public BigDecimal calculateTotal(BigDecimal cartTotal, DiscountStrategy discountStrategy) {
        // Can add new discount algorithms without modifying CheckoutService!
        return discountStrategy.applyDiscount(cartTotal);
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Refactor a monolithic `NotificationService` that uses `if-else` for Email/SMS/WhatsApp into clean classes satisfying SRP, OCP, and DIP.
2. **Assignment 2:** Implement an immutable `UserAccount` object using the GoF Builder pattern with step-by-step validation.

---

## 9. Mini-Projects
- **Extensible Parking Lot Simulation (LLD):** Build an object-oriented parking lot management system supporting multiple vehicle types (Car, Bike, Truck), fee calculation strategies, and spot allocation.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the Liskov Substitution Principle and can you give a classic violation example?
**Answer:** LSP states that objects of a superclass should be replaceable with objects of its subclasses without breaking application correctness. The classic violation is the `Square extends Rectangle` problem. In mathematics, a square is a rectangle, but in code, if `Square` overrides `setWidth()` to also mutate `height`, any client expecting standard rectangle behavior (`width * height` after setting width) fails unexpectedly.

### Q2: Why should you prefer Composition over Inheritance?
**Answer:** Inheritance creates tight coupling between the parent and child ("fragile base class problem"). Changes in the superclass can inadvertently break subclass invariants. Composition models a "has-a" relationship, allows behavior to be swapped dynamically at runtime (via dependency injection), and maintains strict encapsulation.

---

## 11. Revision Notes
- Make classes `final` unless explicitly designing for inheritance.
- Prefer interfaces over abstract classes when defining capabilities.
- Keep constructors lightweight; delegate complex creation to Builders or Factories.
- Program to an interface, not an implementation.

---

## 12. Topic Completion Checklist
- [ ] Implemented Factory, Strategy, and Builder patterns
- [ ] Refactored code to eliminate SRP violations
- [ ] Designed a system using Composition over Inheritance
- [ ] Validated LSP compliance in class hierarchies
