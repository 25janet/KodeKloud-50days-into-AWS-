
# Nautilus DevOps Log Aggregation Project

## Project Overview
This project demonstrates a secure and scalable log aggregation setup in AWS. The goal is to collect log files from a **private EC2 instance**, transfer them securely to a **public EC2 instance**, and finally push them to an **S3 bucket** for storage and analysis.

The workflow simulates a real-world DevOps log pipeline, including automation with **cron jobs**.

### **1️⃣ Networking Setup**

* **Created a Public VPC** alongside the existing private VPC to separate resources based on accessibility.
* **Created subnets** in each VPC:

  * Private subnet for the internal EC2 instance that generates logs.
  * Public subnet for the EC2 instance that receives logs and pushes them to S3.
* **Configured route tables** to control network traffic:

  * Ensured the private subnet could communicate with the public subnet via **VPC peering**.
  * Configured the public subnet route table to allow outbound internet access via an **Internet Gateway**.

---

### **2️⃣ Compute Resources**

* **Launched EC2 instances** in both private and public subnets:

  * The **private EC2** is isolated in the private subnet and generates logs.
  * The **public EC2** receives logs from the private EC2 and pushes them to S3.
* **Secured access with SSH keys**, including generating a dedicated transfer key for automation to avoid sharing sensitive keys.

---

### **3️⃣ Storage & Permissions**

* **Created an S3 bucket** to store logs in a centralized, organized structure.
* **Created an IAM role and policy**:

  * Attached the role to the public EC2.
  * Granted **minimum necessary permissions** (PutObject) for S3, ensuring security best practices.

---

### **4️⃣ Automation**

* **Configured cron jobs**:

  * On the private EC2: automatically SCP logs to the public EC2.
  * On the public EC2: automatically upload logs to S3.
* Ensured logs were organized in S3 by **VPC and log type**.

---




## Architecture Flow
1. **Private EC2 (datacenter-priv-ec2)**:
   - Generates a dedicated SSH key (`log_transfer_key`) for secure log transfer.
   - Uses a cron job to SCP `/var/log/boots.log` to the public EC2 every minute.

2. **Public EC2 (datacenter-pub-ec2)**:
   - Receives logs from the private EC2.
   - Uses an IAM role (`datacenter-s3-role`) with `PutObject` permission to upload logs to S3.
   - Cron job automatically uploads `/tmp/boots.log` to S3 under the path:
     ```
     s3://datacenter-s3-logs-17876/datacenter-priv-vpc/boot/boots.log
     ```

3. **S3 Bucket (datacenter-s3-logs-17876)**:
   - Centralized storage for logs, organized by VPC and log type.
   - Only authorized public EC2 instance has write access.

---

## Key Commands and Configurations

### Private EC2: Generate Transfer Key
```bash
ssh-keygen -t ed25519 -f ~/.ssh/log_transfer_key -C "log-transfer" -N ""
cat ~/.ssh/log_transfer_key.pub
````

### Public EC2: Authorize Key

```bash
echo 'PASTE_PUBLIC_KEY_HERE' >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### Private EC2: Cron Job for SCP

```bash
* * * * * /usr/bin/scp -i ~/.ssh/log_transfer_key \
  -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null \
  /var/log/boots.log ubuntu@PUBLIC_PRIVATE_IP:/tmp/boots.log >> ~/scp-transfer.log 2>&1
```

### Public EC2: Cron Job for S3 Upload

```bash
* * * * * /snap/bin/aws s3 cp /tmp/boots.log \
  s3://datacenter-s3-logs-17876/datacenter-priv-vpc/boot/boots.log >> ~/s3-upload.log 2>&1
```

---

## Challenges & Learnings

* **Key Management:** Avoid copying private PEM keys. Created a dedicated `log_transfer_key` for secure transfers.
* **VPC Networking:** Used private IPs for SCP over VPC peering to avoid internet dependency.
* **Cron Testing:** Learned to verify cron jobs by checking log files (`scp-transfer.log` and `s3-upload.log`).
* **AWS CLI S3 Upload:** Verified paths and permissions to ensure files successfully land in S3.

---

## Future Improvements

* Add logging timestamped filenames to prevent overwrites.
* Use Lambda and CloudWatch for serverless log aggregation.
* Implement error notifications for failed SCP or S3 uploads.

---



