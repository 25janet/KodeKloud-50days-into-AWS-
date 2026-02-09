# Amazon EC2 Read-Only Access Policy

## Overview

This document explains the behavior and purpose of assigning a **read-only IAM policy** for Amazon EC2 console access, typically using the AWS-managed policy **AmazonEC2ReadOnlyAccess** or an equivalent custom policy.

A read-only policy allows users to **view the configuration and state of EC2 resources** without granting permissions to create, modify, or delete any resources. This approach aligns with AWS security best practices.

---

## What the User *Can* Do

When a user is assigned an EC2 read-only policy, they are granted **List** and **Describe** permissions. In the AWS Management Console, this enables the following capabilities:

### 🔹 View EC2 Instances
- See running, stopped, and terminated instances
- View instance IDs, instance types, public and private IP addresses
- Check availability zones and instance metadata

### 🔹 Monitor Instance Status
- View system and instance health checks
- Access basic monitoring metrics (via Amazon CloudWatch)
- View system logs and status checks

### 🔹 Inspect Networking Configuration
- View Security Groups and associated rules
- See Elastic IPs
- Inspect VPCs, subnets, and network interfaces (read-only)

### 🔹 Storage & Snapshots
- View attached EBS volumes
- Inspect available EBS snapshots
- Review volume size and attachment details

### 🔹 View Resource Tags
- Read metadata tags applied to EC2 resources for identification and cost allocation

---

## What the User *Cannot* Do

A read-only policy **explicitly blocks all mutating actions**. If the user attempts any restricted action, AWS returns an **AccessDenied / UnauthorizedOperation** error.

###  State Changes
- Cannot start, stop, reboot, or terminate EC2 instances

###  Modifications
- Cannot change Security Group rules
- Cannot resize instances or modify instance attributes
- Cannot attach or detach IAM roles

###  Resource Creation
- Cannot launch new EC2 instances
- Cannot create or delete EBS volumes or snapshots

###  Instance Access
- Cannot SSH or RDP into instances
- Read-only access does **not** grant OS-level or network access

---

## Why Use EC2 Read-Only Access?

This policy is a practical implementation of the **Principle of Least Privilege**, ensuring users have only the permissions necessary to perform their responsibilities.

### Common Use Cases

- **Auditors**  
  Verify configurations and security posture without the risk of modification.

- **Junior Developers / Interns**  
  Inspect logs, instance status, and infrastructure layout safely.

- **Finance & Billing Teams**  
  Review running resources to estimate costs and understand infrastructure usage.

---

## Important Notes

- IAM policies are **global**, but EC2 resources are **regional** (e.g., us-east-1).
- EC2 read-only access does **not** grant visibility into other AWS services.
- Users may see **Access Denied** errors when accessing services like:
  - Amazon S3
  - Amazon RDS
  - AWS Lambda  
  unless additional read-only permissions are explicitly granted.

---

## Conclusion

Assigning an EC2 read-only policy provides visibility without risk. It is a secure, controlled way to allow teams to observe infrastructure while protecting production environments from accidental or unauthorized changes.

This approach is essential for maintaining strong governance, compliance, and operational safety in AWS environments.
