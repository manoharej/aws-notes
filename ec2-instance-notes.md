Amazon EC2 (Elastic Compute Cloud) – Practice & Theoretical Guide

Amazon EC2 is a core AWS service used to launch and manage virtual servers in the cloud.
It allows organizations to run applications without managing physical hardware.

1. What is EC2?

EC2 (Elastic Compute Cloud) provides virtual machines in AWS.

Common names across cloud platforms:

Cloud Provider	Service Name
AWS	EC2 Instance
Microsoft Azure	Virtual Machine (VM)
Google Cloud	Compute Engine
General Term	Virtual Machine / Server

An EC2 instance behaves like a normal server where we can:

Install applications
Host websites
Run databases
Configure networking
Manage users and storage
2. EC2 Pricing Models

AWS provides multiple pricing options based on workload requirements.

2.1 On-Demand Instances
Description

Pay only for the resources you use without long-term commitment.

Best For
Development and testing
Temporary workloads
New applications
Unpredictable traffic
Advantages
No upfront payment
Flexible
Easy to launch and terminate
Disadvantages
Highest cost per hour
Billing

AWS charges per second with a minimum of 60 seconds.

2.2 Reserved Instances (RI)
Description

Commit to using EC2 for 1 or 3 years and receive significant discounts.

Best For
Stable workloads
Production applications
Always-running systems
Types of Reserved Instances
Standard Reserved Instance
Maximum discount
Fixed instance family/type and region
Less flexibility
Convertible Reserved Instance
Can change instance family or size
More flexibility
Slightly lower discount
Payment Options
Option	Description
Full Upfront	Pay entire amount in advance
Partial Upfront	Pay part now and remaining hourly
No Upfront	No advance payment
2.3 Savings Plans
Description

Flexible pricing model where you commit to a specific usage amount ($/hour).

Types
Compute Savings Plan

Applies to:

EC2
Lambda
Fargate

Works across:

Regions
Instance families
Operating systems
EC2 Instance Savings Plan

Limited to:

Specific EC2 family
Specific region
Database Savings Plan

Applies to:

RDS
Aurora
DynamoDB
ElastiCache
OpenSearch
SageMaker Savings Plan

Applies to Amazon SageMaker workloads.

2.4 Spot Instances
Description

Use AWS spare capacity at very low cost.

Advantages
Huge discounts
Disadvantages
AWS can terminate the instance anytime with 2-minute warning
Best For
Batch jobs
Testing
Fault-tolerant applications
CI/CD jobs
3. Understanding EC2 Instance Types

Example:

t3.micro
Part	Meaning
t	Instance family
3	Generation
micro	Size
Processor Suffixes
Suffix	Processor
i	Intel
a	AMD
g	AWS Graviton ARM
4. EC2 Instance Families
Family	Purpose
T / M	General Purpose
C	Compute Optimized
R / X / U / Z	Memory Optimized
I / D / H	Storage Optimized
P / G / Trn	GPU / Accelerated Computing
HPC	High Performance Computing
5. Environment Sizing Standards
Development / QA

Lower-cost instances:

t3.medium
t4g.medium
t4g.large
UAT / Production

Production-like sizing:

m5.large
m5.xlarge
m6i.large
r5.xlarge
r5.2xlarge
6. EC2 Launch Process
Step 1 – Add Tags

Tags help identify and manage resources.

Common Tags
Tag	Example
Name	app-server-01
Project	BankingApp
ClientID	Client001
CostCenter	Finance
Step 2 – Choose AMI

AMI = Amazon Machine Image

Contains:

Operating system
Preinstalled software
Configuration

Examples:

Amazon Linux
Ubuntu
Windows Server
Step 3 – Choose Instance Type

Example:

t3.micro
c7i-flex.large
Step 4 – Configure Key Pair
Key Pair Components
Component	Purpose
Public Key	Stored in EC2
Private Key	Stored on your laptop
Usage
Linux

Private key used directly for SSH login.

Windows

Private key used to decrypt administrator password.

7. Security Groups

Security Group acts as a virtual firewall.

Common Ports
Service	Port
SSH	22
RDP	3389
HTTP	80
HTTPS	443
MySQL	3306
MSSQL	1433
PostgreSQL	5432
Source Options
Option	Description
Anywhere	0.0.0.0/0
My IP	Only your system
Custom	Specific network
8. Storage in EC2

EC2 uses EBS (Elastic Block Store).

Default Storage
OS	Default Size
Linux	8 GB
Windows	30 GB
9. Connecting to EC2 Instances
9.1 Windows Instance Connection
Using Remote Desktop
Windows OS
mstsc

Provide:

Public IP
Username
Password
macOS

Install Microsoft “Windows App” from App Store.

9.2 Linux Instance Connection
Using SSH
Command
ssh -i keypair.pem ec2-user@<public-ip>

Example:

