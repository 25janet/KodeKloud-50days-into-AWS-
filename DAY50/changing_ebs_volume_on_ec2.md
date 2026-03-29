#  AWS EBS Volume Expansion (DevOps Task)

##  Project Overview

The Nautilus DevOps Team identified that the EC2 instance **`devops-ec2`** was running low on storage.
The root volume was initially **8 GiB**, and the task was to safely expand it to **12 GiB** without downtime or disrupting ongoing development activities.

---

##  Objectives

* Identify the EBS volume attached to the EC2 instance
* Expand the volume from **8 GiB → 12 GiB**
* Ensure the root partition (`/`) reflects the new size
* Perform all operations safely with no data loss

---

##  Steps Performed

###  Identify Attached Volume

* Navigated to **EC2 Dashboard**
* Selected instance: `devops-ec2`
* Located the attached **EBS volume** under the *Storage* tab

---

###  Modify EBS Volume

* Opened the volume in **Elastic Block Store**
* Selected **Modify Volume**
* Updated size:

  * `8 GiB → 12 GiB`
* Confirmed modification and waited for optimization

---

###  Connect to EC2 Instance

Used SSH to access the instance:

```bash
ssh -i /root/devops-keypair.pem ec2-user@<public-ip>
```

---

###  Verify Disk Size

Checked updated disk:

```bash
lsblk
```

---

###  Expand the Partition

Extended the root partition:

```bash
sudo growpart /dev/xvda 1
```

*(or `/dev/nvme0n1` depending on system)*

---

###  Expand the Filesystem

#### For XFS (Amazon Linux default):

```bash
sudo xfs_growfs /
```

#### For EXT4:

```bash
sudo resize2fs /dev/xvda1
```

---

###  Confirm Changes

Verified new size:

```bash
df -h
```

 Root volume successfully expanded to **12 GiB**

---

##  Key Learnings

* Increasing EBS size alone does **not** expand the filesystem
* Partition and filesystem must be manually resized
* Always follow the correct order:

  1. Modify volume (AWS)
  2. Expand partition
  3. Expand filesystem

---

##  Skills Demonstrated

* AWS EC2 & EBS management
* Linux disk management
* Filesystem resizing (XFS & EXT4)
* Troubleshooting storage issues

---

##  Outcome

The EC2 instance now has sufficient storage capacity, ensuring smooth development operations without interruptions.

---

