#  Highly Available Web Application on AWS (ASG + ALB + Nginx)

##  Project Overview

This project demonstrates how to build a highly available and scalable web application architecture on AWS using:

* EC2 Launch Templates
* Auto Scaling Groups (ASG)
* Application Load Balancer (ALB)
* Target Groups with Health Checks
* Nginx Web Server

The system automatically scales EC2 instances based on CPU utilization and distributes traffic across healthy instances.

---

##  Architecture

User → Application Load Balancer → Target Group → EC2 Instances (Nginx)
↑
Auto Scaling Group

---

##  Setup Steps

### 1. Security Group

* Allowed inbound HTTP (Port 80) from anywhere

---

### 2. Launch Template

* AMI: Amazon Linux 2
* Instance type: t2.micro
* User Data script:

```bash
#!/bin/bash
yum update -y
amazon-linux-extras install nginx1 -y
systemctl start nginx
systemctl enable nginx
```

---

### 3. Target Group

* Protocol: HTTP
* Port: 80
* Health check path: `/`

---

### 4. Application Load Balancer

* Internet-facing
* Listener: HTTP (Port 80)
* Forwarding to target group

---

### 5. Auto Scaling Group

* Min: 1
* Desired: 1
* Max: 2
* Scaling policy: Target tracking (CPU at 50%)
* Attached to target group

---

##  Issue Encountered

### Problem:

EC2 instances were marked as **unhealthy** in the target group.

### Error Message:

No package nginx available.

### Root Cause:

Amazon Linux 2 does not include Nginx in the default yum repository.

---

##  Solution

Used Amazon Linux Extras to install Nginx:

```bash
amazon-linux-extras install nginx1 -y
systemctl start nginx
systemctl enable nginx
```

After applying this fix:

* Nginx started successfully
* Health checks passed
* Instances became healthy
* ALB successfully routed traffic

---

##  Verification

* Accessed the ALB DNS name in a browser
* Confirmed the default Nginx page was displayed

---

##  Key Learnings

* Importance of correct package management in Amazon Linux 2
* How health checks impact load balancing
* Debugging unhealthy targets in AWS
* Automating infrastructure with Launch Templates and ASG

---

##  Future Improvements

* Add HTTPS using AWS Certificate Manager (ACM)
* Deploy a custom web application instead of default Nginx page
* Implement monitoring with CloudWatch dashboards

---

##  Output

Successfully served the Nginx default page through the Application Load Balancer.

---

##  Author

Janet Wanjiru John

Cloud & DevOps Learner
