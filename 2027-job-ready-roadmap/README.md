# 🚀 2027 Job-Ready Developer Roadmap & Study Dashboard

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/Study%20Dashboard-Live%20Demo-brightgreen)](https://AdarshKumar12345.github.io/2027-job-ready-roadmap/)
[![Modules](https://img.shields.io/badge/Curriculum-12%20Modules-purple)](#-curriculum-overview)
[![Topics](https://img.shields.io/badge/Specialized%20Topics-67%20Guides-orange)](#-curriculum-overview)
[![Tech Stack](https://img.shields.io/badge/Zero--Build-HTML5%20%7C%20CSS3%20%7C%20ES6%2B-blueviolet)](#-study-dashboard-architecture)

> A production-grade, open-source learning repository and interactive study dashboard designed to prepare software engineers for top-tier full-stack, backend, cloud-native, and AI engineering roles in 2027 and beyond.

---

## 📌 Table of Contents
- [Target Audience](#-who-this-roadmap-is-for)
- [Curriculum Overview](#-curriculum-overview)
- [Interactive Study Dashboard](#-interactive-study-dashboard)
- [7-Month Study Blueprint](#-7-month-study-blueprint)
- [Client-Side Progress Tracking](#-client-side-progress-tracking)
- [Getting Started Locally](#-getting-started-locally)
- [Deployment Instructions (GitHub Pages)](#-deployment-instructions-github-pages)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Who This Roadmap Is For

- **Computer Science Students & Graduates** seeking a structured, rigorous path beyond surface-level bootcamps.
- **Backend & Full-Stack Developers** leveling up to high-throughput Java 21, Spring Boot 3, Microservices, and Event-Driven systems (Kafka/Redis).
- **Engineers Transitioning to Cloud & AI Engineering** looking to master Docker, Kubernetes, AWS, Model Context Protocol (MCP), RAG, and autonomous agent loops.
- **Interview Candidates** preparing for System Design (HLD/LLD), Concurrency, Database Internals, and DSA interviews.

---

## 📚 Curriculum Overview

Every module contains comprehensive, structured guides with prerequisites, core concepts, runnable code examples, hands-on assignments, mini-projects, and interview question banks.

| # | Module | Scope & Core Competencies | Subtopics | Status |
|:---:|:---|:---|:---:|:---:|
| **01** | [**Frontend Development**](01-frontend/) | Semantic HTML5, Modern CSS3, Flexbox/Grid, ES6+ JavaScript, Tailwind CSS, Modern React | 5 Topics | Complete |
| **02** | [**Developer Tooling**](02-developer-tools/) | Git Internals, Branching Strategies, GitHub Workflows, Linux Shell Mastery & Bash Scripting | 2 Topics | Complete |
| **03** | [**Core Java Mastery**](03-core-java/) | Java 21 LTS, JVM Internals, OOP & SOLID, Collections, Multithreading, Virtual Threads, Streams | 8 Topics | Complete |
| **04** | [**Spring Boot Backend**](04-spring-boot-backend/) | Maven, Spring Boot 3, JPA/Hibernate, Spring Security 6, Redis, Elasticsearch, RabbitMQ, Kafka | 9 Topics | Complete |
| **05** | [**Cloud & DevOps**](05-cloud-devops/) | AWS Cloud, Docker Multi-Stage Builds, Kubernetes Orchestration, Jenkins CI/CD, Nginx | 5 Topics | Complete |
| **06** | [**Microservices**](06-microservices/) | Domain-Driven Design, Spring Cloud Gateway, Config Server, Eureka, Resilience4j, Rate Limiting | 6 Topics | Complete |
| **07** | [**Observability**](07-observability/) | ELK / OpenSearch Stack, Prometheus Metrics, Grafana Dashboards, Micrometer, Tracing | 5 Topics | Complete |
| **08** | [**AI Engineering**](08-ai-engineering/) | Prompt Engineering, Structured Outputs, RAG, Vector Search, MCP Protocol, Autonomous Agents | 9 Topics | Complete |
| **09** | [**AI Coding & Agents**](09-ai-coding/) | Multi-Agent Teams, Context Window Management, Custom Skills, Plan-Mode, Token Optimization | 6 Topics | Complete |
| **10** | [**System Design**](10-system-design/) | Distributed Systems Fundamentals, Low-Level Design (LLD), High-Level Design (HLD), Case Studies | 4 Topics | Complete |
| **11** | [**Core CS Foundations**](11-core-cs/) | Data Structures & Algorithms, Operating Systems, DBMS & Indexing, Computer Networks | 4 Topics | Complete |
| **12** | [**Production Capstones**](12-projects/) | URL Shortener, Social Media Platform, Quick Commerce Platform, AI Coding Assistant | 4 Projects | Complete |

---

## 🖥️ Interactive Study Dashboard

The repository includes a companion web dashboard located in the [`website/`](website/) directory:

- **Zero-Build Architecture:** Pure HTML5, modern CSS3 (custom design system), and Vanilla ES6+ JavaScript. No node modules, Webpack, or framework compilation required.
- **GitHub Content Integration:** Fetches and renders Markdown notes directly from repository paths with safe sanitization and syntax highlighting.
- **Full Client-Side Privacy:** Study progress, daily study sessions, completed checkboxes, and customized schedules are persisted securely in your browser's `localStorage`.
- **Theme Switcher:** Seamless Dark and Light modes inspired by Linear and GitHub.
- **Instant Search & Filter:** Filter by module, topic, difficulty, or completion status with keyboard shortcuts (`Cmd/Ctrl + K`).
- **Backup & Restore:** Full JSON export and import capabilities to transfer your progress across devices without requiring third-party accounts.

👉 **Live Study Dashboard URL:** [https://AdarshKumar12345.github.io/2027-job-ready-roadmap/](https://AdarshKumar12345.github.io/2027-job-ready-roadmap/)

---

## 🗓️ 7-Month Study Blueprint

| Month | Core Focus | Parallel Track (Daily 60-90 mins) |
|:---|:---|:---|
| **Month 1** | Frontend Web Foundations, Git/GitHub & Linux Shell | Arrays, Strings, Two Pointers (NeetCode) |
| **Month 2** | Core Java 21, JVM Internals, OOP & Virtual Threads | Linked Lists, Stacks, Queues, Recursion |
| **Month 3** | Spring Boot 3, Hibernate ORM & Spring Security 6 | Binary Search, Trees, BFS & DFS |
| **Month 4** | Redis Caching, Kafka, RabbitMQ, AWS & Docker | Heaps, Priority Queues, Backtracking |
| **Month 5** | Microservices Architecture, Kubernetes & CI/CD | Graphs, Topological Sort, Union-Find |
| **Month 6** | Observability, Generative AI, RAG & MCP Architecture | Dynamic Programming, Greedy Algorithms |
| **Month 7** | System Design (HLD/LLD) & 4 Production Capstones | System Design Mock Interviews & LLD Drills |

Read the full weekly breakdown in [ROADMAP.md](ROADMAP.md).

---

## 💾 Client-Side Progress Tracking

Your study data is saved locally on your device. To ensure you never lose progress:
1. Open the **Study Dashboard**.
2. Navigate to the **Settings** tab.
3. Click **Export Progress (JSON)** to download your snapshot.
4. When switching to another browser or device, click **Import Progress (JSON)**.

Refer to [PROGRESS.md](PROGRESS.md) for full details on the progress tracking schema.

---

## 💻 Getting Started Locally

Because the dashboard uses modern ES Modules and fetches static Markdown/JSON files, run a lightweight local HTTP server rather than opening files directly using `file://`:

### Using Python 3:
```bash
git clone https://github.com/AdarshKumar12345/2027-job-ready-roadmap.git
cd 2027-job-ready-roadmap
python3 -m http.server 8000
```
Open your browser to:
```
http://localhost:8000/website/
```

### Using Node / npx (optional):
```bash
npx serve .
```

---

## 🚢 Deployment Instructions (GitHub Pages)

This repository includes an automated GitHub Actions workflow (`.github/workflows/deploy.yml`) that deploys the static dashboard and bundles all learning Markdown and JSON files to GitHub Pages.

### Step-by-Step Setup:
1. **Push your code to GitHub:**
   ```bash
   git remote add origin https://github.com/AdarshKumar12345/2027-job-ready-roadmap.git
   git branch -M main
   git push -u origin main
   ```
2. **Enable GitHub Actions for Pages:**
   - Go to your repository on GitHub.
   - Navigate to **Settings** > **Pages**.
   - Under **Build and deployment** > **Source**, select **GitHub Actions**.
3. **Trigger Deployment:**
   - The workflow triggers automatically on push to `main`.
   - Monitor progress in the **Actions** tab.
4. **Access the Live Dashboard:**
   - Once completed, visit `https://AdarshKumar12345.github.io/2027-job-ready-roadmap/`.

---

## 🤝 Contributing

Contributions are warmly welcomed! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for documentation conventions, topic structure requirements, and pull request procedures.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
You are free to use, adapt, and share this curriculum for personal study, mentoring, or technical training.

