# Redis Caching – Important Topics & Learning Resources

## 1. Redis Basics
- [ ] What is Redis and common use cases
- [ ] In-memory data store and single-threaded model
- [ ] Installing Redis (Docker) and `redis-cli`
- [ ] Data types: String, List, Set, Sorted Set, Hash
- [ ] Key naming conventions
- [ ] TTL and expiration (`EXPIRE`, `TTL`)

## 2. Persistence & Operations
- [ ] RDB vs AOF
- [ ] Eviction policies (LRU, LFU, `maxmemory`)
- [ ] Replication and Sentinel
- [ ] Redis Cluster basics
- [ ] Pub/Sub and Streams (overview)

## 3. Caching Patterns
- [ ] Cache-Aside
- [ ] Read-Through / Write-Through / Write-Behind
- [ ] Cache invalidation strategies
- [ ] Cache stampede, penetration and avalanche
- [ ] Choosing TTLs

## 4. Spring Integration
- [ ] `spring-boot-starter-data-redis`
- [ ] `RedisTemplate` / `StringRedisTemplate`
- [ ] Serializers (JSON vs JDK)
- [ ] Spring Cache abstraction: `@EnableCaching`, `@Cacheable`, `@CachePut`, `@CacheEvict`
- [ ] Per-cache TTL configuration with `RedisCacheManager`
- [ ] Lettuce vs Jedis

## 5. Advanced Use Cases
- [ ] Distributed locks (Redisson)
- [ ] Rate limiting with Redis
- [ ] Session storage (Spring Session)
- [ ] Leaderboards with Sorted Sets

## 6. Practical Tasks
- [ ] Cache a slow DB query using `@Cacheable` and measure the speedup
- [ ] Build a rate limiter for an API
- [ ] Implement a distributed lock for a scheduled job

## Important Learning Resources
- [Redis Documentation](https://redis.io/docs/)
- [Try Redis / Redis University](https://university.redis.io/)
- [Spring Data Redis Reference](https://docs.spring.io/spring-data/redis/reference/)
- [Baeldung – Spring Data Redis](https://www.baeldung.com/spring-data-redis-tutorial)
- [Redisson](https://redisson.org/)
