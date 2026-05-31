# AWS Lambda Hands-On Practice README

## Overview

This repository contains hands-on AWS Lambda practice exercises focused on:

* AWS Lambda automation
* IAM permissions
* S3 event-driven workflows
* Lambda Layers
* boto3 automation
* VPC networking concepts
* RDS integration basics
* CloudWatch monitoring

The goal is to learn real-world serverless and cloud automation practices used in enterprise environments.

---

# Topics Practiced

## 1. Basic Lambda Function

### Covered

* Lambda creation
* Python runtime
* IAM execution role
* CloudWatch logging
* Environment variables
* JSON response handling

### Key Learnings

* Lambda is stateless
* Execution role importance
* Logging and monitoring basics

---

# 2. EC2 Automation using boto3

## Activities

* List EC2 instances
* IAM permission troubleshooting
* boto3 SDK usage

### Important IAM Permission

```text
AmazonEC2ReadOnlyAccess
```

### Key Learnings

* AWS SDK automation
* Describe APIs
* IAM access debugging

---

# 3. S3 Automation using Lambda

## Activities

* Create source bucket
* Create target bucket
* List S3 buckets
* List objects in bucket
* Copy object between buckets
* Automatic S3 event trigger

### Workflow

```text
S3 Upload
    ↓
Lambda Trigger
    ↓
Copy Object
    ↓
Target Bucket
```

### Key Learnings

* Event-driven architecture
* S3 notifications
* CloudWatch troubleshooting
* Object copy automation

---

# 4. Lambda Layers

## Activities

* Create custom Lambda layer
* Package requests library
* Attach layer to Lambda
* Import external dependency

### Layer Structure

```text
python/
    requests/
```

### Key Learnings

* Shared dependency management
* Runtime compatibility
* Layer packaging structure

---

# 5. RDS Describe API Practice

## Activities

* Retrieve RDS instance details
* Understand control plane APIs
* Practice boto3 RDS automation

### IAM Permission

```text
AmazonRDSReadOnlyAccess
```

### Key Learnings

* describe_db_instances()
* AWS control plane APIs
* Difference between metadata access and DB connection

---

# 6. Lambda with VPC Concepts

## Activities

* Attach Lambda to VPC
* Understand private subnet behavior
* VPC execution role troubleshooting

### Required IAM Policy

```text
AWSLambdaVPCAccessExecutionRole
```

### Key Learnings

* ENI creation
* Private subnet behavior
* NAT Gateway requirement
* Internet access limitations

---

# Important AWS Networking Concepts Learned

## Lambda Outside VPC

* Internet available by default

## Lambda Inside VPC

* Internet access removed
* Requires NAT Gateway or VPC Endpoint

## Public Subnet Myth

Lambda does NOT automatically get public IP even in public subnet.

---

# CloudWatch Monitoring Practice

## Covered

* Lambda logs
* Timeout troubleshooting
* Event debugging
* Runtime errors
* Invocation tracing

---

# Common Issues Faced and Fixed

| Issue                   | Root Cause             | Fix                    |
| ----------------------- | ---------------------- | ---------------------- |
| UnauthorizedOperation   | Missing IAM permission | Attach required policy |
| S3 trigger not invoking | Incorrect event config | Recreate trigger       |
| 'Records' error         | Invalid test event     | Use proper S3 event    |
| Lambda timeout in VPC   | No internet/NAT        | Remove VPC or add NAT  |
| Layer import failure    | Wrong ZIP structure    | Add python/ folder     |

---

# AWS Services Practiced

* AWS Lambda
* IAM
* S3
* CloudWatch
* RDS
* VPC
* Lambda Layers

---

# boto3 APIs Practiced

## EC2

```python
describe_instances()
```

## S3

```python
list_buckets()
list_objects_v2()
copy_object()
```

## RDS

```python
describe_db_instances()
```

---

# Real Industry Concepts Covered

* Event-driven serverless architecture
* Least privilege IAM
* Serverless automation
* AWS SDK automation
* Logging and observability
* Private networking
* Shared dependency layers
* Cloud automation troubleshooting

---

# Next Topics Planned

## Immediate Next Topics

* Parameter Store integration
* Secrets Manager
* Lambda inside VPC
* Lambda to RDS connectivity
* NAT Gateway setup
* VPC endpoints
* Security group troubleshooting

---

# Future Advanced Topics

* Step Functions
* EventBridge scheduling
* DynamoDB integration
* API Gateway integration
* Terraform deployment
* AWS SAM
* CI/CD for Lambda
* Multi-environment deployment
* Cross-account automation

---

# Best Practices Learned

## Lambda

* Keep functions small
* Use structured logging
* Avoid hardcoded configs
* Use layers for shared dependencies

## IAM

* Apply least privilege
* Separate execution roles
* Avoid excessive permissions

## S3

* Use event notifications carefully
* Avoid recursive triggers
* Validate object types

## Networking

* Use private subnets for databases
* Understand NAT requirements
* Use VPC endpoints where possible

---

# Final Outcome

After completing these practices, the learner gained practical experience in:

* AWS serverless automation
* Lambda event processing
* boto3 scripting
* AWS networking concepts
* IAM troubleshooting
* Enterprise-style debugging
* Event-driven cloud architectures

This practice setup closely resembles real-world cloud automation and serverless engineering workflows used in production AWS environments.
