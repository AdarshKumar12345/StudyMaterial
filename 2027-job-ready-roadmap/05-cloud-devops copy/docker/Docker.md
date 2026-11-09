# Docker & Docker Compose – Important Topics & Learning Resources

## 1. Container Basics
- [ ] What is containerization and why use it
- [ ] Containers vs Virtual Machines
- [ ] Docker architecture (client, daemon, registry)
- [ ] Namespaces and cgroups (overview)
- [ ] Installing Docker and `docker run hello-world`

## 2. Images & Containers
- [ ] Image layers and union filesystem
- [ ] `docker pull`, `run`, `ps`, `stop`, `rm`, `logs`, `exec`
- [ ] Port mapping (`-p`) and environment variables (`-e`)
- [ ] Docker Hub and other registries (ECR, GHCR)
- [ ] Tagging and versioning images
- [ ] Image cleanup (`docker system prune`)

## 3. Dockerfile
- [ ] `FROM`, `WORKDIR`, `COPY`, `ADD`, `RUN`
- [ ] `CMD` vs `ENTRYPOINT`
- [ ] `ENV`, `ARG`, `EXPOSE`
- [ ] `.dockerignore`
- [ ] Layer caching and instruction ordering
- [ ] Multi-stage builds (Spring Boot + Node.js)
- [ ] Slim/distroless/alpine base images
- [ ] Running as a non-root user
- [ ] Spring Boot layered jars / buildpacks (`spring-boot:build-image`)

## 4. Storage & Networking
- [ ] Volumes vs bind mounts vs tmpfs
- [ ] Bridge, host and none networks
- [ ] User-defined networks and DNS between containers
- [ ] Container-to-container communication

## 5. Docker Compose
- [ ] `docker-compose.yml` structure (services, networks, volumes)
- [ ] `depends_on` and health checks
- [ ] `.env` files
- [ ] Multi-service stack: app + PostgreSQL + Redis + Kafka
- [ ] `docker compose up/down/logs/build`
- [ ] Compose profiles and override files

## 6. Security & Best Practices
- [ ] Image scanning (Trivy, Docker Scout)
- [ ] Secrets handling (never bake into images)
- [ ] Resource limits (CPU / memory)
- [ ] Small, reproducible, pinned images

## 7. Practical Tasks
- [ ] Containerize a Spring Boot app with a multi-stage Dockerfile
- [ ] Compose stack: API + PostgreSQL + Redis
- [ ] Reduce an image from 700MB+ to under 200MB
- [ ] Push an image to Docker Hub / ECR

## Important Learning Resources
- [Docker Official Documentation](https://docs.docker.com/)
- [Docker – Get Started](https://docs.docker.com/get-started/)
- [Dockerfile Best Practices](https://docs.docker.com/build/building/best-practices/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Spring Boot – Container Images](https://docs.spring.io/spring-boot/reference/packaging/container-images/index.html)
- [Play with Docker](https://labs.play-with-docker.com/)
