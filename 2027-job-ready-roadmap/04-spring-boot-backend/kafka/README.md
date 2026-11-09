# Apache Kafka Distributed Event Streaming Architecture

## 1. Introduction & Prerequisites
Apache Kafka is an open-source distributed event streaming platform used by over 80% of Fortune 100 companies. It provides high-throughput, low-latency, fault-tolerant publish-subscribe pipelines, event sourcing, stream processing, and data integration across distributed microservice ecosystems.

- **Prerequisites:** Java Concurrency, Networking basics, Message queues.
- **Estimated Study Time:** 22 hours
- **Difficulty:** Advanced

---

## 2. Detailed Learning Objectives
- Master Kafka fundamentals: Topics, Partitions, Offsets, Brokers, Producers, Consumers, and Consumer Groups.
- Understand partition ordering guarantees and message keys hashing.
- Master Producer configurations: `acks` (`0`, `1`, `all`), idempotence (`enable.idempotence=true`), and batching (`linger.ms`, `batch.size`).
- Master Consumer rebalancing, offset commit strategies (automatic vs manual `commitSync`/`commitAsync`), and Consumer Lag.
- Understand KRaft (Kafka Raft Metadata mode) replacing legacy Apache ZooKeeper.
- Integrate Kafka with Spring Boot using Spring for Apache Kafka (`KafkaTemplate`, `@KafkaListener`).

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Distributed Commit Log Architecture & Zero-Copy OS Disk Reads (`sendfile`)
- [ ] Topics, Partitions, and Consumer Scaling Limits
- [ ] Partition Leaders, Followers, and In-Sync Replicas (ISR)
- [ ] Producer Internals: Partitioner, RecordAccumulator, Serializers
- [ ] Producer Delivery Guarantees: `acks=all`, `min.insync.replicas`, retries
- [ ] Consumer Groups & Horizontal Scaling: 1 Partition = Max 1 Consumer in a Group
- [ ] Consumer Offsets (`__consumer_offsets` topic) & Manual Commit Modes
- [ ] Rebalance Protocol (Cooperative Sticky Assignor)
- [ ] Exactly-Once Semantics (EOS) and Transactional Producers
- [ ] Monitoring Consumer Lag via Prometheus and CLI

---

## 4. Free Documentation Resources
- **Apache Kafka Official Documentation:** [https://kafka.apache.org/documentation/](https://kafka.apache.org/documentation/)
- **Confluent Developer (Free Kafka Tutorials & Architecture Guides):** [https://developer.confluent.io/](https://developer.confluent.io/)
- **Baeldung — Spring Kafka Guide:** [https://www.baeldung.com/spring-kafka](https://www.baeldung.com/spring-kafka)

---

## 5. Free Video Courses & Tutorials
- **freeCodeCamp:** *Apache Kafka Crash Course*
- **Hussein Nasser:** *Kafka Architecture and Internals*
- **Amigoscode:** *Spring Boot & Apache Kafka Tutorial*

---

## 6. Important Concepts Explained

### Distributed Commit Log & Consumer Group Scaling
Messages published to a Kafka topic are appended to an immutable, ordered commit log partitioned across brokers. Consumers subscribe as part of a **Consumer Group**: each partition is consumed by exactly one consumer within the group.

```
 Topic: 'order-events' (3 Partitions)
 ┌────────────────────────────────────────────────────────┐
 │ Partition 0: [msg0] [msg1] [msg2] ───► Consumer A     │
 │ Partition 1: [msg0] [msg1] [msg2] ───► Consumer B     │ (Consumer Group 1)
 │ Partition 2: [msg0] [msg1] [msg2] ───► Consumer C     │
 └────────────────────────────────────────────────────────┘
```

---

## 7. Practical Code Examples

### Spring Boot Idempotent Kafka Producer & Consumer with Manual ACK
```java
package com.roadmap.kafka.service;

import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.kafka.support.Acknowledgment;
import org.springframework.stereotype.Service;

public record OrderEvent(String orderId, String userId, double totalAmount) {}

@Service
public class OrderEventService {

    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;

    public OrderEventService(KafkaTemplate<String, OrderEvent> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void publishOrder(OrderEvent event) {
        // Using orderId as message key guarantees all events for this order hit the same partition!
        kafkaTemplate.send("orders.topic", event.orderId(), event)
            .whenComplete((result, ex) -> {
                if (ex == null) {
                    System.out.printf("Message delivered to partition %d with offset %d%n",
                        result.getRecordMetadata().partition(),
                        result.getRecordMetadata().offset());
                } else {
                    System.err.println("Delivery failed: " + ex.getMessage());
                }
            });
    }

    @KafkaListener(topics = "orders.topic", groupId = "order-fulfillment-group", containerFactory = "kafkaListenerContainerFactory")
    public void consumeOrder(OrderEvent event, Acknowledgment ack) {
        System.out.println("Processing order: " + event.orderId());
        // Acknowledge offset manually only after successful business processing
        ack.acknowledge();
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Start a multi-broker Kafka cluster using Docker Compose and inspect topics and consumer groups using CLI tools (`kafka-topics.sh`, `kafka-consumer-groups.sh`).
2. **Assignment 2:** Produce messages with identical partition keys and verify that order is strictly preserved within the assigned partition.

---

## 9. Mini-Projects
- **Real-Time Financial Transaction Audit Stream:** Build an event-driven fraud detection pipeline where transactions are produced to Kafka, validated by consumer workers, and flagged anomalies are written to an alert topic.

---

## 10. Interview Questions & Detailed Answers

### Q1: How does Kafka guarantee message ordering?
**Answer:** Kafka guarantees strict message ordering **only within a single partition**, never across multiple partitions of a topic. To preserve ordering for related events (e.g. all updates for `user_123`), the producer assigns a message key (e.g. `userId`). Kafka hashes the key (`murmur2(key) % numPartitions`) to deterministically route all events with that key to the exact same partition.

### Q2: What configuration guarantees Zero Data Loss (At-Least-Once / Durable writes) in Kafka?
**Answer:**
1. Producer: `acks = all` (producer waits until all In-Sync Replicas acknowledge).
2. Producer: `enable.idempotence = true` (prevents duplicate messages on network retries).
3. Broker: `min.insync.replicas = 2` (ensures at least two ISRs have committed before ACK).
4. Broker: Topic replication factor $\ge 3$.
5. Consumer: `enable.auto.commit = false` (commit offset only after business processing completes).

---

## 11. Revision Notes
- Ordering is guaranteed only per partition, never across a whole topic.
- You cannot have more active consumers in a group than there are partitions.
- Always use `acks=all` and `min.insync.replicas >= 2` for critical financial events.

---

## 12. Topic Completion Checklist
- [ ] Understand Commit Log and Zero-Copy architecture
- [ ] Configured idempotent producer with `acks=all`
- [ ] Mastered Consumer Groups and Partition key routing
- [ ] Implemented manual offset acknowledgment in Spring Kafka
