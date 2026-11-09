# Java Stream API & Parallel Streams

## 1. Introduction & Prerequisites
The Stream API (introduced in Java 8 and continuously refined) enables functional, declarative querying and transformation of in-memory data sequences. Streams operate lazily, optimize execution pipelines, and support parallel processing across multi-core processors.

- **Prerequisites:** Java Collections, Functional Programming, Lambdas.
- **Estimated Study Time:** 12 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Differentiate between intermediate operations (lazy) and terminal operations (eager).
- Master core intermediate operations: `filter`, `map`, `flatMap`, `distinct`, `sorted`, `peek`, `limit`, `skip`.
- Master terminal operations: `collect`, `reduce`, `forEach`, `count`, `anyMatch`, `findFirst`.
- Master standard and custom `Collectors`: `groupingBy`, `partitioningBy`, `mapping`, `joining`.
- Understand Parallel Streams: ForkJoinPool under the hood, overhead costs, and state mutation hazards.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Stream Pipeline Anatomy: Source -> Intermediate Operations -> Terminal Operation
- [ ] Laziness of Intermediate Operations & Short-Circuiting (`findFirst`, `anyMatch`)
- [ ] `map()` vs `flatMap()` (One-to-One vs One-to-Many flattening)
- [ ] Aggregation using `reduce()` with Identity, Accumulator, and Combiner
- [ ] Modern Collectors: `Collectors.groupingBy()`, `Collectors.counting()`, `Collectors.toMap()`
- [ ] Downstream Collectors for Multi-Level Grouping
- [ ] Parallel Streams (`parallelStream()`) & Common ForkJoinPool
- [ ] Performance Gotchas: Boxing/Unboxing in Streams (`IntStream`, `LongStream`)
- [ ] Thread Safety: Stateful operations and non-interfering functions
- [ ] Java 9-21 Stream Enhancements: `takeWhile()`, `dropWhile()`, `toList()`, `gatherers`

---

## 4. Free Documentation Resources
- **Oracle Java Stream Package Documentation:** [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)
- **Baeldung — The Java 8 Stream API Guide:** [https://www.baeldung.com/java-8-streams](https://www.baeldung.com/java-8-streams)
- **Dev.java — The Stream API:** [https://dev.java/learn/api/streams/](https://dev.java/learn/api/streams/)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Java Streams In-Depth Playlist
- **Amigoscode:** Java Streams Full Tutorial
- **Defog Tech:** How Java Streams Work Under the Hood

---

## 6. Important Concepts Explained

### map vs flatMap Visualized
- **`map`:** Transforms each element of type $T$ into an element of type $R$. An input of 5 elements produces an output of 5 elements.
- **`flatMap`:** Transforms each element of type $T$ into a *Stream* of elements of type $R$, then flattens all nested streams into a single output stream.

```
map: [ Order1(3 items), Order2(2 items) ] ───> [ 3, 2 ] (Item counts)
flatMap: [ Order1([A, B, C]), Order2([D, E]) ] ───> [ A, B, C, D, E ] (Flat items)
```

---

## 7. Practical Code Examples

### Complex Multi-Level Aggregation with `Collectors.groupingBy`
```java
package com.roadmap.streams;

import java.util.*;
import java.util.stream.Collectors;

public class TransactionAnalytics {

    public record Transaction(String id, String category, double amount, String city) {}

    public static void main(String[] args) {
        List<Transaction> transactions = List.of(
            new Transaction("TX1", "Electronics", 1200.0, "New York"),
            new Transaction("TX2", "Grocery", 85.5, "New York"),
            new Transaction("TX3", "Electronics", 450.0, "San Francisco"),
            new Transaction("TX4", "Grocery", 120.0, "San Francisco"),
            new Transaction("TX5", "Apparel", 210.0, "New York")
        );

        // Group total revenue by city, and then by category
        Map<String, Map<String, Double>> revenueByCityAndCategory = transactions.stream()
            .collect(Collectors.groupingBy(
                Transaction::city,
                Collectors.groupingBy(
                    Transaction::category,
                    Collectors.summingDouble(Transaction::amount)
                )
            ));

        System.out.println("Revenue breakdown: " + revenueByCityAndCategory);
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Given a list of sentences, find the top 5 most frequently occurring words (case-insensitive) using `flatMap`, `groupingBy`, and sorting.
2. **Assignment 2:** Implement an aggregation pipeline that calculates the average, minimum, maximum, and total salary across engineering departments using `Collectors.summarizingDouble()`.

---

## 9. Mini-Projects
- **Log File Analytics Engine:** Parse an Apache server access log file, extract IP addresses, status codes, and request durations using Stream operations, and output traffic statistics grouped by HTTP response code (2xx, 4xx, 5xx).

---

## 10. Interview Questions & Detailed Answers

### Q1: What makes Stream intermediate operations lazy?
**Answer:** Intermediate operations (such as `filter`, `map`) do not process any data when invoked; they simply return a new `Stream` instance with an appended pipeline stage. Computation only occurs when a terminal operation (such as `collect`, `count`, `forEach`) is triggered. This allows the JVM to fuse operations into a single pass and optimize with short-circuiting.

### Q2: When should you avoid using `parallelStream()`?
**Answer:** Avoid parallel streams when:
1. The dataset is small (thread coordination overhead exceeds execution gain).
2. Operations perform blocking I/O (this monopolizes the shared `ForkJoinPool.commonPool()`).
3. Splitting cost is high (e.g. `LinkedList` has poor splitting characteristics $O(n)$ compared to `ArrayList` $O(1)$).
4. Order matters or state is mutated concurrently without locks.

---

## 11. Revision Notes
- Use `stream.toList()` (Java 16+) instead of `collect(Collectors.toList())` for unmodifiable lists.
- Use `IntStream.range(0, n)` instead of creating boxed `Integer` collections.
- Never modify the underlying collection during stream execution.

---

## 12. Topic Completion Checklist
- [ ] Mastered intermediate vs terminal operations
- [ ] Used `flatMap` for nested data extraction
- [ ] Built multi-level grouping with Collectors
- [ ] Benchmarked sequential vs parallel streams
