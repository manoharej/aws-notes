# aws-notes
All the notes for AWS Cloud
# 📘 AWS & DevOps Basics – Q&A Notes

---

## 1. What is Infrastructure?

**Answer:**
Infrastructure is the collection of hardware, software, networking, and services required to run applications.

👉 Examples:

* EC2 (Compute)
* S3 (Storage)
* VPC (Networking)

---

## 2. What is an Application?

**Answer:**
An application is a software program designed to perform specific tasks for users.

👉 Examples:

* Web apps (Amazon)
* Mobile apps (WhatsApp)
* APIs (backend services)

---

## 3. What is an Operating System (OS)?

**Answer:**
An Operating System is system software that acts as a bridge between user and hardware.

👉 Responsibilities:

* CPU management
* Memory management
* File system handling
* Process management

👉 Examples:

* Linux
* Windows

---

## 4. What is Cloud Computing?

**Answer:**
Cloud computing is the delivery of computing services over the internet with pay-as-you-go pricing.

👉 Benefits:

* Scalability
* Cost-effective
* High availability
* Flexibility

---

## 5. What is a Data Center?

**Answer:**
A Data Center is a physical facility that contains servers, storage systems, and networking equipment.

👉 In AWS:

* Data centers are grouped into Availability Zones 

---

## 6. What is On-Premises Data Center?

**Answer:**
An On-Premises Data Center is a physical infrastructure owned and managed by an organization within its own location.

👉 Characteristics:

* Full control over hardware
* High setup and maintenance cost
* Requires dedicated IT team

---

## 7. What are Cloud Services?

**Answer:**
Cloud services are on-demand IT resources provided over the internet.

👉 Examples:

* Compute (EC2)
* Storage (S3)
* Database (RDS)
* Networking (VPC)

---

## 8. What is Shared Responsibility Model in Cloud?

**Answer:**
It defines the security responsibilities between AWS and the customer.

👉 AWS Responsibility:

* Hardware
* Data centers
* Networking infrastructure

👉 Customer Responsibility:

* OS configuration
* Applications
* Data security
* IAM permissions

---

## 9. What are Cloud Service Models?

**Answer:**
Cloud service models define how much control you have over infrastructure and services.

👉 Types:

* IaaS
* PaaS
* SaaS

---

## 10. What is IaaS (Infrastructure as a Service)?

**Answer:**
Provides basic infrastructure like servers, storage, and networking.

👉 You manage:

* OS
* Applications

👉 AWS manages:

* Hardware

👉 Example:

* EC2

---

## 11. What is PaaS (Platform as a Service)?

**Answer:**
Provides a platform to develop and deploy applications without managing infrastructure.

👉 You manage:

* Application

👉 AWS manages:

* OS
* Infrastructure

👉 Example:

* Elastic Beanstalk

---

## 12. What is SaaS (Software as a Service)?

**Answer:**
Provides complete software applications over the internet.

👉 You just use the application.

👉 Examples:

* Gmail
* Zoom

---

## 13. What are Cloud Deployment Models?

**Answer:**
Deployment models define how cloud infrastructure is set up and accessed.

👉 Types:

* Public Cloud
* Private Cloud
* Hybrid Cloud

---

## 14. What is Public Cloud?

**Answer:**
Cloud infrastructure provided by third-party providers and shared across multiple users.

👉 Example:

* AWS

---

## 15. What is Private Cloud?

**Answer:**
Cloud infrastructure used exclusively by a single organization.

👉 Can be:

* On-premises
* Hosted privately

---

## 16. What is Hybrid Cloud?

**Answer:**
Combination of on-premises infrastructure and public cloud.

👉 Example:

* Some apps in data center
* Some apps in AWS

---

## 🔗 Concept Flow

```
Data Center → Infrastructure → OS → Application → Cloud
```

---
# 📘 AWS & DevOps Basics – Q&A Notes (Extended)

---

## 🔹 AWS Basics

### 17. What is AWS?

**Answer:**
AWS (Amazon Web Services) is a cloud computing platform that provides on-demand IT services like compute, storage, networking, databases, etc.

---

### 18. What is AWS Global Infrastructure?

**Answer:**
AWS Global Infrastructure is the worldwide network of data centers, regions, availability zones, and edge locations designed for high availability and performance. 

---

### 19. What is AWS Region?

**Answer:**
A Region is a geographically isolated area that contains multiple Availability Zones.

