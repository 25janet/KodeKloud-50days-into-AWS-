#  Public VPC with EC2 Instance (SSH Access)

##  Project Overview

This project demonstrates how to configure a Public VPC in AWS and
launch an EC2 instance that is accessible over the internet using SSH
(Port 22).

The goal of this lab is to understand:

-   VPC networking fundamentals\
-   Public vs Private subnets\
-   Internet Gateway configuration\
-   Route table configuration\
-   Security Groups\
-   SSH connectivity

------------------------------------------------------------------------

##  Architecture Overview

Internet\
⬇\
Internet Gateway\
⬇\
Public Subnet\
⬇\
EC2 Instance (Public IP)

------------------------------------------------------------------------

##  Resources Created

-   VPC\
-   Public Subnet\
-   Internet Gateway (IGW)\
-   Route Table\
-   EC2 Instance\
-   Security Group\
-   Key Pair

------------------------------------------------------------------------

## 🛠 Step-by-Step Configuration

### 1️⃣ Create a VPC

-   Define CIDR block (e.g., 10.0.0.0/16)

### 2️⃣ Create a Public Subnet

-   Example CIDR: 10.0.1.0/24
-   Enable Auto-assign Public IP

### 3️⃣ Create and Attach an Internet Gateway

-   Create Internet Gateway
-   Attach it to the VPC

### 4️⃣ Configure Route Table

Add route:

Destination: 0.0.0.0/0\
Target: Internet Gateway (IGW)

Associate the route table with the public subnet.

### 5️⃣ Create Security Group

Inbound Rule: - Type: SSH - Protocol: TCP - Port: 22 - Source: 0.0.0.0/0

Note: In production, restrict SSH access to your IP only.

###  Launch EC2 Instance

-   Select AMI (Amazon Linux or Ubuntu)
-   Choose instance type
-   Select created VPC and subnet
-   Enable Auto-assign Public IP
-   Select created security group
-   Create or use existing key pair

------------------------------------------------------------------------

##  Connect to EC2 via SSH

For Amazon Linux: ssh -i your-key.pem ec2-user@`<Public-IP>`{=html}

For Ubuntu: ssh -i your-key.pem ubuntu@`<Public-IP>`{=html}

------------------------------------------------------------------------

##  Key Concepts Learned

-   A subnet becomes public when its route table points 0.0.0.0/0 to an
    Internet Gateway.
-   An EC2 instance must have:
    -   A public IP
    -   A route to an IGW
    -   Security group allowing port 22
-   NAT Gateway is not required for public SSH access.
-   Security Groups control inbound and outbound traffic at the instance
    level.

------------------------------------------------------------------------

##  Security Considerations

For production environments:

-   Do NOT allow SSH from 0.0.0.0/0
-   Use your specific IP address
-   Consider using Systems Manager Session Manager or Bastion Hosts

------------------------------------------------------------------------

##  Outcome

Successfully configured a Public VPC and launched an EC2 instance
accessible via SSH over the internet.
