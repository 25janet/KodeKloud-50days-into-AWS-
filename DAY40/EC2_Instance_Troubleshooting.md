#  EC2 Internet Accessibility Troubleshooting (Day 40 - KodeKloud)

##  Overview

This project documents the troubleshooting process for an EC2-hosted web application that was not accessible from the internet.

---

##  Problem Statement

An application hosted on an EC2 instance could not be accessed via a browser.

---

##  Troubleshooting Steps

### 1. Network-Level Checks

* Verified **VPC configuration**
* Checked if an **Internet Gateway (IGW)** was attached
* Reviewed **route tables**
* Confirmed subnet type (public vs private)

### 2. Security Configuration

* Checked **Security Groups**

  * HTTP (port 80)
  * HTTPS (port 443)
* Reviewed **Network ACLs (NACLs)**

### 3. Instance-Level Checks

* Verified EC2 instance was running
* Connected via SSH
* Checked web server status:

  ```bash
  sudo systemctl status nginx
  ```

### 4. Application-Level Checks

* Tested locally:

  ```bash
  curl localhost
  ```

---

##  Findings

* Nginx service was **running successfully**
* Application was accessible via:

  ```
  http://<public-ip>
  ```
* HTTPS was **not working**
* The subnet had:

  * No Internet Gateway
  * No public route (0.0.0.0/0 → IGW)
* Default NACL was in use

---

##  Root Cause

The EC2 instance was deployed in a **private subnet without internet routing**, preventing external access to the application.

Additionally, HTTPS failed due to **lack of SSL/TLS configuration**.

---

##  Solution

To enable public access:

1. Attach an **Internet Gateway** to the VPC
2. Update route table:

   ```
   0.0.0.0/0 → Internet Gateway
   ```
3. Associate route table with subnet
4. Assign a **public IP** to EC2
5. Configure Security Group:

   * Allow HTTP (80)
   * Allow HTTPS (443)
6. (Optional) Configure SSL using Let's Encrypt for HTTPS

---

##  Key Takeaways

* Troubleshoot systematically: **Network → Instance → Application**
* A running service does not guarantee external accessibility
* HTTP vs HTTPS issues often relate to SSL configuration
* Understanding VPC architecture is critical in cloud environments

---

##  Next Steps

* Configure HTTPS using SSL certificates
* Explore secure access to private subnets (Bastion Host / Load Balancer)
* Automate deployment using Infrastructure as Code (IaC)

---

