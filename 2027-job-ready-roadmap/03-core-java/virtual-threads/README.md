# Modern Java Virtual Threads (Project Loom)

## 1. Introduction & Prerequisites
Virtual Threads (introduced as a production feature in Java 21 LTS via Project Loom) fundamentally revolutionize Java's concurrency model. Rather than mapping 1:1 to heavy operating system threads (Platform Threads), Virtual Threads are lightweight user-mode threads managed directly by the JVM runtime.

- **Prerequisites:** Java Concurrency, ThreadPools, Blocking I/O concepts.
- **Estimated Study Time:** 12 hours
- **Difficulty:** Advanced

---

## 2. Detailed Learning Objectives
- Contrast Platform Threads (OS threads, 1MB stack, costly context switching) vs Virtual Threads (JVM-managed, dynamic heap stack, negligible memory footprint).
- Understand the carrier thread mechanism and continuation yielding during blocking operations.
- Understand thread pinning: why `synchronized` blocks and native JNI methods can temporarily pin a virtual thread to its OS carrier.
- Refactor blocking network/database code to achieve reactive-scale throughput without reactive callback complexity.
- Master Structured Concurrency and Scoped Values (Java 21 Preview / Modern Java).

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Limitations of Traditional Thread-Per-Request Architecture
- [ ] Reactive Programming (WebFlux) vs Virtual Threads Trade-offs
- [ ] Platform Thread vs Virtual Thread Internal Structure
- [ ] Carrier Thread Pool (ForkJoinPool) Scheduling Internals
- [ ] Continuation Yielding on Socket/Channel I/O
- [ ] Thread Pinning: Detection (`-Djdk.tracePinnedThreads=full`) and Resolution
- [ ] Replacing `synchronized` with `ReentrantLock` to Prevent Pinning
- [ ] `Thread.ofVirtual().start(...)` and `Executors.newVirtualThreadPerTaskExecutor()`
- [ ] ThreadLocal anti-patterns with Virtual Threads
- [ ] Structured Concurrency (`StructuredTaskScope`)

---

## 4. Free Documentation Resources
- **OpenJDK Project Loom Official Page:** [https://openjdk.org/projects/loom/](https://openjdk.org/projects/loom/)
- **JEP 444: Virtual Threads (Java 21):** [https://openjdk.org/jeps/444](https://openjdk.org/jeps/444)
- **Oracle Dev.java — Virtual Threads Guide:** [https://dev.java/learn/concurrency/virtual-threads/](https://dev.java/learn/concurrency/virtual-threads/)

---

## 5. Free Video Courses & Tutorials
- **Inside Java:** *Virtual Threads in Java 21 Explained by Ron Pressler*
- **Devoxx:** *Project Loom: Modern Concurrency for the JVM*
- **Amigoscode:** *Java 21 Virtual Threads Tutorial*

---

## 6. Important Concepts Explained

### Carrier Thread & Continuation Yielding Mechanism
When a Virtual Thread initiates a blocking I/O operation (e.g. `InputStream.read()`, socket fetch, JDBC query), the JVM unmounts the virtual thread from its underlying OS **Carrier Thread**. The continuation's execution stack is preserved in JVM heap memory. The carrier thread is immediately freed to run another virtual thread. Once the OS signals that I/O data is ready, the JVM schedules the virtual thread onto an available carrier thread to resume execution seamlessly.

```
┌────────────────────────────────────────────────────────┐
│  Virtual Threads (Millions in Heap Memory)             │
│  [ VT 1 ]  [ VT 2 (Blocked on I/O) ]  [ VT 3 ] ...     │
└───────────────────────┬────────────────────────────────┘
                        │ Mount / Unmount
                        ▼
┌────────────────────────────────────────────────────────┐
│  Carrier Threads (ForkJoinPool - Matches CPU Cores)    │
│  [ Carrier Thread 1 ]   [ Carrier Thread 2 ] ...       │
└───────────────────────┬────────────────────────────────┘
                        ▼
┌────────────────────────────────────────────────────────┐
│  Operating System Kernel Threads                       │
└────────────────────────────────────────────────────────┘
```

---

## 7. Practical Code Examples

### Running 10,000 Concurrent HTTP Requests using Virtual Threads
```java
package com.roadmap.virtualthreads;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.Executors;
import java.time.Duration;

public class HighThroughputScraper {

    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();

        // Virtual-thread-per-task executor: No thread pooling!
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            HttpClient client = HttpClient.newBuilder()
                    .connectTimeout(Duration.ofSeconds(3))
                    .build();

            for (int i = 1; i <= 10_000; i++) {
                final int taskId = i;
                executor.submit(() -> {
                    try {
                        HttpRequest request = HttpRequest.newBuilder()
                                .uri(URI.create("https://httpbin.org/delay/1"))
                                .GET()
                                .build();

                        // Blocking call that automatically unmounts from carrier thread
                        var response = client.send(request, HttpResponse.BodyHandlers.discarding());
                        if (taskId % 1000 == 0) {
                            System.out.println("Completed task: " + taskId + " | Status: " + response.statusCode());
                        }
                    } catch (Exception e) {
                        System.err.println("Task failed: " + taskId);
                    }
                });
            }
        } // Auto-closes executor and waits for all 10,000 tasks to finish

        long totalTime = System.currentTimeMillis() - startTime;
        System.out.println("Finished 10,000 concurrent requests in: " + totalTime + " ms");
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Benchmark launching 100,000 Platform Threads vs 100,000 Virtual Threads and measure heap consumption and time to spawn.
2. **Assignment 2:** Run a benchmark with `-Djdk.tracePinnedThreads=full` on code using `synchronized` vs `ReentrantLock` during `Thread.sleep()` to observe carrier thread pinning.

---

## 9. Mini-Projects
- **High-Throughput Synthetic Load Generator:** Build an HTTP load testing CLI tool using Virtual Threads that sends 5,000 requests/second to an endpoint and logs p50, p95, and p99 response latencies.

---

## 10. Interview Questions & Detailed Answers

### Q1: Should you pool Virtual Threads like traditional Platform Threads?
**Answer:** **No, never pool Virtual Threads.** Platform threads are pooled because creating an OS thread requires allocating ~1MB of stack memory and making kernel calls. Virtual Threads are cheap JVM objects that cost only a few hundred bytes initially. Pooling them defeats their purpose and restricts concurrency. Instead, create a new virtual thread per task using `Executors.newVirtualThreadPerTaskExecutor()`.

### Q2: What is Thread Pinning in Project Loom?
**Answer:** Thread Pinning occurs when a Virtual Thread cannot be unmounted from its Carrier Thread during a blocking call. This happens in two scenarios:
1. Inside a `synchronized` block or method.
2. Inside a native C/C++ method or foreign function call.
When pinned, the carrier OS thread remains blocked, diminishing concurrency. To fix this, replace `synchronized` with `java.util.concurrent.locks.ReentrantLock`.

---

## 11. Revision Notes
- Do not pool virtual threads.
- Replace `synchronized` blocks with `ReentrantLock` to avoid pinning.
- Avoid storing large objects in `ThreadLocal` variables when millions of virtual threads exist.
- Virtual threads are intended for I/O-bound tasks, not CPU-bound computation (use parallel streams for CPU).

---

## 12. Topic Completion Checklist
- [ ] Understand Carrier threads and continuation yielding
- [ ] Used `Executors.newVirtualThreadPerTaskExecutor()`
- [ ] Diagnosed and resolved Thread Pinning
- [ ] Implemented high-concurrency I/O pipelines
