# Redis Distributed Caching & In-Memory Data Structures

## 1. Introduction & Prerequisites
Redis (Remote Dictionary Server) is an in-memory key-value data structure store used as a distributed cache, message broker, and fast database. In enterprise backend systems, Redis offloads read traffic from relational databases, reduces endpoint response latency to sub-milliseconds, and provides distributed synchronization.

- **Prerequisites:** Key-Value concepts, Spring Boot basics.
- **Estimated Study Time:** 12 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Master core Redis data structures: Strings, Hashes, Lists, Sets, Sorted Sets (ZSet), Bitmaps, and HyperLogLog.
- Implement caching design patterns: Cache-Aside (Lazy Loading), Write-Through, and Write-Behind.
- Prevent caching failure modes: Cache Penetration (Bloom filters), Cache Stampede (Probabilistic early expiration), and Cache Avalanche (Jittered TTL).
- Implement Distributed Locks using Redisson and Redis atomic commands (`SET key value NX PX`).
- Integrate Redis with Spring Boot using Spring Data Redis and `@Cacheable`.

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] In-Memory Architecture, Single-Threaded Event Loop, and I/O Multiplexing (epoll)
- [ ] Core Data Types: Strings, Hashes, Lists, Sets, Sorted Sets (ZSet)
- [ ] Time-To-Live (TTL) and Key Expiration Policies
- [ ] Memory Eviction Policies: `noeviction`, `allkeys-lru`, `volatile-lru`, `allkeys-lfu`
- [ ] Caching Strategies: Cache-Aside vs Write-Through vs Write-Behind
- [ ] Failure Modes: Cache Penetration, Avalanche, and Stampede Mitigation
- [ ] Spring Cache Abstraction: `@Cacheable`, `@CachePut`, `@CacheEvict`
- [ ] Distributed Locking with Redisson (handling node crashes and lease renewal)
- [ ] Redis Persistence: RDB (Snapshots) vs AOF (Append-Only File)
- [ ] Redis Sentinel (High Availability) & Redis Cluster (Sharding)

---

## 4. Free Documentation Resources
- **Redis Official Documentation:** [https://redis.io/docs/](https://redis.io/docs/)
- **Redis University (Free Courses):** [https://university.redis.com/](https://university.redis.com/)
- **Baeldung — Guide to Spring Data Redis:** [https://www.baeldung.com/spring-data-redis-tutorial](https://www.baeldung.com/spring-data-redis-tutorial)

---

## 5. Free Video Courses & Tutorials
- **Hussein Nasser:** *Redis Crash Course & Architecture Internals*
- **freeCodeCamp:** *Redis Course for Beginners*
- **Amigoscode:** *Spring Boot Redis Cache Tutorial*

---

## 6. Important Concepts Explained

### Cache-Aside (Lazy Loading) Architecture
1. Application receives read request.
2. App checks Redis:
   - **Cache HIT:** Returns cached data directly (~1ms).
   - **Cache MISS:** Reads from Database (~50ms), writes data into Redis with a TTL, and returns data to client.

```
 Client Request
       │
       ▼
 ┌───────────┐    1. Cache Check    ┌─────────────┐
 │  Backend  ├─────────────────────►│ Redis Cache │
 │  Service  │◄─────────────────────┤             │
 └─────┬─────┘    2. Cache Hit!     └─────────────┘
       │
       │ (On Cache Miss)
       ▼
 ┌───────────┐
 │ PostgreSQL│
 │ Database  │
 └───────────┘
```

---

## 7. Practical Code Examples

### Spring Boot Distributed Caching with `@Cacheable` and JSON Serialization
```java
package com.roadmap.redis.service;

import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

public record ProductDto(String id, String title, double price) {}

@Service
public class ProductService {

    // Cache-Aside: Reads from Redis 'products' cache first; calls DB only on cache miss
    @Cacheable(value = "products", key = "#productId", unless = "#result == null")
    public ProductDto getProductById(String productId) {
        System.out.println("CACHE MISS: Fetching product from primary database: " + productId);
        // Simulate slow relational database query
        return new ProductDto(productId, "Developer Mechanical Keyboard", 149.99);
    }

    // Invalidate cache when product details are updated
    @CacheEvict(value = "products", key = "#productId")
    public void updateProduct(String productId, ProductDto updated) {
        System.out.println("Updating database and evicting Redis cache for: " + productId);
        // Save to DB...
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Build a real-time leaderboard using Redis Sorted Sets (`ZSet`) with operations to add player scores (`ZADD`), retrieve ranks (`ZRANK`), and get the top 10 players (`ZREVRANGE`).
2. **Assignment 2:** Simulate a Cache Avalanche scenario and fix it by introducing a randomized TTL jitter (e.g., base TTL 3600s + random(0, 300)s).

---

## 9. Mini-Projects
- **Redis Rate Limiter Middleware:** Build a distributed rate limiter in Spring Boot using Redis atomic increment (`INCR`) and `EXPIRE` to restrict client IP addresses to 60 requests/minute.

---

## 10. Interview Questions & Detailed Answers

### Q1: Why is Redis single-threaded yet extremely fast?
**Answer:** Redis runs its core command execution in a single thread, which eliminates expensive CPU context-switching and synchronization lock contention. It achieves 100,000+ operations/second because:
1. All data is kept entirely in RAM.
2. It uses non-blocking I/O multiplexing (such as `epoll` on Linux or `kqueue` on macOS) to handle tens of thousands of concurrent client connections on a single thread.
3. Data structures are custom-optimized for low-level memory efficiency.

### Q2: What is Cache Stampede and how do you prevent it?
**Answer:** A Cache Stampede (or Dog-Piling) occurs when a heavily requested cached key expires. Thousands of concurrent requests experience a cache miss at the exact same moment and all execute the expensive database query simultaneously, crashing the database. Prevention techniques include:
1. Using a distributed mutex lock (Redisson) so only one thread recomputes the cache while others wait.
2. Probabilistic Early Expiration (XFetch algorithm) where background threads refresh the cache slightly before it actually expires.

---

## 11. Revision Notes
- Always set a TTL on cached entries to avoid memory exhaustion.
- Never use the `KEYS *` command in production (it blocks the single thread); use `SCAN` instead.
- Use Redis Hashes to store objects with multiple fields rather than serializing the whole object into a single String.

---

## 12. Topic Completion Checklist
- [ ] Understand Redis single-threaded event loop
- [ ] Used Redis Strings, Hashes, and Sorted Sets
- [ ] Configured `@Cacheable` and `@CacheEvict` in Spring Boot
- [ ] Prevented Cache Penetration and Avalanche
