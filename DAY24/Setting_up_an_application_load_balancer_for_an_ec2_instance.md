# AWS Application Load Balancer Setup

## Project Overview

This project demonstrates how to configure an **Application Load Balancer (ALB)** in front of an EC2 instance running an Nginx web server.

The objective is to route HTTP traffic from the internet through the ALB to the EC2 instance securely and efficiently.

---

## Architecture

Internet  
   ↓  
Application Load Balancer (datacenter-alb)  
   ↓  
Target Group (datacenter-tg)  
   ↓  
EC2 Instance (datacenter-ec2 running Nginx)

---

## Components Created

### 1. Security Group (datacenter-sg)

Created a security group to allow public HTTP traffic.

**Inbound Rules:**
- HTTP (Port 80) → 0.0.0.0/0

**Outbound Rules:**
- Allow all traffic (default)

This security group was attached to the Application Load Balancer.

---

### 2. Target Group (datacenter-tg)

Configured a target group to register the EC2 instance.

**Configuration:**
- Target Type: Instance
- Protocol: HTTP
- Port: 80
- Health Check Path: /

The EC2 instance `datacenter-ec2` was registered as a target.

---

### 3. EC2 Security Group Update

The default security group attached to the EC2 instance was modified.

**Inbound Rule Added:**
- HTTP (Port 80)
- Source: datacenter-sg (ALB security group)

This ensures that only the load balancer can access the EC2 instance.

---

### 4. Application Load Balancer (datacenter-alb)

Configured an internet-facing ALB.

**Settings:**
- Scheme: Internet-facing
- Protocol: HTTP (Port 80)
- Subnets: At least two public subnets (for High Availability)
- Security Group: datacenter-sg
- Listener: Forward HTTP traffic to datacenter-tg

---

## Traffic Flow

1. User sends request to ALB DNS name.
2. ALB listener receives traffic on port 80.
3. Traffic is forwarded to datacenter-tg.
4. Target group routes traffic to the healthy EC2 instance.
5. Nginx responds with the web page.

---

## High Availability & Best Practices

- ALB deployed in multiple Availability Zones.
- Health checks automatically monitor instance status.
- EC2 only accepts traffic from the ALB security group.
- Public access is restricted to the ALB layer.

---

## Validation Checklist

- datacenter-alb created successfully
- datacenter-tg created and attached
- datacenter-sg configured correctly
- EC2 inbound rule updated
- Target status shows Healthy
- Application accessible via ALB DNS name

---

## Key Learning Outcomes

- Understanding ALB architecture
- Configuring target groups and health checks
- Implementing secure security group rules
- Designing for high availability
- Routing HTTP traffic through Layer 7 load balancing
