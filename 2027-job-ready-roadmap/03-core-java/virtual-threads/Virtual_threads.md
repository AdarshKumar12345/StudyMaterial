# Java Virtual Threads – Important Topics & Learning Resources

## 1. Virtual Threads Basics
- [ ] What are Virtual Threads?
- [ ] Platform Threads vs Virtual Threads
- [ ] Why Virtual Threads?
- [ ] Project Loom
- [ ] Virtual Threads in Java 21+
- [ ] Thread-per-Request Model
- [ ] JVM Thread Scheduling

## 2. Creating Virtual Threads
- [ ] Thread.ofVirtual()
- [ ] Thread.startVirtualThread()
- [ ] Thread.Builder
- [ ] ThreadFactory
- [ ] Executors.newVirtualThreadPerTaskExecutor()
- [ ] Starting and Joining Virtual Threads
- [ ] Naming Virtual Threads

## 3. Internal Working
- [ ] Carrier Threads
- [ ] Scheduler
- [ ] Mounting and Unmounting
- [ ] Continuations
- [ ] Blocking Operations
- [ ] Continuation Yielding
- [ ] Virtual Thread Lifecycle

## 4. Synchronization and Concurrency
- [ ] synchronized with Virtual Threads
- [ ] ReentrantLock
- [ ] ThreadLocal
- [ ] InheritableThreadLocal
- [ ] Locks and Semaphores
- [ ] Race Conditions
- [ ] Deadlocks
- [ ] Thread Safety
- [ ] Structured Concurrency

## 5. Performance and Best Practices
- [ ] Virtual Threads vs Thread Pools
- [ ] CPU-Bound vs I/O-Bound Tasks
- [ ] Scalability
- [ ] Memory Consumption
- [ ] Pinning and Monitor Pinning
- [ ] JFR Monitoring
- [ ] Avoid Pooling Virtual Threads
- [ ] Database Connection Pooling
- [ ] When Not to Use Virtual Threads

## 6. Practical Programs
- [ ] Create Multiple Virtual Threads
- [ ] Run Tasks Using Virtual Thread Executor
- [ ] Compare Platform and Virtual Threads
- [ ] Build a Concurrent HTTP Client
- [ ] Handle Multiple API Requests
- [ ] Implement Producer-Consumer
- [ ] Measure Thread Execution Time
- [ ] Build a Simple Concurrent Server

## Important Learning Resources

- [GeeksforGeeks – Java Virtual Threads](https://www.geeksforgeeks.org/java/virtual-threads-in-java/)
- [Baeldung – Virtual Threads](https://www.baeldung.com/java-virtual-thread)
- [Oracle – Virtual Threads](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html)
- [Dev.java – Virtual Threads](https://dev.java/learn/new-features/virtual-threads/)
- [OpenJDK – Project Loom](https://openjdk.org/projects/loom/)