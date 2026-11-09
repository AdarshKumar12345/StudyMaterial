# AWS Core Services – Important Topics & Learning Resources

## 1. Cloud Fundamentals
- [ ] Cloud computing models (IaaS, PaaS, SaaS)
- [ ] Regions, Availability Zones and Edge locations
- [ ] Shared Responsibility Model
- [ ] AWS Free Tier, billing alerts and Budgets
- [ ] AWS Console, CLI and SDKs

## 2. IAM (Identity & Access Management)
- [ ] Users, Groups, Roles and Policies
- [ ] JSON policy structure (Effect, Action, Resource)
- [ ] Principle of least privilege
- [ ] Root account protection and MFA
- [ ] IAM roles for EC2 / ECS / EKS (no hard-coded keys)
- [ ] AWS STS and temporary credentials

## 3. Networking (VPC)
- [ ] VPC, CIDR blocks and subnets (public vs private)
- [ ] Internet Gateway and NAT Gateway
- [ ] Route tables
- [ ] Security Groups vs Network ACLs
- [ ] Load Balancers (ALB / NLB)
- [ ] Route 53 (DNS) and ACM (certificates)

## 4. Compute
- [ ] EC2 instance types, AMIs, key pairs
- [ ] User data scripts
- [ ] Auto Scaling Groups
- [ ] Elastic Beanstalk (overview)
- [ ] ECS / Fargate (overview)
- [ ] EKS (overview)
- [ ] Lambda and API Gateway (overview)

## 5. Storage & Databases
- [ ] S3: buckets, objects, storage classes, versioning
- [ ] S3 pre-signed URLs and bucket policies
- [ ] EBS and EFS
- [ ] RDS (PostgreSQL/MySQL), Multi-AZ, read replicas, backups
- [ ] DynamoDB basics
- [ ] ElastiCache (Redis)

## 6. Messaging, Monitoring & Integration
- [ ] SQS, SNS
- [ ] MSK (managed Kafka) – overview
- [ ] CloudWatch logs, metrics and alarms
- [ ] CloudTrail
- [ ] Secrets Manager / Parameter Store
- [ ] AWS SDK for Java v2 with Spring Boot (`S3Client`)
- [ ] Spring Cloud AWS

## 7. Practical Tasks
- [ ] Launch an EC2 instance and deploy a Dockerized Spring Boot app
- [ ] Upload files from Spring Boot to S3 using pre-signed URLs
- [ ] Put the app behind an ALB in a private subnet with RDS
- [ ] Create a CloudWatch alarm on CPU usage

## Important Learning Resources
- [AWS Documentation](https://docs.aws.amazon.com/)
- [AWS Skill Builder (free tier)](https://skillbuilder.aws/)
- [AWS Cloud Practitioner Essentials](https://aws.amazon.com/training/digital/aws-cloud-practitioner-essentials/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS SDK for Java v2 Guide](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/home.html)
- [Spring Cloud AWS](https://docs.awspring.io/spring-cloud-aws/docs/current/reference/html/index.html)
