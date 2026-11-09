# RabbitMQ – Important Topics & Learning Resources

## 1. Messaging Basics
- [ ] Why message brokers (async, decoupling, load levelling)
- [ ] Producer, Consumer, Queue, Exchange, Binding
- [ ] AMQP 0-9-1 overview
- [ ] Running RabbitMQ with Docker and the management UI

## 2. Exchanges & Routing
- [ ] Direct exchange
- [ ] Fanout exchange
- [ ] Topic exchange
- [ ] Headers exchange
- [ ] Routing keys and binding patterns
- [ ] Virtual hosts, users and permissions

## 3. Reliability
- [ ] Message acknowledgements (auto vs manual)
- [ ] Durable queues and persistent messages
- [ ] Publisher confirms
- [ ] Prefetch / QoS
- [ ] Retry strategies
- [ ] Dead Letter Exchanges (DLX/DLQ)
- [ ] TTL and delayed messages
- [ ] Idempotent consumers

## 4. Spring Integration
- [ ] `spring-boot-starter-amqp`
- [ ] Declaring `Queue`, `Exchange`, `Binding` beans
- [ ] `RabbitTemplate`
- [ ] `@RabbitListener`
- [ ] Message converters (JSON)
- [ ] Listener container concurrency
- [ ] Error handling and retry configuration

## 5. Advanced
- [ ] Clustering and quorum queues
- [ ] RabbitMQ Streams (overview)
- [ ] RabbitMQ vs Kafka: when to choose which

## 6. Practical Tasks
- [ ] Email-notification service fed by a queue
- [ ] Order processing with a retry queue + DLQ
- [ ] Topic exchange routing logs by severity

## Important Learning Resources
- [RabbitMQ Tutorials](https://www.rabbitmq.com/tutorials)
- [RabbitMQ Documentation](https://www.rabbitmq.com/docs)
- [Spring AMQP Reference](https://docs.spring.io/spring-amqp/reference/)
- [Baeldung – Messaging with Spring AMQP](https://www.baeldung.com/spring-amqp)
