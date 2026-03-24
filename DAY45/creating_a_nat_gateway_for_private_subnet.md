#  AWS NAT Gateway Setup for Private Subnet Internet Access

##  Project Overview

This project demonstrates how to enable secure internet access for an EC2 instance running in a private subnet using a NAT Gateway in AWS.

The goal is to allow outbound internet connectivity without exposing the instance to inbound public traffic.

---

##  Architecture

Private EC2 → Private Subnet → NAT Gateway → Internet Gateway → Internet (S3)

---

##  Resources Used

* VPC: `datacenter-priv-vpc`
* Private Subnet: `datacenter-priv-subnet`
* Public Subnet: `datacenter-pub-subnet`
* EC2 Instance: `datacenter-priv-ec2`
* NAT Gateway: `datacenter-natgw`
* S3 Bucket: `datacenter-nat-22893`

---

##  Steps Implemented

### 1. Created a Public Subnet

* CIDR block within the VPC range
* Designed to host internet-facing resources

### 2. Configured Internet Gateway

* Created and attached to the VPC
* Enables internet access for public resources

### 3. Created Public Route Table

* Route added:

  * `0.0.0.0/0 → Internet Gateway`
* Associated with the public subnet

### 4. Deployed NAT Gateway

* Created in the public subnet
* Allocated an Elastic IP for external communication

### 5. Updated Private Route Table

* Route added:

  * `0.0.0.0/0 → NAT Gateway`
* Enables outbound internet traffic for private instances

---

##  Verification

* The EC2 instance in the private subnet successfully accessed the internet
* A test file was uploaded automatically to the S3 bucket:

  * `datacenter-nat-22893`
* This confirmed that the NAT Gateway was correctly configured

---

##  Key Learnings

* Public subnets require a route to an Internet Gateway
* Private subnets use NAT Gateways for outbound internet access
* Route tables control traffic flow within a VPC
* NAT Gateways enhance security by preventing inbound access

---

##  Common Issues

* NAT Gateway placed in a private subnet
* Missing Internet Gateway attachment
* Incorrect route table configuration
* No Elastic IP assigned to NAT Gateway

---

##  Outcome

Successfully implemented a secure and scalable networking setup allowing private resources to access the internet without being publicly exposed.

---

##  Future Improvements

* Add high availability using multiple NAT Gateways
* Implement auto-scaling for EC2 instances
* Introduce monitoring with CloudWatch

---
