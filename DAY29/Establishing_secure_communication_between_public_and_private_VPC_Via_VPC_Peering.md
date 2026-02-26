# Nautilus VPC Peering Lab - README.md

## Overview

This lab demonstrates setting up a **VPC Peering Connection** between a **Default VPC** and a **Private VPC** in AWS, configuring route tables, security groups, and verifying connectivity between EC2 instances.

**Goal:** Enable communication between a public EC2 instance in the Default VPC and a private EC2 instance in the Private VPC.

---

## Lab Environment

* **Public EC2 Instance:** nautilus-public-ec2 (Default VPC)
* **Private VPC:** nautilus-private-vpc, CIDR: 10.1.0.0/16
* **Private Subnet:** nautilus-private-subnet, CIDR: 10.1.1.0/24
* **Private EC2 Instance:** nautilus-private-ec2 (inside nautilus-private-subnet)
* **VPC Peering Connection Name:** nautilus-vpc-peering

---

## Steps Followed

### Step 5: Create VPC Peering

1. Go to AWS Console → **VPC → Peering Connections**.
2. Click **Create Peering Connection**.

   * Requester VPC: Default VPC
   * Accepter VPC: nautilus-private-vpc
   * Name: nautilus-vpc-peering
3. Accept the peering request.
4. Verify the status is **Active**.

**Mistake / Challenge:** Initially forgot to accept the peering request, which made routes show as **Blackhole**.

**Solution:** Accepted the request; status changed to Active and routes became valid.

---

### Step 6: Configure Route Tables

#### Default VPC Route Table

* Destination: 10.1.0.0/16 → Target: nautilus-vpc-peering

#### Private VPC Route Table

* Destination: Default VPC CIDR (e.g., 172.31.0.0/16) → Target: nautilus-vpc-peering

**Mistake / Challenge:** Initially did not verify routes on both VPCs; traffic would not flow properly.

**Solution:** Added missing routes to both VPCs; verified connectivity.

---

### Step 7: Test the Connection

#### Part 1: Security Group Configuration

* Go to **EC2 → Private EC2 → Security Group → Inbound Rules**
* Add rule:

  * Type: **All ICMP - IPv4**
  * Source: Default VPC CIDR (e.g., 172.31.0.0/16)

**Challenge:** Initially forgot to allow ICMP; ping from public EC2 failed.

**Solution:** Added the rule to allow ICMP traffic from the public VPC.

#### Part 2: SSH Key Setup

* On **AWS Client Host** (KodeKloud terminal), run:

  ```bash
  cat ~/.ssh/id_rsa.pub
  ```
* Copy the public key.
* Initially tried SSH directly from client host, but connection froze (no prompt) because:

  * SSH port was blocked in the public EC2 security group.
  * EC2 did not have the client host public key.

**Solution Steps:**

1. Added **SSH rule (port 22)** in public EC2 security group.
2. Used **EC2 Instance Connect** from AWS Console to access public EC2.
3. Opened `~/.ssh/authorized_keys` on public EC2:

   ```bash
   nano ~/.ssh/authorized_keys
   ```
4. Pasted the public key at the bottom.
5. Saved file (CTRL + O → Enter, CTRL + X).

#### Part 3: Verify SSH and Ping

* SSH from AWS Client Host to public EC2:

  ```bash
  ssh ec2-user@<public-ip>
  ```
* Verify login succeeds (key-based authentication works)
* From public EC2, ping private EC2:

  ```bash
  ping <private-ip>
  ```
* Successful ping confirms VPC peering, route tables, and security groups are properly configured.

**Challenge:** Initially froze when trying to SSH; resolved by adding security group rule and updating authorized_keys via EC2 Instance Connect.

---

## Mistakes and Solutions Summary

| Mistake / Challenge                                         | Solution                                                                                        |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Peering request not accepted → route table showed Blackhole | Accepted peering request, status became Active                                                  |
| Route tables missing routes in one or both VPCs             | Added correct routes to both VPCs                                                               |
| Private EC2 blocked ICMP traffic → ping failed              | Added inbound ICMP rule to private EC2 security group from Default VPC CIDR                     |
| SSH to public EC2 froze / permission denied                 | Added SSH inbound rule, added client public key to `authorized_keys` using EC2 Instance Connect |
| Confusion about AWS Client Host                             | Clarified that KodeKloud terminal is the AWS Client Host                                        |

---

## Challenges Experienced

1. Understanding what the lab meant by **AWS Client Host**.
2. Route tables showing **Blackhole** due to peering not accepted.
3. Connection froze when trying to SSH to public EC2 (security group issue).
4. Permission denied on SSH because public key was missing.
5. Getting familiar with `nano` editor for authorized_keys file.

---

## Lessons Learned

* Always accept VPC peering requests before testing connectivity.
* Both VPC route tables must have peering routes configured.
* Security groups must allow the correct traffic (ICMP for ping, SSH for access).
* EC2 Instance Connect is a handy fallback to update `authorized_keys`.
* KodeKloud terminal is effectively your AWS Client Host for labs.
* Debugging SSH connectivity requires checking network, security group, and key permissions step by step.

---


