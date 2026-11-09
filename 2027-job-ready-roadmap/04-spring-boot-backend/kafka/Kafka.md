# Apache Kafka – Important Topics & Learning Resources

## 1. Kafka Basics
- [ ] What is Kafka and event streaming
- [ ] Broker, Topic, Partition, Offset
- [ ] Producer and Consumer
- [ ] Consumer Groups
- [ ] Kafka vs traditional message queues
- [ ] Running Kafka with Docker (KRaft mode)
- [ ] CLI tools (`kafka-topics`, `kafka-console-producer/consumer`)

## 2. Architecture
- [ ] Replication, leader and followers, ISR
- [ ] Log segments and retention
- [ ] Log compaction
- [ ] Partitioning and keys (ordering guarantees)
- [ ] KRaft vs ZooKeeper
- [ ] Rebalancing

## 3. Producers & Consumers
- [ ] `acks` (0, 1, all)
- [ ] Batching, `linger.ms`, compression
- [ ] Idempotent producer
- [ ] Offset commit strategies (auto vs manual)
- [ ] Delivery semantics: at-most-once, at-least-once, exactly-once
- [ ] Transactions
- [ ] Serialization (JSON, Avro) and Schema Registry

## 4. Spring Integration
- [ ] `spring-kafka`
- [ ] `KafkaTemplate`
- [ ] `@KafkaListener`
- [ ] Consumer and producer configuration in `application.yml`
- [ ] Error handling, retries and `DeadLetterPublishingRecoverer`
- [ ] `@RetryableTopic`
- [ ] Testing with Testcontainers / `EmbeddedKafka`

## 5. Advanced
- [ ] Kafka Streams
- [ ] Kafka Connect
- [ ] Outbox pattern and CDC (Debezium)
- [ ] Event-driven architecture and Saga pattern
- [ ] Monitoring consumer lag

## 6. Practical Tasks
- [ ] Order service publishing events, inventory service consuming them
- [ ] Add a DLT and retry topics
- [ ] Scale consumers and observe partition assignment

## Important Learning Resources
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Confluent Developer – Kafka 101](https://developer.confluent.io/courses/apache-kafka/events/)
- [Spring for Apache Kafka Reference](https://docs.spring.io/spring-kafka/reference/)
- [Baeldung – Spring Kafka](https://www.baeldung.com/spring-kafka)
