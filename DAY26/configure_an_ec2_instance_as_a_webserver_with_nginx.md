# Day 26 – Configuring an EC2 Instance as a Web Server with Nginx

##  Project Overview
As part of my AWS/DevOps learning journey, I configured an EC2 instance to function as a web server using Nginx. The goal was to automate the server setup using a user data script and ensure the instance was publicly accessible over HTTP.

---

##  Architecture

Internet  
↓  
Internet Gateway  
↓  
Public Subnet  
↓  
EC2 Instance (Nginx Web Server)

---

## ⚙ Services Used

- Amazon EC2
- Internet Gateway
- Security Groups
- Ubuntu Server AMI
- User Data Script (Bootstrapping)

---

##  Implementation Steps

### 1️⃣ EC2 Instance Configuration
- Instance Name: `devops-ec2`
- AMI: Ubuntu Server
- Instance Type: t2.micro
- Auto-assign Public IP: Enabled
- Subnet: Public Subnet

---

### 2️⃣ Security Group Rules

| Type | Protocol | Port | Source |
|------|----------|------|--------|
| HTTP | TCP | 80 | 0.0.0.0/0 |
| SSH  | TCP | 22 | My IP |

This ensures the web server is accessible from the internet while keeping SSH restricted.

---

### 3️⃣ User Data Script

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx