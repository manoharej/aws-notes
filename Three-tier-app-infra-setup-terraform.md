# 🚀 Terraform 3-Tier Architecture (AWS)

## 📌 Overview

This project demonstrates a **3-tier architecture** built using Terraform following **industry best practices**.

It includes:

* VPC with public & private subnets
* Security Groups (least privilege model)
* Application Load Balancer (ALB)
* EC2 instances (App Layer)
* RDS (Database Layer - optional with AZ correction)
* Modular Terraform structure
* Environment separation (Dev / UAT ready)

---

# 🧱 Architecture

```
Internet → ALB → EC2 (Private) → RDS (Private)
```

---

# 📁 Project Structure

```
terraform-3tier/
│
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   ├── provider.tf
│   │   ├── backend.tf (optional)
│
├── modules/
│   ├── vpc/
│   ├── security-groups/
│   ├── alb/
│   ├── ec2/
│   ├── rds/
│
├── versions.tf
```

---

# ⚙️ Prerequisites

* AWS Account
* IAM user with required permissions
* Terraform >= 1.5
* AWS CLI configured

```bash
aws configure
```

---

# 🔐 Versioning (Best Practice)

```hcl
terraform {
  required_version = ">= 1.5.0, < 2.0.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

---

# 🌐 VPC Module

## Features

* Custom VPC
* Public & Private subnets
* Internet Gateway

## Key Learning

* Subnets must be distributed across AZs for RDS

---

# 🔐 Security Groups

## Design (Zero Trust Model)

| Layer | Access                 |
| ----- | ---------------------- |
| ALB   | 0.0.0.0/0 (HTTP/HTTPS) |
| APP   | Only from ALB          |
| DB    | Only from APP          |

---

# 🌍 ALB (Application Load Balancer)

## Features

* Public-facing
* Listener on port 80
* Target group routing

---

# 💻 EC2 (Application Layer)

## Features

* Private subnet deployment
* Apache installed via user_data
* Registered with ALB target group

```bash
Hello from dev instance
```

---

# 🗄️ RDS (Database Layer)

## Features

* MySQL database
* Private subnet
* No public access

## ⚠️ Important Learning

RDS requires subnets in **at least 2 Availability Zones**

### ❌ Wrong

```
Both subnets in ap-south-1c
```

### ✅ Correct

```
Subnet 1 → ap-south-1a
Subnet 2 → ap-south-1b
```

---

# 🔑 Secrets Management

## ❌ Avoid

```hcl
db_password = "Admin1234!"
```

## ✅ Use Environment Variables

```bash
export TF_VAR_db_password=Admin1234!
```

## ✅ Mark Sensitive

```hcl
variable "db_password" {
  sensitive = true
}
```

## 🚀 Recommended (Production)

* AWS Secrets Manager
* SSM Parameter Store

---

# 💰 Cost Optimization (Free Tier)

## Free Tier Eligible

* EC2 (t2.micro)
* RDS (db.t3.micro)
* VPC, SG

## Not Free

* ❌ ALB (main cost)

## Recommendation

* Use ALB only for testing
* Destroy resources after use

```bash
terraform destroy
```

---

# 📦 Terraform Commands

```bash
terraform init
terraform plan
terraform apply
```

## With Plan File (Best Practice)

```bash
terraform plan -out=tfplan
terraform apply tfplan
```

---

# 🧠 Key Learnings

* Modular Terraform design
* Variable scoping (module vs environment)
* State management basics
* Security group chaining
* ALB → EC2 traffic flow
* RDS AZ requirement
* Cost awareness
* Secrets management

---

# 🚨 Common Errors & Fixes

## 1. Undeclared Variable

```
Fix: Add variables.tf in environment folder
```

## 2. Wrong Resource Reference

```
aws.this ❌
aws_vpc.this ✅
```

## 3. RDS AZ Error

```
Fix: Use subnets across 2 AZs
```

---

# 🚀 Future Enhancements

* Auto Scaling Group (ASG)
* HTTPS (ACM + ALB)
* Remote backend (S3 + DynamoDB)
* CI/CD pipeline
* Monitoring (CloudWatch)
* Tagging strategy

---

# 💬 Interview Summary

This project demonstrates:

* 3-tier AWS architecture using Terraform
* Secure networking with least privilege
* Modular and reusable infrastructure code
* Real-world debugging and issue resolution
* Cost and security considerations

---

# 🔥 Author

**Manohar E**

DevOps | Terraform | AWS | Cloud Engineering

---
