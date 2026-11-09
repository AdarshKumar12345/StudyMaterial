# Jenkins CI/CD Automation & Pipelines – Important Topics & Learning Resources

## 1. CI/CD Fundamentals
- [ ] Continuous Integration, Delivery and Deployment
- [ ] Pipeline stages: lint, test, build, scan, package, deploy
- [ ] Branching strategies (trunk-based, GitFlow)
- [ ] Artifacts and versioning

## 2. Jenkins Basics
- [ ] Installing Jenkins (Docker / VM)
- [ ] Jenkins architecture: controller and agents
- [ ] Plugins (Git, Pipeline, Docker, Credentials)
- [ ] Freestyle jobs vs Pipeline jobs
- [ ] Global Tool Configuration (JDK, Maven)

## 3. Pipeline as Code
- [ ] Declarative vs Scripted pipelines
- [ ] `Jenkinsfile` structure: `pipeline`, `agent`, `stages`, `steps`, `post`
- [ ] `environment`, `parameters`, `when`
- [ ] Parallel stages
- [ ] Shared Libraries
- [ ] Multibranch pipelines
- [ ] Webhooks (GitHub / GitLab triggers)

## 4. Build, Test & Quality
- [ ] Maven build and unit/integration tests
- [ ] Publishing JUnit reports and coverage (JaCoCo)
- [ ] Static analysis: SonarQube
- [ ] Dependency and image scanning (Trivy, OWASP Dependency-Check)

## 5. Docker & Deployment
- [ ] Docker agents in pipelines
- [ ] Building and pushing images to Docker Hub / ECR
- [ ] Credentials Binding and secrets management
- [ ] Deploying to EC2 (SSH) and to Kubernetes (`kubectl`/Helm)
- [ ] Rollback strategy
- [ ] Notifications (Slack/email)

## 6. Alternatives
- [ ] GitHub Actions basics
- [ ] GitLab CI basics
- [ ] Jenkins vs GitHub Actions: when to use which

## 7. Practical Tasks
- [ ] Jenkinsfile: checkout → test → build jar → build image → push
- [ ] Add a deploy stage to Kubernetes or EC2
- [ ] Trigger the pipeline on every push via webhook
- [ ] Add a SonarQube quality gate

## Important Learning Resources
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Jenkins – Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Jenkins – Using a Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)
- [Baeldung – Jenkins Pipeline](https://www.baeldung.com/ops/jenkins-pipelines)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
