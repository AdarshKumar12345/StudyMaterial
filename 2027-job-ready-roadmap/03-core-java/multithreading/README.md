# Java Concurrency, Multithreading & Thread Safety

## 1. Introduction & Prerequisites
Concurrency in Java allows multi-core CPUs to execute multiple instruction streams simultaneously. Understanding memory visibility, race conditions, synchronization primitives, the `java.util.concurrent` (JUC) package, and thread pools is required for building high-throughput systems.

- **Prerequisites:** Java Fundamentals, OOP, Collections.
- **Estimated Study Time:** 22 hours
- **Difficulty:** Advanced

---

## 2. Detailed Learning Objectives
- Master the Java Memory Model (JMM), happens-before relationships, and visibility guarantees.
- Differentiate between `synchronized`, `volatile`, and atomic variables (`AtomicInteger`, `AtomicReference`).
- Use explicit locks: `ReentrantLock`, `ReadWriteLock`, and `StampedLock` for optimistic locking.
- Prevent concurrency hazards: Deadlocks, Livelocks, Starvation, and Race Conditions.
- Master thread coordination using `CountDownLatch`, `CyclicBarrier`, and `Semaphore`.
- Build custom thread pool architectures with `ThreadPoolExecutor`.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Thread Lifecycle: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED
- [ ] Creating Threads: `Thread`, `Runnable`, and `Callable<V>` with `Future<V>`
- [ ] Java Memory Model (JMM), CPU Caches & Hardware Coherence
- [ ] Memory Visibility and the `volatile` keyword
- [ ] Intrinsic Locks (`synchronized` method and block granularity)
- [ ] Lock-Free Atomic Operations: Hardware CAS (Compare-And-Swap)
- [ ] Explicit Locking: `ReentrantLock` (fairness, `tryLock()`, conditions)
- [ ] Concurrent Synchronizers: `CountDownLatch`, `CyclicBarrier`, `Semaphore`
- [ ] Thread Pools: `ExecutorService`, `Executors.newFixedThreadPool`, `ThreadPoolExecutor` parameters
- [ ] Deadlock Detection using `jstack` and thread dumps

---

## 4. Free Documentation Resources
- **Java Concurrency Utilities Guide:** [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/package-summary.html)
- **Baeldung Concurrency Series:** [https://www.baeldung.com/java-concurrency](https://www.baeldung.com/java-concurrency)
- **Java Concurrency in Practice (Summaries):** [https://jcip.net/](https://jcip.net/)

---

## 5. Free Video Courses & Tutorials
- **Defog Tech:** Java Concurrency & Multithreading Playlist
- **Java Brains:** Concurrency and Multithreading in Java
- **Jakob Jenkov:** Java Concurrency Tutorials

---

## 6. Important Concepts Explained

### The Java Memory Model & Volatile Keyword
Each CPU core possesses L1/L2 caches. Without synchronization, writes to a variable made by Core 1 may remain in its L1 cache and never be flushed to main RAM, leaving Core 2 reading stale data. The `volatile` modifier establishes a **happens-before** edge: writes to a volatile variable are immediately flushed to main memory, and reads always fetch directly from main memory. Note that `volatile` guarantees **visibility**, but NOT **atomicity** (e.g., `count++` is a read-modify-write composite operation and is not atomic).

```
   CPU Core 1                    CPU Core 2
┌──────────────┐              ┌──────────────┐
│  L1/L2 Cache │              │  L1/L2 Cache │
└──────┬───────┘              └──────┬───────┘
       │ Writes                      │ Reads
       ▼                             ▼
┌────────────────────────────────────────────┐
│         Main Memory (RAM)                  │
│       [ volatile boolean flag ]            │
└────────────────────────────────────────────┘
```

---

## 7. Practical Code Examples

### Thread-Safe Rate Limiter using `ReentrantLock`
```java
package com.roadmap.concurrency;

import java.util.concurrent.locks.ReentrantLock;

public class ThreadSafeCounter {
    private int counter = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock(); // Blocks until lock is acquired
        try {
            counter++;
        } finally {
            lock.unlock(); // Always release in finally block!
        }
    }

    public boolean tryIncrementWithTimeout(long timeoutMillis) throws InterruptedException {
        if (lock.tryLock(timeoutMillis, java.util.concurrent.TimeUnit.MILLISECONDS)) {
            try {
                counter++;
                return true;
            } finally {
                lock.unlock();
            }
        }
        return false; // Timed out waiting for lock
    }

    public int getValue() {
        lock.lock();
        try {
            return counter;
        } finally {
            lock.unlock();
        }
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a multi-threaded web crawler that fetches URLs concurrently using an `ExecutorService` and an `AtomicInteger` to track page count.
2. **Assignment 2:** Simulate a bank transfer service with account locking that intentionally causes and then fixes a deadlock using ordered resource acquisition.

---

## 9. Mini-Projects
- **High-Throughput Order Ingestion Queue:** Build a thread-safe order intake buffer using a bounded `ArrayBlockingQueue` with multiple producer worker threads and consumer worker threads with graceful shutdown via `shutdown()` and `awaitTermination()`.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between `synchronized` and `ReentrantLock`?
**Answer:** `synchronized` is an intrinsic language-level construct whose locks are automatically acquired and released. `ReentrantLock` is an explicit class offering advanced capabilities: fairness policies (giving lock to longest-waiting thread), interruptible lock acquisition (`lockInterruptibly()`), non-blocking attempts (`tryLock()`), and multiple associated `Condition` objects.

### Q2: What are the 4 core parameters of `ThreadPoolExecutor`?
**Answer:**
1. `corePoolSize`: Minimum number of threads kept alive even if idle.
2. `maximumPoolSize`: Maximum allowed threads when queue fills up.
3. `keepAliveTime`: Time excess idle threads above corePoolSize wait before termination.
4. `workQueue`: The `BlockingQueue` used to hold tasks before execution.

---

## 11. Revision Notes
- Always call `lock.unlock()` inside a `finally` block.
- For simple counters, use `AtomicInteger` or `LongAdder` instead of locks.
- Never use `Executors.newCachedThreadPool()` in production (unbounded thread creation causes OOM).
- Thread interruption is cooperative: check `Thread.currentThread().isInterrupted()`.

---

## 12. Topic Completion Checklist
- [ ] Understand happens-before relationships and JMM
- [ ] Used `ReentrantLock` and `tryLock()`
- [ ] Solved Deadlock via deterministic lock ordering
- [ ] Configured custom `ThreadPoolExecutor` with a bounded queue
