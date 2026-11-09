# Java Fundamentals, Memory Model & JVM Architecture

## 1. Introduction & Prerequisites
Modern Java (Java 21 LTS) combines rock-solid backward compatibility with cutting-edge language features: record patterns, sealed classes, pattern matching for switch, and modernized garbage collectors. Understanding the Java Virtual Machine (JVM) memory architecture is required to diagnose production bottlenecks, out-of-memory errors, and latency spikes.

- **Prerequisites:** Basic programming logic.
- **Estimated Study Time:** 14 hours
- **Difficulty:** Beginner to Intermediate

---

## 2. Detailed Learning Objectives
- Differentiate between JDK, JRE, JVM, and JIT Compiler (C1 / C2).
- Understand JVM Memory Regions: Heap (Young/Old Generation), Stack, Metaspace, and Program Counter.
- Learn Garbage Collection algorithms: Serial, Parallel, G1GC, and ZGC (low-latency concurrent GC).
- Master Java 21 modern syntax: Records, Pattern Matching, Sealed Classes/Interfaces, and Text Blocks.
- Understand value vs reference semantics and object equality (`equals()` vs `==` and `hashCode()`).

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] JDK vs JRE vs JVM internals and ClassLoader hierarchy (Bootstrap, Platform, Application)
- [ ] Primitive Data Types, Wrapper Classes, and Autoboxing traps
- [ ] Memory Management: Stack frames, Heap allocation, and Metaspace
- [ ] Garbage Collection fundamentals: Young Gen (Eden, S0, S1) and Tenured Old Gen
- [ ] Modern Garbage Collectors: G1GC vs ZGC concurrent compaction
- [ ] String Pool immutability and `StringBuilder` optimization
- [ ] Java Records: Canonical constructors, compact constructors, immutability
- [ ] Sealed Classes and Interfaces (`permits`, `sealed`, `non-sealed`, `final`)
- [ ] Pattern Matching for `instanceof` and `switch` expressions
- [ ] Exception Hierarchy (`Throwable`, Checked `Exception`, Unchecked `RuntimeException`, `Error`)

---

## 4. Free Documentation Resources
- **Oracle Java SE 21 Documentation:** [https://docs.oracle.com/en/java/javase/21/](https://docs.oracle.com/en/java/javase/21/)
- **Dev.java (Official Java Developer Portal):** [https://dev.java/](https://dev.java/)
- **Inside Java (Oracle JVM Engineering Team):** [https://inside.java/](https://inside.java/)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Java 8 to 21 Features Deep Dive
- **freeCodeCamp:** Java Full Course for Beginners
- **Amigoscode:** Java 21 Masterclass & JVM Architecture

---

## 6. Important Concepts Explained

### JVM Memory Layout & Stack vs Heap
- **Stack Memory:** Each thread has its own private stack. It stores primitive local variables and references to objects. Memory allocation is LIFO and instantaneous. When a method returns, its frame is popped.
- **Heap Memory:** Shared across all threads. Stores all actual objects and arrays. Managed automatically by the Garbage Collector.
- **Metaspace:** Native off-heap memory storing class metadata, bytecode, and method signatures.

```
┌─────────────────────────────────────────────────────────────┐
│                         JVM HEAP                            │
│  ┌───────────────────────┐      ┌────────────────────────┐  │
│  │  Young Generation     │      │   Old / Tenured Gen    │  │
│  │  [Eden | S0 | S1]     │      │   (Long-lived objects) │  │
│  └───────────────────────┘      └────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
          ▲                                ▲
          │ (References)                   │ (References)
┌─────────┴─────────┐            ┌─────────┴─────────┐
│  Thread 1 Stack   │            │  Thread 2 Stack   │
│  [Frame: methodA] │            │  [Frame: methodB] │
└───────────────────┘            └───────────────────┘
```

---

## 7. Practical Code Examples

### Modern Java 21: Sealed Hierarchies & Pattern Matching
```java
package com.roadmap.fundamentals;

// Sealed interface defining exhaustive payment methods
public sealed interface PaymentMethod 
    permits CreditCard, UpiPayment, CryptoPayment {}

public record CreditCard(String cardNumber, String expiryDate, int cvv) implements PaymentMethod {}
public record UpiPayment(String vpaAddress) implements PaymentMethod {}
public record CryptoPayment(String walletAddress, String network) implements PaymentMethod {}

public class PaymentProcessor {

    public static String processPayment(PaymentMethod payment, double amount) {
        // Exhaustive pattern matching: no 'default' branch needed!
        return switch (payment) {
            case CreditCard(var number, var expiry, _) -> 
                String.format("Charging $%.2f to Card ending in %s", amount, number.substring(number.length() - 4));
            case UpiPayment(var vpa) -> 
                String.format("Initiating UPI collect request of $%.2f to %s", amount, vpa);
            case CryptoPayment(var wallet, var network) -> 
                String.format("Transferring $%.2f via %s to wallet %s", amount, network, wallet);
        };
    }

    public static void main(String[] args) {
        PaymentMethod upi = new UpiPayment("developer@okaxis");
        System.out.println(processPayment(upi, 249.99));
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Write a program that demonstrates `equals()` and `hashCode()` contract violation using a custom `User` class inside a `HashSet`.
2. **Assignment 2:** Model an order state machine using Java 21 sealed interfaces and evaluate state transitions with pattern-matching `switch` statements.

---

## 9. Mini-Projects
- **JVM Memory Leak Simulator & Profiler:** Create a controlled simulation that generates an `OutOfMemoryError: Java heap space` and analyze the heap dump using VisualVM or Eclipse Memory Analyzer (MAT).

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between String, StringBuilder, and StringBuffer?
**Answer:** `String` is immutable; every modification creates a new object in memory (or the string pool). `StringBuilder` is mutable and not thread-safe, making it the most performant choice for single-threaded string concatenation. `StringBuffer` is mutable and thread-safe because its methods are synchronized, incurring thread contention overhead.

### Q2: Why must `hashCode()` be overridden whenever `equals()` is overridden?
**Answer:** The Java specification states that if two objects are equal according to `equals(Object)`, they must return the identical integer from `hashCode()`. Hash-based collections (like `HashMap`, `HashSet`) use `hashCode()` to determine the bucket index. If `hashCode()` is not overridden, two logically equal objects will hash to different buckets, causing `map.get(key)` to fail and duplicates to enter sets.

---

## 11. Revision Notes
- Prefer `Record` for immutable data-transfer carriers (DTOs).
- ZGC achieves sub-millisecond maximum GC pause times regardless of heap size.
- Avoid autoboxing inside tight loops; use primitive arrays where throughput is paramount.
- Always use `try-with-resources` for auto-closing `AutoCloseable` resources.

---

## 12. Topic Completion Checklist
- [ ] Understand JVM Stack vs Heap allocation
- [ ] Overrode `equals()` and `hashCode()` correctly
- [ ] Used Java 21 Records and Sealed Classes
- [ ] Inspected a running JVM process using `jcmd` or VisualVM
