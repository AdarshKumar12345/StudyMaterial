# Java Collections Framework Internals

## 1. Introduction & Prerequisites
The Java Collections Framework (JCF) provides reusable, high-performance data structures. In enterprise backend development and high-concurrency systems, choosing the correct collection and understanding its internal memory layout, rehashing mechanism, and locking granularity directly impacts throughput and memory usage.

- **Prerequisites:** Java OOP, Generics basics.
- **Estimated Study Time:** 18 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Understand the complete Collection interface hierarchy (`List`, `Set`, `Queue`, `Deque`, `Map`).
- Master `HashMap` internals: hashing algorithm, bucket array, linked list to Red-Black tree conversion (`TREEIFY_THRESHOLD = 8`), and load factor rehashing.
- Contrast `HashMap`, `LinkedHashMap`, `TreeMap`, and `ConcurrentHashMap`.
- Analyze time and space complexity ($O(1)$, $O(\log n)$, $O(n)$) across all collections.
- Master thread-safe collections: `CopyOnWriteArrayList`, `ConcurrentLinkedQueue`, `BlockingQueue`.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Collections vs Arrays trade-offs
- [ ] `ArrayList` dynamic resizing formula ($oldCapacity + (oldCapacity >> 1)$)
- [ ] `LinkedList` doubly-linked node overhead vs sequential access
- [ ] `HashMap` Internals: `hash()`, bucket index calculation `(n - 1) & hash`
- [ ] Hash Collisions: Separate chaining, Red-Black tree treeification
- [ ] `ConcurrentHashMap`: CAS (Compare-And-Swap), synchronized bucket heads, no global lock
- [ ] `TreeMap` & `TreeSet`: Self-balancing Red-Black trees ($O(\log n)$ operations)
- [ ] `PriorityQueue`: Binary Min-Heap implementation and Comparator ordering
- [ ] `Collections.unmodifiableList` vs `List.of()` (Java 9+ immutable collections)
- [ ] Generics: Invariance, Covariance (`? extends T`), Contravariance (`? super T`) (PECS rule)

---

## 4. Free Documentation Resources
- **Oracle Java Collections Guide:** [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/doc-files/coll-index.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/doc-files/coll-index.html)
- **Baeldung — Guide to Java HashMap:** [https://www.baeldung.com/java-hashmap](https://www.baeldung.com/java-hashmap)
- **Dev.java — The Collections Framework:** [https://dev.java/learn/api/collections-framework/](https://dev.java/learn/api/collections-framework/)

---

## 5. Free Video Courses & Tutorials
- **Java Brains:** Java Collections Framework in Depth
- **Defog Tech:** HashMap Internal Working in Java
- **Amigoscode:** Java Data Structures & Collections

---

## 6. Important Concepts Explained

### HashMap Treeification & Rehashing Internals
In Java 8+, `HashMap` organizes entries into an array of buckets. When multiple keys share the same bucket (collision):
1. Up to 8 entries: Stored as a singly linked list.
2. At 8 entries (and total table capacity $\ge 64$): The bucket converts from a linked list to a self-balancing **Red-Black Tree** (`TreeNode`), reducing lookup from $O(n)$ to $O(\log n)$.
3. When entries decrease to 6: The tree converts back to a linked list.

```
Table [Bucket Array]
  [0] -> null
  [1] -> Node(K1, V1) -> Node(K2, V2) [Linked List]
  [2] -> null
  [3] -> TreeNode [Red-Black Tree Root]
           /             TreeNode  TreeNode
```

---

## 7. Practical Code Examples

### Producer-Consumer Queue using `BlockingQueue`
```java
package com.roadmap.collections;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class OrderQueuePipeline {
    private static final int QUEUE_CAPACITY = 100;
    private final BlockingQueue<String> orderQueue = new ArrayBlockingQueue<>(QUEUE_CAPACITY);

    public void produceOrder(String orderId) throws InterruptedException {
        // put() blocks if the queue is full (Backpressure handling)
        orderQueue.put(orderId);
        System.out.println("Produced order: " + orderId + " | Current size: " + orderQueue.size());
    }

    public String consumeOrder() throws InterruptedException {
        // take() blocks if the queue is empty
        String orderId = orderQueue.take();
        System.out.println("Processing order: " + orderId);
        return orderId;
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a custom Least Recently Used (LRU) Cache by extending `LinkedHashMap` and overriding `removeEldestEntry()`.
2. **Assignment 2:** Demonstrate the Producer-Consumer problem using a custom bounded queue with wait/notify locks.

---

## 9. Mini-Projects
- **In-Memory Order Matching Engine:** Implement a stock or crypto limit order book using `TreeMap` to maintain bid/ask price levels with priority queues for order execution.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why does `HashMap` require the table size to always be a power of two?
**Answer:** To map a hash value to a bucket index, the modulo operator `hash % n` is mathematically equivalent to bitwise AND `hash & (n - 1)` **only when** `n` is a power of two. Bitwise AND is substantially faster at the CPU level than division/modulo. Additionally, `n - 1` produces a mask of all `1`s (e.g. 16 - 1 = 15 = `01111_2`), ensuring that all lower bits of the hash code contribute to index distribution.

### Q2: How does `ConcurrentHashMap` achieve thread safety without synchronizing all operations?
**Answer:** Unlike legacy `Hashtable` (which synchronizes every method with a single lock), `ConcurrentHashMap` uses fine-grained locking. Read operations are entirely lock-free using `volatile` reads. Write operations lock only the specific bucket head node being updated (`synchronized(node)`), or use atomic Compare-And-Swap (`CAS`) operations when creating a new bucket node. This allows concurrent writes across different buckets.

---

## 11. Revision Notes
- Remember the PECS rule: **Producer Extends, Consumer Super** for Generics wildcards.
- Prefer `List.of()` and `Map.of()` for immutable small collections (null elements are forbidden).
- For concurrent read-heavy, write-rare lists, use `CopyOnWriteArrayList`.
- Never mutate collection keys while stored in a `HashSet` or `HashMap`.

---

## 12. Topic Completion Checklist
- [ ] Understand HashMap treeification rules
- [ ] Implemented an LRU Cache with `LinkedHashMap`
- [ ] Used `BlockingQueue` for concurrent producer-consumer pipelines
- [ ] Mastered Generics bounded wildcards (PECS)
