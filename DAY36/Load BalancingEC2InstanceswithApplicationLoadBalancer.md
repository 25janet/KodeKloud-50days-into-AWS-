Here’s a cleaned-up and slightly enhanced version of your **Nautilus Cloud Deployment Lab** documentation, with corrected formatting, complete code blocks, and clearer step descriptions:

---

# Nautilus Cloud Deployment Lab

This project demonstrates the deployment of an **NGINX web server** on an **Ubuntu EC2 instance** behind an **Application Load Balancer (ALB)** using AWS services.

The lab covers:

* Automated EC2 instance setup using **user data scripts**.
* Security group configuration for secure HTTP access.
* Application Load Balancer (ALB) creation and routing.
* Target Group configuration and health checks.

---

## Architecture Overview

```text
Internet
   ↓
ALB (nautilus-alb, default security group)
   ↓
Target Group (nautilus-tg)
   ↓
EC2 Instance (nautilus-ec2, nautilus-sg)
   ↓
NGINX Web Server
```

---

## Step 1: Create Security Group

* **Name:** `nautilus-sg`
* **Rules:** Allow HTTP (port 80) from the ALB security group.
* Ensures secure communication between the ALB and the EC2 instance.

---

## Step 2: Launch EC2 Instance

* **Name:** `nautilus-ec2`
* **AMI:** Ubuntu (latest LTS recommended)
* **Security Group:** Attach `nautilus-sg`
* **User Data Script:** Automatically installs and starts NGINX.

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

* This script ensures NGINX is installed, started, and enabled to run on boot.

---

## Step 3: Create Target Group

* **Name:** `nautilus-tg`
* **Target Type:** Instance
* **Protocol / Port:** HTTP : 80
* **Register Targets:** Add the EC2 instance (`nautilus-ec2`)
* **Health Checks:** HTTP path `/` (default)

---

## Step 4: Create Application Load Balancer

* **Name:** `nautilus-alb`
* **Scheme:** Internet-facing
* **Listener:** HTTP : 80
* **Security Group:** Default or custom ALB SG
* **Target Group:** Forward traffic to `nautilus-tg`

---

## Step 5: Verify Deployment

* **Target Health:** Should show **Healthy** in the Target Group.
* **Access via ALB DNS:** Open a browser and navigate to:

```
http://<your-alb-dns>
```

* You should see the default NGINX welcome page, confirming a successful deployment.

---

This setup ensures a scalable and secure NGINX deployment on AWS, with traffic routed through an ALB to the EC2 instance.

---

