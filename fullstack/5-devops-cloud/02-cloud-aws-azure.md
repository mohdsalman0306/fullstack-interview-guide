# Cloud: AWS and Azure

> **Time needed:** about 1 hour · **Questions:** 8 + 6 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

You have used **both** AWS (SNS/SQS) and Azure (Service Bus). Say that in the interview. Being comfortable with more than one cloud is a plus.

## Progress checklist
- [ ] 1. IaaS vs PaaS vs SaaS vs serverless
- [ ] 2. Serverless and cold starts
- [ ] 3. Object storage and signed URLs
- [ ] 4. IAM and least privilege
- [ ] 5. High availability and disaster recovery
- [ ] 6. AWS and Azure equivalents (cheat sheet)
- [ ] 7. Cost awareness
- [ ] 8. Observability in the cloud

---

## 1. What's the difference between IaaS, PaaS, SaaS and serverless?

**In simple words**
It's about how much you manage yourself and how much the cloud manages for you. The more the cloud manages, the less work you do, but the less control you have.

**Picture it**
IaaS is renting an empty kitchen. PaaS is a kitchen with a chef who cooks your recipe. Serverless is a chef who only comes in when an order arrives, and you pay per dish. SaaS is just ordering food from a restaurant.

**🗣️ Say this in the interview**
> "It's about **how much you manage**. With **IaaS** you rent virtual machines and manage the OS and everything above it, like EC2 or Azure VMs. With **PaaS** you deploy your app and the platform runs it, like App Service or Elastic Beanstalk, and managed Kubernetes partly. With **serverless**, or FaaS, you deploy functions that run on demand, scale to zero, and you pay per run, like Lambda or Azure Functions. With **SaaS** you just use the software, like Shopify or Salesforce.
> Going up the list means less operations work, but less control."

**⚠️ Avoid saying**
"Serverless means there are no servers." There are servers; you just don't manage them.

---

## 2. When would you use serverless? What is the cold start problem?

**In simple words**
Serverless is great for work that comes in bursts or not very often. The main downside is the **cold start**: after it's been idle, the first request is slower because the platform has to start the function first.

**🗣️ Say this in the interview**
> "Serverless is great for **event-driven, spiky or low-traffic** work: processing an uploaded image, a webhook, a scheduled job, a queue consumer. You pay only when it runs, and scaling is automatic.
> The downsides: **cold starts**, where the first request after idle is slower while the runtime starts. **Time limits** on each run. Harder **local debugging**. And database connection problems: thousands of functions running at once can use up all the DB connections. That's why tools like RDS Proxy exist. For a steady, high-traffic API, containers are usually cheaper and more predictable."

**⚠️ Avoid saying**
"Serverless is always cheaper." For steady high traffic, containers usually cost less.

---

## 3. How do you handle file uploads and storage in the cloud?

**In simple words**
Files go to **object storage** (S3 or Azure Blob). Your API doesn't carry the file. It gives the browser a short-lived **signed URL**, and the browser uploads straight to storage.

**Picture it**
A signed URL is a one-time visitor pass. It opens one door, for a short time, and then stops working.

**🗣️ Say this in the interview**
> "Files like product images, invoices and exports go to **object storage**: S3 or Azure Blob Storage. It's cheap, durable and almost unlimited. Our API server doesn't stream big uploads itself. It creates a **short-lived signed URL**, an S3 presigned URL or an Azure SAS token, and the browser **uploads directly** to storage. Downloads work the same way, or go through a **CDN**. Buckets stay **private** by default."

**⚠️ Avoid saying**
"I make the bucket public so users can upload." Keep it private and use signed URLs.

---

## 4. What is IAM and least privilege?

**In simple words**
**IAM** (identity and access management) controls who can do what in the cloud. **Least privilege** means each service gets only the permissions it really needs, nothing more.

**Picture it**
A hotel key card that opens only your room and the gym, not every room in the building.

**🗣️ Say this in the interview**
> "Every service gets an **identity** with **only the permissions it needs**. The order service can write to the orders queue, but it can't delete the bucket. On AWS that's an **IAM role** attached to the workload. On Azure it's a **Managed Identity**. The big win: **no access keys stored anywhere**. The platform gives short-lived credentials automatically. Long-lived keys in environment variables are the most common way cloud accounts get hacked."

**⚠️ Avoid saying**
"I put the access key in an environment variable." Use roles or Managed Identity.

---

## 5. How do you design for high availability and disaster recovery?

**In simple words**
A **region** is an area of the world (like "West Europe"). An **availability zone (AZ)** is a separate data centre inside that region. Spread across AZs to survive one data centre failing. Copy to another region to survive a whole region failing.

**🗣️ Say this in the interview**
> "A **region** is a geographic area; an **availability zone** is a separate data centre inside it. For **high availability**, I run across **multiple AZs**: load balancer, app instances and a multi-AZ database. So one data centre failing doesn't take us down. **Disaster recovery** covers losing a whole region, with backups or replicas in another region.
> Two numbers drive the design. **RPO**: how much data can we afford to lose? **RTO**: how long can we be down? Lower numbers cost more, so it's a business decision, not just a technical one."

