# AWS VPC – Practical + Theory Guide

## What is VPC?

VPC (Virtual Private Cloud) is a logically isolated network in AWS where you can launch and manage AWS resources securely.

**Key points:**

* Region specific service
* Provides isolation similar to an on-premise data center network
* Gives control over:

  * IP ranges (CIDR)
  * Subnets
  * Route tables
  * Internet access
  * Security

---

# Networking Fundamentals

## IPv4 vs IPv6

| Type |    Size | Address Space    |
| ---- | ------: | ---------------- |
| IPv4 |  32-bit | ~4.3 Billion     |
| IPv6 | 128-bit | ~340 undecillion |

IPv4 example:

192.168.1.10

IPv6 example:

2001:db8::1

---

## Public IP vs Private IP

### Public IP

* Accessible over internet
* Globally unique
* Assigned through ISP

### Private IP

* Used inside internal networks
* Not reachable directly from internet

Private ranges:

Class A: 10.0.0.0 – 10.255.255.255

Class B: 172.16.0.0 – 172.31.255.255

Class C: 192.168.0.0 – 192.168.255.255

---

## Useful commands

Windows:

```bash
ipconfig /all
```

Linux:

```bash
ifconfig
ip addr
ip
```

---

# IPv4 Classes

| Class | Range   | Format    |
| ----- | ------- | --------- |
| A     | 1–126   | N.H.H.H   |
| B     | 128–191 | N.N.H.H   |
| C     | 192–223 | N.N.N.H   |
| D     | 224–239 | Multicast |
| E     | 240–255 | Reserved  |

127.0.0.1 → localhost / loopback

---

# DHCP

Dynamic Host Configuration Protocol automatically allocates IP addresses.

---

# CIDR Basics

Examples:

| CIDR | Total IPs |
| ---- | --------: |
| /28  |        16 |
| /27  |        32 |
| /24  |       256 |
| /16  |    65,536 |

AWS reserves 5 IPs in every subnet:

1. Network Address
2. VPC Router
3. DNS
4. Future use
5. Broadcast reservation equivalent

Usable examples:

| CIDR | Total | Usable |
| ---- | ----: | -----: |
| /28  |    16 |     11 |
| /27  |    32 |     27 |
| /24  |   256 |    251 |

Subnet range:

* Minimum: /28
* Maximum: /16

---

# VPC Design Questions

Before creating VPC:

1. What CIDR size is needed?
2. Number of subnets?
3. Future expansion?
4. Multi-AZ requirement?

Example:

Public:

* Web-AZ1
* Web-AZ2

Private:

* App-AZ1
* App-AZ2

Database:

* DB-AZ1
* DB-AZ2

---

# Practical VPC Creation

CIDR:

192.168.0.0/24

Creates automatically:

* Default Security Group
* Main Route Table
* Default NACL

## Subnets

Public:

192.168.0.0/27
192.168.0.32/27

Private:

192.168.0.64/27
192.168.0.96/27

Database:

192.168.0.128/27
192.168.0.160/27

---

# Internet Gateway

Provides internet connectivity.

Rules:

* Attach to VPC
* One IGW per VPC

---

# Route Tables

Local route exists by default.

Public Route Table:

0.0.0.0/0 → IGW

Private Route Table:

0.0.0.0/0 → NAT Gateway

Database Route Table:

No internet route

---

# Practical Testing

Public EC2:

```bash
wget google.com
```

Should work.

Private EC2:

No internet.

Connect through Bastion/Public EC2.

Enable ICMP in Security Group if using ping.

---

# NAT Gateway

Used to give outbound internet to private instances.

Must be deployed in Public subnet.

Types:

1. Public NAT Gateway
2. Private NAT Gateway

---

# Security Group vs NACL

| Security Group | NACL         |
| -------------- | ------------ |
| Stateful       | Stateless    |
| Allow only     | Allow + Deny |
| EC2 level      | Subnet level |

Ephemeral ports:

1024–65535

Common ports:

HTTP:80
HTTPS:443
RDP:3389
MySQL:3306
MSSQL:1433

---

# VPC Endpoints

Gateway Endpoint:

* S3
* DynamoDB
* Lower cost

Interface Endpoint:

* Creates ENI
* Additional cost

Benefit:
Private subnet EC2 can access AWS services without NAT.

---

# VPC Peering

Allows VPC communication.

Rules:

* Non-overlapping CIDR
* Non-transitive

Steps:

1. Create peering request
2. Accept request
3. Update routes both sides

---

# Transit Gateway

Hub-and-spoke architecture.

Steps:

1. Create TGW
2. Share through RAM
3. Attach VPCs
4. Update routes

---

# IPv6 Internet

IPv4 private internet:
NAT Gateway

IPv6:
Egress-only Internet Gateway

---

# Extend VPC CIDR

Possible:

Add secondary CIDR.

Not possible:

Expand existing subnet.

Create new subnet instead.

---

# Site-to-Site VPN

Components:

Customer Gateway
Virtual Private Gateway
VPN Connection

Common vendors:

* Cisco
* Fortinet
* Palo Alto
* Yamaha
* F5
* Dell SonicWall
