# Day 20 – IAM Role for EC2 with Policy Attachment

## Overview

On Day 20 of my Cloud Challenge, I created an **IAM Role for an EC2 instance** and attached a **customer-managed policy** to it.

This task demonstrates how AWS services securely access other AWS resources without using hardcoded credentials.

---

## What is an IAM Role?

An IAM Role is a set of permissions that can be **assumed by trusted entities**, such as:

- AWS services (e.g., EC2, Lambda)
- IAM users
- Applications

IAM Roles provide **temporary security credentials** through AWS Security Token Service (STS), making them more secure than using long-term access keys.

---

## Task Implementation

### 1. Created IAM Role
- Trusted entity type: **AWS Service**
- Use case selected: **EC2**
- This allows EC2 instances to assume the role.

### 2. Attached Policy
- Attached a **Customer-Managed Policy**
- The policy defines what actions the EC2 instance is allowed to perform.
- Permissions were customized instead of using AWS managed policies.

### 3. Purpose
Once attached to an EC2 instance, the instance can:
- Access permitted AWS resources
- Use temporary credentials automatically
- Operate securely without storing access keys

---

## Significance of This Task

### 1. Enhanced Security
Using IAM Roles eliminates the need to store access keys inside EC2 instances.  
AWS automatically provides temporary credentials that rotate securely.

### 2. Principle of Least Privilege
By attaching a **customer-managed policy**, permissions can be tightly controlled to allow only required actions.

### 3. Secure Service-to-Service Access
This setup enables EC2 to securely interact with other AWS services such as:
- S3
- CloudWatch
- DynamoDB
- Systems Manager

### 4. Scalability
If multiple EC2 instances require the same permissions, the same IAM Role can be reused.

### 5. Industry Best Practice
Attaching roles to EC2 is the recommended and secure method for granting permissions in AWS environments.

---

## Architecture Flow

IAM Policy → Attached to IAM Role → Assigned to EC2 Instance → Access to AWS Resources

---

## Key Learning Outcome

I learned how IAM Roles provide secure, temporary access to AWS services and why attaching policies to roles (instead of directly to resources) is a best practice in cloud security.

---