👉 Example:

* ap-south-1 (Mumbai)

---

### 20. What is AWS Availability Zone (AZ)?

**Answer:**
An Availability Zone is one or more data centers within a region with independent power, networking, and cooling. 

---

## 🔹 EC2 & Compute

### 21. What is AWS EC2?

**Answer:**
EC2 (Elastic Compute Cloud) is a service that provides virtual servers in the cloud.

👉 Used to run applications.

---

### 22. What is AWS Free Tier?

**Answer:**
AWS Free Tier provides limited free usage of AWS services for 12 months or always-free services.

---

### 23. What is AWS Pricing Calculator?

**Answer:**
A tool used to estimate the cost of AWS services before deploying resources.

---

### 24. How to create AWS EC2 instance?

**Answer (Steps):**

1. Go to EC2 Dashboard
2. Click Launch Instance
3. Choose AMI
4. Select Instance Type
5. Configure details
6. Add storage
7. Configure security group
8. Create/select key pair
9. Launch instance

---

### 25. What is AMI (Amazon Machine Image)?

**Answer:**
AMI is a pre-configured template used to launch EC2 instances (includes OS, software, configurations).

---

### 26. What are AWS Instance Types and how to choose?

**Answer:**
Instance types define CPU, memory, storage, and networking capacity.

👉 Types:

* General purpose (t2, t3)
* Compute optimized (c5)
* Memory optimized (r5)

👉 Choose based on:

* Application requirement (CPU/memory)

---

### 27. What is AWS EBS Volume?

**Answer:**
EBS (Elastic Block Store) is block storage used with EC2 instances.

👉 Acts like a hard disk.

---

### 28. What is AWS Key Pair?

**Answer:**
A Key Pair is used to securely connect to EC2 instances.

👉 Components:

* Public key (stored in AWS)
* Private key (.pem file)

---

### 29. How to change EC2 Instance Type?

**Answer:**

1. Stop the instance
2. Go to Actions → Instance Settings
3. Change Instance Type
4. Start the instance

---

### 30. How to explore AWS Billing Dashboard?

**Answer:**

* Go to AWS Console → Billing
* View:

  * Usage
  * Cost breakdown
  * Bills
  * Budgets

---

## 🔹 Security & Networking

### 31. What is Security Group in AWS?

**Answer:**
A Security Group acts as a virtual firewall controlling inbound and outbound traffic for EC2.

---

### 32. Why Security Groups are Stateful?

**Answer:**
Because if inbound traffic is allowed, the response traffic is automatically allowed — no need to define outbound rules separately.

---

## 🔹 Serverless & Monitoring

### 33. What is AWS Lambda?

**Answer:**
AWS Lambda is a serverless service that allows you to run code without managing servers.

---

### 34. How to Configure CloudWatch Events?

**Answer:**

1. Go to CloudWatch
2. Create Rule
3. Select event source (schedule or service)
4. Add target (Lambda/SNS)
5. Enable rule

---

### 35. What is CloudWatch Logs Group?

**Answer:**
A Log Group is a collection of log streams used to store logs from AWS services like EC2, Lambda.

---

## 🔹 S3 (Storage)

### 36. How to setup S3 Lifecycle Rules?

**Answer:**

1. Go to S3 bucket
2. Open Management tab
3. Create Lifecycle Rule
4. Define transition (e.g., Standard → Glacier)
5. Set expiration

---

### 37. How to enable S3 Versioning?

**Answer:**

1. Go to S3 bucket
2. Open Properties tab
3. Enable Versioning

👉 Keeps multiple versions of objects for recovery

---

## 🎯 Final Tip

👉 These questions are **top AWS interview questions**
👉 Practice explaining each in **2–3 lines clearly**

---

## 🚀 Real-Time Example

Deploying a Web Application:

* EC2 → App hosting
* RDS → Database
* S3 → Storage
* VPC → Networking

---

## 📌 Quick Summary

| Concept        | Meaning                        |
| -------------- | ------------------------------ |
| Infrastructure | Foundation resources           |
| Application    | Software used by users         |
| OS             | Bridge between user & hardware |
| Cloud          | Services over internet         |
| Data Center    | Physical location              |
| Cloud Services | On-demand resources            |

---

## 🎯 Final Tip

👉 These are **must-know basics for AWS interviews**.
If you’re clear here, advanced topics become easy.

---
