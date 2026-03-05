# Day 30 – Enabling Internet Access for a Private EC2 Using a NAT Instance

## 📌 Project Overview

This lab demonstrates how to securely allow a private EC2 instance to access the internet without exposing it to inbound public traffic. The solution leverages a NAT Instance inside a public subnet to route outbound traffic from private subnets.

---

## 🏗 Architecture Design

```
Internet
   ↓
Internet Gateway
   ↓
Public Subnet
   └── NAT Instance (Elastic IP attached)
   ↓
Private Subnet
   └── Private EC2 Instance
```

The private EC2 does not have a public IP and cannot receive inbound internet traffic.

---

##  Implementation Steps

### 1️⃣ VPC and Subnet Configuration

* Created a VPC with CIDR block
* Created:

  * Public Subnet
  * Private Subnet

### 2️⃣ Internet Gateway

* Created and attached an Internet Gateway to the VPC

### 3️⃣ NAT Instance Setup

* Launched EC2 in Public Subnet
* Attached Elastic IP
* Disabled Source/Destination Check
* Configured IP forwarding
* Updated iptables for masquerading outbound traffic

### 4️⃣ Route Table Configuration

**Public Route Table:**

* 0.0.0.0/0 → Internet Gateway

**Private Route Table:**

* 0.0.0.0/0 → NAT Instance (via Instance ID)

### 5️⃣ Security Groups

* NAT Instance SG: Allowed inbound traffic from Private Subnet
* Private EC2 SG: No public inbound rules

---

##  Security Considerations

* Private EC2 has no public IP
* All outbound traffic flows through NAT
* No direct inbound internet exposure
* Network segmentation enforced via route tables

This reduces the attack surface and follows the principle of least privilege.

---

##  Key Technical Concepts

* VPC routing behavior
* Source/Destination check disabling
* Elastic IP usage
* Stateful security groups
* Network Address Translation

---

##  Outcome

The private EC2 instance successfully:

* Installed packages from the internet
* Retrieved external updates
* Remained inaccessible from public networks

This setup mirrors production environments where backend systems require outbound connectivity while maintaining isolation.

---

##  Lessons Learned

* Routing determines traffic flow, not just security groups
* NAT Instances require manual configuration and management
* Proper subnet segmentation is critical for secure architecture

---

**Status:** Successfully implemented and verified connectivity
