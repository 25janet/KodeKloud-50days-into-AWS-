# Day 15: Creating an EBS Snapshot from an EC2 Instance

## Overview
On Day 15, I created a snapshot of an Amazon Elastic Block Store (EBS) volume attached to an EC2 instance.  
An EBS snapshot is a point-in-time backup of the data stored on an EBS volume. Snapshots are stored securely in Amazon S3 and can be used to restore data, create new volumes, or recover from failures.

---

## What is an EBS Snapshot?
An **EBS Snapshot** is a backup of an EBS volume that captures the state of the data at a specific moment in time.  
It is **incremental**, meaning that after the first snapshot, only the changed blocks are saved, which helps reduce storage costs.

Snapshots can be used to:
- Restore a volume if data is lost or corrupted
- Create new EBS volumes
- Create AMIs (Amazon Machine Images)
- Migrate data across Availability Zones or Regions

---

## Importance of EBS Snapshots
EBS snapshots are important because they:

- **Provide data backup and recovery**  
  Protect data from accidental deletion, corruption, or system failure.

- **Enable disaster recovery**  
  Snapshots can be copied to other regions to support recovery in case of regional outages.

- **Support scalability and automation**  
  Snapshots can be used to quickly create new volumes for scaling applications.

- **Improve cost efficiency**  
  Incremental snapshots save only changed data blocks, reducing storage costs.

- **Help with compliance and auditing**  
  Snapshots allow you to retain historical data states for compliance purposes.

---

## Steps to Create an EBS Snapshot

### Step 1: Open the EC2 Dashboard
- Sign in to the AWS Management Console
- Navigate to **EC2**
- From the left-hand menu, select **Volumes** under *Elastic Block Store*

### Step 2: Select the EBS Volume
- Identify the EBS volume attached to your EC2 instance
- Select the volume you want to back up

### Step 3: Create the Snapshot
- Click **Actions**
- Choose **Create snapshot**
- Add a **Description** (for example: `Snapshot for Day 15 backup`)
- (Optional) Add **Tags** for better organization
- Click **Create snapshot**

### Step 4: Verify the Snapshot
- Go to **Snapshots** in the EC2 console
- Confirm that the snapshot status changes from `pending` to `completed`

---

## Use Cases of the Snapshot
- Restoring data after accidental deletion
- Creating a new EBS volume from the snapshot
- Attaching the restored volume to an EC2 instance
- Creating an AMI for launching identical instances

---

## Conclusion
Creating an EBS snapshot is a critical practice for maintaining data durability and reliability in AWS.  
It ensures that data can be recovered quickly, supports scalability, and plays a key role in building highly available and fault-tolerant cloud architectures.

---


