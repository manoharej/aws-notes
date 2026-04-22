# 📘 AWS EFS Multi-AZ EC2 Shared Volume – Implementation Guide

## 🧾 Overview
This document explains how to:
- Launch **2 EC2 instances in different Availability Zones**
- Configure **HTTP server (Apache)**
- Enable **SSH (22) & HTTP (80) access**
- Create and configure **EFS (Elastic File System)**
- Mount EFS on both instances
- Validate shared storage behavior

---

## 🏗️ Architecture
- EC2 Instance 1 → AZ-A  
- EC2 Instance 2 → AZ-B  
- Shared Storage → EFS  
- Mount Path → `/var/www/html`

---

## 🚀 Step 1: Launch EC2 Instances

### Configuration
- AMI: Amazon Linux 2
- Instance Type: t2.micro
- AZ:
  - Instance 1 → AZ-A
  - Instance 2 → AZ-B

### Security Group Rules
| Type | Port | Source |
|------|------|--------|
| SSH  | 22   | Your IP |
| HTTP | 80   | 0.0.0.0/0 |

---

## 🔧 Step 2: Install HTTP Server (Apache)

Run on **both instances**:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

Verify HTTP Service
sudo systemctl status httpd
Test in Browser
http://<EC2-Public-IP>
🔍 Step 3: Pre-check Before Mounting EFS
Check block devices
lsblk
Check mounted file systems
df -hT

👉 EFS should NOT be visible yet

🔐 Step 4: Create EFS Security Group
Name:
NFS-SG
Inbound Rule:
Type	Port	Source
NFS	2049	EC2 Security Group
📦 Step 5: Create EFS File System
Go to AWS Console → EFS → Create File System
Select VPC (same as EC2)
Enable mount targets in multiple AZs
Important:
Remove default security group
Attach NFS-SG
🔗 Step 6: Install EFS Utils

Run on both EC2 instances:

sudo yum install -y amazon-efs-utils
🔌 Step 7: Mount EFS
Create directory
sudo mkdir -p /var/www/html
Mount command (replace fs-id)
sudo mount -t efs fs-xxxx:/ /var/www/html
Recommended (TLS enabled)
sudo mount -t efs -o tls fs-xxxx:/ /var/www/html
✅ Step 8: Verify Mount
df -hT

👉 You should see EFS mounted on /var/www/html

🔍 Troubleshooting
Check Security Group
Port 2049 must be open from EC2 SG
Check Mount Targets
Must exist in both AZs
Check DNS
nslookup fs-xxxx.efs.<region>.amazonaws.com
🧪 Step 9: Validate Shared Storage
On Instance 1:
echo "Hello from Instance 1" | sudo tee /var/www/html/index.html
On Instance 2:
cat /var/www/html/index.html

👉 Output should be:

Hello from Instance 1
🌐 Step 10: Test via Browser
http://<Instance-1-IP>
http://<Instance-2-IP>

👉 Both should display the same content

🔁 Step 11: Auto Mount on Reboot

Edit fstab:

sudo vi /etc/fstab

Add:

fs-xxxx:/ /var/www/html efs defaults,_netdev 0 0
🧪 Final Validation Checklist
Check	Command
HTTP running	systemctl status httpd
Disk before mount	df -hT
Disk after mount	df -hT
EFS mounted	Yes
File shared	Yes
🎯 Key Learnings
EFS is multi-AZ shared storage
Uses NFS protocol (port 2049)
Can be mounted on multiple EC2 instances simultaneously
💡 Interview Tip

EBS vs EFS

Feature	EBS	EFS
Scope	Single AZ	Multi-AZ
Usage	Single instance	Multiple instances
Type	Block storage	File storage
📌 Notes
Ensure same VPC for EC2 and EFS
Security group configuration is critical for mounting
Always verify using df -hT after mount
