# Kubernetes & Helm – Important Topics & Learning Resources

## 1. Kubernetes Basics
- [ ] Why orchestration (scaling, self-healing, rollouts)
- [ ] Architecture: control plane (API server, etcd, scheduler, controller manager)
- [ ] Worker node: kubelet, kube-proxy, container runtime
- [ ] Local clusters: minikube, kind, Docker Desktop
- [ ] `kubectl` basics and contexts
- [ ] Declarative YAML and `kubectl apply`

## 2. Core Workloads
- [ ] Pods and the pod lifecycle
- [ ] Labels, selectors and annotations
- [ ] ReplicaSets
- [ ] Deployments, rolling updates and rollbacks
- [ ] Namespaces
- [ ] Jobs and CronJobs
- [ ] StatefulSets and DaemonSets

## 3. Networking
- [ ] Services: ClusterIP, NodePort, LoadBalancer
- [ ] Cluster DNS and service discovery
- [ ] Ingress and Ingress Controllers (NGINX)
- [ ] NetworkPolicies
- [ ] TLS with cert-manager

## 4. Configuration & Storage
- [ ] ConfigMaps
- [ ] Secrets (and their limits)
- [ ] Environment variables vs mounted files
- [ ] Volumes, PersistentVolumes and PersistentVolumeClaims
- [ ] StorageClasses

## 5. Reliability & Scaling
- [ ] Resource requests and limits
- [ ] Liveness, readiness and startup probes (Spring Boot Actuator health groups)
- [ ] Horizontal Pod Autoscaler
- [ ] Pod Disruption Budgets
- [ ] Node affinity, taints and tolerations
- [ ] Graceful shutdown with Spring Boot

## 6. Helm & Ecosystem
- [ ] Helm charts, values and templates
- [ ] `helm install/upgrade/rollback`
- [ ] Chart repositories
- [ ] RBAC and ServiceAccounts
- [ ] Kustomize (overview)
- [ ] Monitoring: Prometheus + Grafana
- [ ] Managed Kubernetes: EKS (overview)
- [ ] GitOps basics (Argo CD)

## 7. Debugging
- [ ] `kubectl logs`, `describe`, `exec`, `port-forward`
- [ ] CrashLoopBackOff, ImagePullBackOff, Pending pods
- [ ] Reading events

## 8. Practical Tasks
- [ ] Deploy a Spring Boot app + PostgreSQL on minikube/kind
- [ ] Expose it with Ingress and configure via ConfigMap/Secret
- [ ] Add probes and an HPA, then load test
- [ ] Package the app as a Helm chart

## Important Learning Resources
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Kubernetes Basics Tutorial](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [Helm Documentation](https://helm.sh/docs/)
- [Spring Boot – Kubernetes Probes](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html#actuator.endpoints.kubernetes-probes)
- [KodeKloud – Free Kubernetes Resources](https://kodekloud.com/)
- [Kubernetes by Example](https://kubernetesbyexample.com/)
