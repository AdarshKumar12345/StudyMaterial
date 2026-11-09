# RabbitMQ Message Broker & AMQP Protocol

## 1. Introduction & Prerequisites
RabbitMQ is an open-source message broker implementing the Advanced Message Queuing Protocol (AMQP). It provides decoupled asynchronous communication, reliable message queuing, delivery guarantees, routing topologies, and backpressure handling between microservices.

- **Prerequisites:** Microservices basics, Asynchronous processing concepts.
- **Estimated Study Time:** 12 hours
- **Difficulty:** Intermediate

---

## 2. Detailed Learning Objectives
- Master core AMQP entities: Producers, Exchanges, Queues, Bindings, Routing Keys, and Consumers.
- Understand all 4 Exchange Types: Direct, Fanout, Topic, and Headers.
- Master message reliability: Publisher Confirms, Consumer Acknowledgments (`basic.ack`, `basic.nack`), and Persistent Messages.
- Implement Dead Letter Exchanges (DLX) and Dead Letter Queues (DLQ) for failed message handling.
- Integrate RabbitMQ with Spring Boot using Spring AMQP (`RabbitTemplate`, `@RabbitListener`).

---

## 3. Beginner-to-Advanced Topic Checklist
- [ ] Synchronous REST vs Asynchronous Messaging Trade-offs
- [ ] AMQP 0-9-1 Architecture & Connection/Channel multiplexing
- [ ] Exchange Types: Direct (exact match), Fanout (broadcast), Topic (wildcards `*` and `#`), Headers
- [ ] Message Durability: Durable Queues vs Transient Queues
- [ ] Consumer Acknowledgments: Auto-Ack vs Manual Ack (`MANUAL`)
- [ ] Dead Letter Exchanges (DLX) & Retry policies
- [ ] Message TTL and Expiration
- [ ] Prefetch Count & Fair Dispatching (`basic.qos`)
- [ ] Spring AMQP: `@RabbitListener`, `RabbitTemplate`, Jackson JSON message converter
- [ ] RabbitMQ Clustering and Quorum Queues (Raft consensus)

---

## 4. Free Documentation Resources
- **RabbitMQ Official Tutorials (Java & Spring):** [https://www.rabbitmq.com/getstarted.html](https://www.rabbitmq.com/getstarted.html)
- **Spring AMQP Official Documentation:** [https://docs.spring.io/spring-amqp/reference/](https://docs.spring.io/spring-amqp/reference/)

---

## 5. Free Video Courses & Tutorials
- **Hussein Nasser:** *RabbitMQ Crash Course*
- **TechWorld with Nana:** *RabbitMQ in 20 Minutes*
- **Java Brains:** *Spring Boot & RabbitMQ Messaging*

---

## 6. Important Concepts Explained

### Exchange Types & Routing Mechanics
```
  Producer ──► [ Exchange ]
                     │
       ┌─────────────┼─────────────┐
       │ (Direct)    │ (Fanout)    │ (Topic)
       ▼             ▼             ▼
   Exact Key      Broadcast     Wildcard Match
   "order.new"    All Queues    "order.*" or "log.#"
       │             │             │
       ▼             ▼             ▼
   [Queue A]     [Queue B]     [Queue C]
```

---

## 7. Practical Code Examples

### Robust Spring Boot RabbitMQ Consumer with Manual Acknowledgment & DLQ
```java
package com.roadmap.rabbitmq.consumer;

import com.rabbitmq.client.Channel;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

import java.io.IOException;

@Component
public class OrderProcessingConsumer {

    @RabbitListener(queues = "orders.processing.queue", ackMode = "MANUAL")
    public void processOrder(String orderPayload, Channel channel, Message message) throws IOException {
        long deliveryTag = message.getMessageProperties().getDeliveryTag();

        try {
            System.out.println("Processing order message: " + orderPayload);
            // Execute business logic...

            // Manually acknowledge successful receipt
            channel.basicAck(deliveryTag, false);
        } catch (Exception ex) {
            System.err.println("Processing failed, routing to Dead Letter Queue: " + ex.getMessage());
            // Negative acknowledgment: requeue=false routes message directly to DLX
            channel.basicNack(deliveryTag, false, false);
        }
    }
}
```

---

## 8. Practice Assignments
1. **Assignment 1:** Configure a RabbitMQ Topic Exchange where notifications are routed to `email.queue` for `*.email` and `sms.queue` for `*.sms`.
2. **Assignment 2:** Configure a Dead Letter Exchange that captures messages after 3 failed retries.

---

## 9. Mini-Projects
- **Asynchronous Invoice Generation System:** A Spring Boot architecture where an Order Service sends messages to RabbitMQ, an Invoice Worker generates PDF invoices asynchronously, and failures route to a DLQ for operational review.

---

## 10. Interview Questions & Detailed Answers

### Q1: What is the difference between RabbitMQ and Apache Kafka?
**Answer:** RabbitMQ is a traditional message broker oriented around smart brokers and dumb consumers. It routes messages via exchanges into queues, removes messages immediately upon consumer acknowledgment, and excels at complex routing, task queues, and low-latency transactional workflows. Kafka is an append-only distributed commit log oriented around dumb brokers and smart consumers. It retains messages for days/weeks, allowing multiple consumer groups to replay event streams at high throughput (millions/sec).

### Q2: Why is consumer `basic.qos(prefetchCount = 1)` important in RabbitMQ?
**Answer:** By default, RabbitMQ dispatches all messages to consumers round-robin as soon as they arrive, regardless of consumer workload. If one consumer gets stuck on an intensive task while another sits idle, messages pile up unevenly. Setting `prefetchCount = 1` tells RabbitMQ not to give more than one unacknowledged message to a worker at a time, ensuring fair workload distribution.

---

## 11. Revision Notes
- Use manual acknowledgments for mission-critical messages.
- Always configure a Dead Letter Exchange (DLX) for poison-pill messages.
- Set `channel.basicQos()` to prevent consumers from being overwhelmed.

---

## 12. Topic Completion Checklist
- [ ] Understand AMQP architecture and channel multiplexing
- [ ] Used Direct, Fanout, and Topic exchanges
- [ ] Implemented manual ACKs and DLQ routing
- [ ] Configured Spring AMQP with Jackson converter
