# Day 31 – Configuring a Private RDS Instance for Application Development

## 📌 Project Overview

This lab focuses on provisioning a private MySQL RDS instance for application development following secure cloud architecture principles. The objective was to ensure the database layer is isolated from direct internet exposure.

---

## 🏗 Architecture Design

```
Internet
   ↓
Load Balancer / Public Layer
   ↓
Application EC2 Instance
   ↓
Private RDS Instance
```

The database resides in private subnets and is not publicly accessible.

---

##  Configuration Details

### 1️⃣ Database Configuration

* Engine: MySQL 8.4.x
* Instance Class: db.t3.micro
* Deployment Template: Sandbox
* DB Identifier: nautilus-rds

### 2️⃣ Storage Configuration

* Storage Autoscaling: Enabled
* Maximum Storage Threshold: 50 GB

### 3️⃣ Networking Configuration

* Deployed in Private Subnets via DB Subnet Group
* Public Access: Disabled
* No public IP assigned

### 4️⃣ Security Group Configuration

* Allowed inbound traffic only from Application EC2 Security Group
* No 0.0.0.0/0 inbound rules

---

##  Why Private RDS is Critical

### Network Isolation

The database is isolated within private subnets, preventing direct internet access.

### Reduced Attack Surface

Without public exposure, the risk of brute-force attempts and automated scanning attacks is minimized.

### Principle of Least Privilege

Only trusted application servers are permitted to communicate with the database.

### Defense in Depth

Even if the application layer is compromised, the database layer remains protected by subnet boundaries and strict security group rules.

---

##  Key Technical Concepts Reinforced

* VPC subnet segmentation
* DB Subnet Groups across multiple Availability Zones
* Stateful Security Groups
* Application-layer database access control
* Storage autoscaling for cost and capacity management

---

##  Outcome

* RDS instance reached "Available" state
* Application layer can securely connect
* Database remains inaccessible from the public internet

This setup mirrors production-grade backend architectures where the data layer must be strictly protected.

---

##  Lessons Learned

* Public accessibility settings directly impact security posture
* Storage autoscaling helps manage unpredictable growth
* Secure architecture is about controlled access, not just functionality

---

**Status:** Successfully provisioned and validated