**They may also ask**
- *How to remember RPO vs RTO?* RP**O** = data, "how far back do we go". RT**O** = time, "how long until we're back".

**⚠️ Avoid saying**
"We need zero downtime and zero data loss." Ask the business what RPO and RTO they can pay for.

---

## 6. Do you know the AWS and Azure equivalents?

**In simple words**
Most services have a twin on the other cloud. Knowing the pairs shows you can move between clouds easily.

**🗣️ Say this in the interview**
> "I've used both. The ideas are the same, only the names change. For example, SQS is like Service Bus queues, SNS is like Service Bus topics, S3 is Blob Storage, Lambda is Azure Functions, and IAM roles are like Managed Identity."

| Need | AWS | Azure |
|---|---|---|
| Virtual machines | EC2 | Virtual Machines |
| Containers (managed K8s) | EKS | AKS |
| Simple containers | ECS / Fargate, App Runner | Container Apps |
| Serverless functions | Lambda | Functions |
| Object storage | S3 | Blob Storage |
| Relational DB | RDS / Aurora | Azure SQL, Database for PostgreSQL/MySQL |
| NoSQL | DynamoDB | Cosmos DB |
| Cache | ElastiCache | Azure Cache for Redis |
| Queue | SQS | Service Bus queues / Storage Queues |
| Pub/sub | SNS, EventBridge | Service Bus topics, Event Grid |
| Streaming | Kinesis, MSK | Event Hubs |
| API gateway | API Gateway | API Management |
| CDN / edge | CloudFront | Front Door / CDN |
| Secrets | Secrets Manager, Parameter Store | Key Vault |
| Identity for workloads | IAM roles | Managed Identity |
| Monitoring | CloudWatch, X-Ray | Azure Monitor, Application Insights |
| Infrastructure as Code | CloudFormation, CDK | ARM, Bicep |

**From my work**
Mention your SNS/SQS and Service Bus work here. Use **Story S12** — see `1-interview-basics/02-your-story.md`.

---

## 7. How do you keep cloud costs under control?

**In simple words**
Senior engineers are expected to think about cost. Most savings come from not paying for things you don't use.

**🗣️ Say this in the interview**
> "The usual big savings: **right-sizing**, because most instances are bigger than needed. **Autoscaling** down at night. **Reserved instances or savings plans** for steady load. **Spot instances** for work that can be interrupted. **Storage lifecycle rules** that move old files to cheaper tiers. **Caching and a CDN** to cut compute and egress. And watching **data transfer costs** between regions and AZs, which surprise everyone. I also **tag resources**, so we know which team or feature costs what."

**⚠️ Avoid saying**
"Cost is the finance team's job." Seniors own the cost of what they build.

---

## 8. How do you do observability in the cloud?

**In simple words**
**Observability** means you can see what your system is doing in production. You need logs, metrics and traces, all linked by one ID, and alerts on what users actually feel.

**🗣️ Say this in the interview**
> "I want the three pillars in one place: **logs**, **metrics** and **traces**, all linked by a **trace or correlation ID**. That's CloudWatch and X-Ray on AWS, Azure Monitor and Application Insights on Azure, or a vendor like Datadog or New Relic. Ideally it's fed by **OpenTelemetry**, an open standard, so we're not locked in. Then I **alert on symptoms users feel**, like error rate, p99 latency and queue age, not on every CPU spike. And I keep a dashboard per service."

**They may also ask**
- *What is p99 latency?* 99% of requests are faster than this number. It shows the slow experience that averages hide.

**⚠️ Avoid saying**
"We alert on high CPU." Alert on what users feel: errors, latency, queue age.

---

## Quick questions
- **Multi-AZ vs multi-region?** Multi-AZ survives one data centre failing (the standard). Multi-region survives a whole region going down (expensive and complex).
- **Vertical vs horizontal scaling?** A bigger instance vs more instances behind a load balancer.
- **VPC / VNet?** Your private network in the cloud. Databases go in private subnets with no public IP.
- **Security groups / NSGs?** Firewall rules: which ports and sources can reach a resource.
- **What is egress cost?** Paying for data **leaving** the cloud, or crossing regions or AZs.
- **Shared responsibility model?** The provider secures the cloud itself. **You** secure what you put in it: config, IAM, data and code.

## Remember these 10 things
1. IaaS → PaaS → serverless → SaaS: less work for you, less control.
2. Serverless fits spiky, event-driven work. Watch cold starts, time limits and DB connections.
3. Steady high-traffic APIs are usually cheaper on containers.
4. Files go to S3 / Blob. Browser uploads directly with a short-lived signed URL. Buckets stay private.
5. Least privilege: IAM roles (AWS) or Managed Identity (Azure). No stored access keys.
6. Run across multiple AZs for high availability; another region for disaster recovery.
7. RPO = how much data you can lose. RTO = how long you can be down. It's a business decision.
8. Know the twins: SQS ↔ Service Bus queues, SNS ↔ Service Bus topics, S3 ↔ Blob, Lambda ↔ Functions.
9. Cost: right-size, autoscale, reserved/spot, lifecycle rules, CDN, watch data transfer, tag everything.
10. Observability: logs + metrics + traces linked by one ID, OpenTelemetry, alert on user symptoms.
