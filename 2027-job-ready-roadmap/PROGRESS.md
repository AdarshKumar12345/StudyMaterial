# 📈 2027 Job-Ready Roadmap — Progress Tracker

This document provides a manual tracking matrix as well as documentation on how the automated client-side study dashboard synchronizes your study milestones.

---

## 🧭 How Progress Tracking Works

1. **Local Storage System:** When using the **Study Dashboard** (`website/index.html`), your completed topics, current study module, daily streak, and active notes are saved in your browser's `localStorage` under the key `job_ready_roadmap_progress_v1`.
2. **Device Independence & Privacy:** Your data never leaves your browser. No external database or credentials are required.
3. **Export & Import Backup:** You can export your progress to a `.json` backup file at any time from the dashboard settings and import it onto your MacBook, desktop, or mobile device.
4. **Manual Markdown Checklists:** If you prefer offline tracking in Git, mark items below with `[x]` and commit your changes directly!

---

## 📊 High-Level Curriculum Summary

- **Total Modules:** 12 Modules
- **Total In-Depth Topics:** 67 Specialized Domains
- **Core Stacks:** Java 21+ / Spring Boot 3+ / Microservices / AWS & DevOps / AI Engineering & MCP / System Design & Core CS

---

## 📋 Comprehensive Topic Checklist

### Module 01: Modern Frontend Development
- [ ] HTML5 Semantic Architecture & Web Accessibility (WCAG / ARIA)
- [ ] Modern CSS3, Flexbox, Grid, Custom Properties & Responsive Layouts
- [ ] Modern JavaScript (ES6+), Event Loop, Promises, Async/Await & DOM
- [ ] Tailwind CSS Utility-First Architecture & Design Systems
- [ ] Modern React (Hooks, Context, State Management & Component Lifecycle)

### Module 02: Developer Environment & Tooling
- [ ] Git Internals, Branching Strategies, GitHub Workflows & Merge Conflicts
- [ ] Linux Command Line Mastery, Bash Scripting, Permissions & Process Management

### Module 03: Core Java Mastery (Java 21 LTS)
- [ ] Java Fundamentals, Memory Model (Stack vs Heap), JVM Internals
- [ ] Object-Oriented Programming (SOLID, Abstraction, Polymorphism, Design Patterns)
- [ ] Java Collections Framework (List, Set, Map Internals, Big-O Complexity)
- [ ] Traditional Multithreading, Concurrency, Synchronization & Locks
- [ ] Modern Java Virtual Threads (Project Loom) & High-Throughput I/O
- [ ] Functional Programming, Lambdas, Built-in Functional Interfaces
- [ ] Stream API & Parallel Streams for Data Processing
- [ ] Modern File Handling (NIO.2, Path, Files & Channel Buffers)

### Module 04: Enterprise Backend with Spring Boot 3+
- [ ] Apache Maven & Build Lifecycle, Dependency Scopes & Plugins
- [ ] Spring Core Framework, Inversion of Control (IoC) & Dependency Injection
- [ ] Spring Boot 3 Architecture, Auto-Configuration & REST APIs
- [ ] Hibernate ORM, JPA, Entity Relationships, N+1 Query Problem & Auditing
- [ ] Spring Security 6, JWT Authentication, OAuth2 & Role-Based Access Control
- [ ] Redis Distributed Caching, Cache-Aside Pattern & Eviction Policies
- [ ] Elasticsearch for Full-Text Search, Document Indexing & Aggregations
- [ ] RabbitMQ Message Broker, Exchanges, Queues & Dead Letter Queues
- [ ] Apache Kafka Event Streaming, Partitions, Consumer Groups & Offsets

### Module 05: Cloud Infrastructure & DevOps
- [ ] Amazon Web Services (AWS) Core Services (EC2, S3, RDS, IAM, VPC)
- [ ] Docker Containerization, Multi-Stage Dockerfiles & Compose Orchestration
- [ ] Kubernetes (Pods, Deployments, Services, Ingress, ConfigMaps, Secrets)
- [ ] Jenkins CI/CD Pipelines, Declarative Jenkinsfiles & Automated Testing
- [ ] Nginx Reverse Proxy, Load Balancing, SSL/TLS Termination & Caching