ssh -i awar08-kp.pem ec2-user@3.110.169.105
Linux/macOS Permission Fix
chmod 400 keypair.pem
Windows Permission Fix
icacls "C:\path\key.pem" /inheritance:r
icacls "C:\path\key.pem" /grant:r "%USERNAME%:(F)"
10. EBS Volume Management
Identify Volume
sudo file -s /dev/nvme1n1
Format Volume
sudo mkfs -t xfs /dev/nvme1n1
Mount Volume
sudo mkdir /mnt/newvol
sudo mount /dev/nvme1n1 /mnt/newvol
11. Snapshots

Snapshot = Backup copy of EBS volume.

Key Concepts
Point-in-time backup
Stored internally in Amazon S3
Incremental backup mechanism
Cannot directly browse snapshot data
Encryption Workflow

To encrypt an unencrypted volume:

Create snapshot
Create new volume from snapshot
Enable encryption
12. RTO and RPO
RTO (Recovery Time Objective)

Maximum acceptable downtime.

Example

If RTO = 2 hours:

System must recover within 2 hours
RPO (Recovery Point Objective)

Maximum acceptable data loss.

Example

If RPO = 1 hour:

Backups required every hour
13. Golden AMI

Golden AMI = Preconfigured custom AMI.

Use Case

Need 10 servers with:

Custom users
Packages installed
Security configuration
Mounted volumes
Monitoring agents
Traditional Approach

Configure each server manually.

Recommended Approach
Configure one instance
Install all software
Create custom AMI
Launch multiple servers from AMI
14. Instance Metadata Service (IMDS)

Used to retrieve EC2 instance metadata.

Metadata Examples:

Instance ID
AMI ID
IP address
Security groups
IMDSv1

Less secure.

Example:

curl http://169.254.169.254/latest/meta-data/
IMDSv2

Token-based secure metadata access.

Generate Token
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
-H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
Access Metadata
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
http://169.254.169.254/latest/meta-data/ami-id
15. User Data Script

Used to execute commands during first boot.

Example
#!/bin/bash
dnf install httpd -y
systemctl start httpd
systemctl enable httpd

echo "<h1>Website from userdata</h1>" > /var/www/html/index.html
Log File
/var/log/cloud-init-output.log
16. EFS (Elastic File System)

Shared file storage for Linux instances.

Key Features
Unlimited storage
Multiple EC2 instances can access simultaneously
Uses NFS protocol
Port
2049
Mount Command
sudo mount -t nfs4 \
-o nfsvers=4.1 \
fs-xxxx.efs.ap-south-1.amazonaws.com:/ /var/www/html/
17. CloudWatch Monitoring

CloudWatch monitors AWS resources.

Monitoring Types
Type	Interval
Basic	5 minutes
Detailed	1 minute
Default Metrics
CPU
Network
Status checks
Additional Metrics via CloudWatch Agent
Memory usage
Disk usage
Application logs
18. EventBridge

Serverless event bus service.

Common Use Cases
Event-Based
EC2 stopped → Send alert
Security group changed → Notify team
Scheduled
Start dev servers at 8 AM
19. AWS CLI Basics
Configure CLI
aws configure
Verify Identity
aws sts get-caller-identity
Create EC2 Instance
aws ec2 run-instances \
--image-id ami-xxxxxxxx \
--instance-type t3.micro \
--key-name mykey \
--count 1
20. IAM Roles

IAM Roles allow EC2 to access AWS services securely without storing credentials.

Examples:

Access S3
Access Systems Manager
Access CloudWatch
21. Vertical vs Horizontal Scaling
Vertical Scaling

Increase resources on same instance.

Example:

t3.micro → t3.small

Requires:

Instance stop/start
Horizontal Scaling

Add more EC2 instances.

Used with:

Auto Scaling Groups
Load Balancers
22. Load Balancers
Application Load Balancer (ALB)

Layer 7 Load Balancer.

Supports:

HTTP/HTTPS
Path-based routing
Host-based routing
Network Load Balancer (NLB)

Layer 4 Load Balancer.

Supports:

TCP
UDP
TLS

High performance and low latency.

23. Auto Scaling Group (ASG)

Automatically scales EC2 instances.

Scaling Types
Type	Description
Simple Scaling	Basic alarm-based scaling
Step Scaling	Multiple scaling thresholds
Target Tracking	AWS-managed scaling
Scheduled Scaling	Time-based scaling
24. CloudWatch Agent Installation

Install agent:

sudo dnf install amazon-cloudwatch-agent -y
Start Service
sudo systemctl start amazon-cloudwatch-agent
25. Best Practices
Use IAM roles instead of access keys
Enable detailed monitoring for production
Use Golden AMIs for standardization
Restrict security group access
Use snapshots regularly
Enable IMDSv2
Use Auto Scaling for high availability
Store logs in CloudWatch
Use Systems Manager instead of opening SSH everywhere
26. Real-Time Practice Tasks
Beginner Tasks
Launch Linux EC2 instance
Connect using SSH
Install Apache web server
Create HTML page
Attach additional EBS volume
Intermediate Tasks
Configure CloudWatch Agent
Create snapshot and restore volume
Configure EventBridge alert
Create custom AMI
Mount EFS
Advanced Tasks
Configure ALB + ASG
Create Launch Template
Configure scaling policies
Implement Golden AMI architecture
Configure Systems Manager access