### Module 06: Production Microservices Architecture
- [ ] Microservices Fundamentals, Domain-Driven Design (DDD) & Service Boundaries
- [ ] Spring Cloud Gateway, Dynamic Routing, Filters & CORS Configuration
- [ ] Centralized Configuration with Spring Cloud Config Server & Vault
- [ ] Service Discovery & Registry with Netflix Eureka & Spring Cloud
- [ ] Resilience4j Circuit Breaker, Fallbacks, Retry & Bulkhead Patterns
- [ ] Distributed Rate Limiting (Token Bucket & Redis Integration)

### Module 07: Observability & Production Monitoring
- [ ] ELK / OpenSearch Stack (Elasticsearch, Logstash, Kibana, Filebeat)
- [ ] Prometheus Time-Series Metrics Collection, Scrape Targets & PromQL
- [ ] Grafana Dashboards, Visualization & Alert Rules
- [ ] Micrometer Instrumentation, Custom Metrics & Spring Boot Actuator
- [ ] Distributed Tracing with OpenTelemetry, Jaeger & Zipkin

### Module 08: AI Engineering & LLM Application Development
- [ ] Advanced Prompt Engineering (Zero-shot, Few-shot, Chain-of-Thought, ReAct)
- [ ] Structured LLM Outputs (JSON Schema, Pydantic, Instructor, Function Calling)
- [ ] Conversational Memory, Session Management & Token Windowing
- [ ] Tool Calling & LLM External API Integrations
- [ ] Retrieval-Augmented Generation (RAG) Architecture & Chunking Strategies
- [ ] Vector Search, Embeddings & Vector Databases (Milvus, Pinecone, pgvector)
- [ ] Model Context Protocol (MCP) Client-Server Architecture & Standards
- [ ] Autonomous AI Agents, Multi-Step Reasoning & Planning Engines
- [ ] LLM Observability, Evaluations (Evals), Tracing & Cost Tracking (Langfuse/Arize)

### Module 09: AI-Assisted Coding & Developer Agent Workflows
- [ ] Multi-Agent Engineering Teams & Subagent Orchestration
- [ ] Dynamic Context Management & Token Window Conservation
- [ ] Agent Skills & Custom Tool Authoring
- [ ] Plan Mode, Spec-Driven Development & Step-by-Step Verification
- [ ] Autonomous Agent Execution Loops & Self-Correction Loops
- [ ] Token Optimization & Context Compaction Strategies

### Module 10: System Design (HLD & LLD)
- [ ] System Design Fundamentals (CAP Theorem, PACELC, Sharding, Replication, Consistency)
- [ ] Low-Level Design (LLD), Object-Oriented Design Principles & GoF Design Patterns
- [ ] High-Level Design (HLD), Caching, Load Balancing, Database Scaling
- [ ] Real-World Case Studies (YouTube, WhatsApp, Uber, Rate Limiter, Payment Gateway)

### Module 11: Core Computer Science Foundations
- [ ] Data Structures & Algorithms (Arrays, Trees, Graphs, DP, Sliding Window)
- [ ] Operating Systems (Process Scheduling, Threads, Memory Management, Concurrency)
- [ ] Database Management Systems (ACID, Transactions, Indexing B-Trees, Isolation Levels)
- [ ] Computer Networks (OSI Model, TCP/IP, DNS, HTTP/1.1 vs HTTP/2 vs HTTP/3, WebSockets)

### Module 12: Production-Grade Capstone Projects
- [ ] Project 1: High-Throughput Scalable URL Shortener (Redis + Spring Boot + Rate Limiting)
- [ ] Project 2: Cloud-Native Social Media Platform (AWS S3 + Feed Generation + JWT)
- [ ] Project 3: Distributed Quick Commerce Platform (Kafka + Event Sourcing + Microservices)
- [ ] Project 4: Full-Stack AI Coding Assistant (MCP Server + Tool Calling + Context Management)

