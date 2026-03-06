# Day 32 – Snapshot and Restoration of an RDS Instance

## Overview

In this lab, I practiced creating and restoring **Amazon RDS snapshots** as part of a database backup and disaster recovery strategy.

Snapshots provide a **point-in-time backup** of an RDS database instance, allowing recovery in case of system failure, accidental deletion, or database corruption.

---

## Technologies Used

* AWS
* Amazon RDS
* RDS Snapshots

---

## Architecture Concept

The workflow implemented in this lab:

1. Create an RDS database instance
2. Generate a manual snapshot of the database
3. Store the snapshot securely
4. Restore the snapshot to create a new database instance

This process ensures **database durability and recoverability**.

---

## Step 1 – Create an RDS Instance

A new database instance was launched using Amazon RDS.

Configuration included:

* Database engine: MySQL
* Instance type: db.t3.micro
* Default VPC configuration
* Storage configured according to lab requirements

---

## Step 2 – Create a Manual Snapshot

A manual snapshot was created from the running RDS instance.

Steps:

1. Navigate to the **RDS Console**
2. Select the database instance
3. Click **Actions**
4. Choose **Take Snapshot**
5. Provide a snapshot name

Example snapshot name:

```
nautilus-rds-snapshot
```

Manual snapshots remain available until they are deleted manually.

---

## Step 3 – Restore the Snapshot

To recover the database, the snapshot was restored.

Steps:

1. Navigate to **Snapshots**
2. Select the created snapshot
3. Click **Restore Snapshot**
4. Provide a new DB instance identifier

Example restored instance:

```
nautilus-rds-restored
```

AWS creates a **new RDS instance from the snapshot**, ensuring the original database remains unchanged.

---

## Key Concepts Learned

### 1. Point-in-Time Backups

Snapshots allow databases to be restored to a specific state captured at the time of backup.

### 2. Disaster Recovery

Snapshots protect against:

* Accidental data deletion
* Application errors
* Database corruption
* Infrastructure failures

### 3. Safe Restoration

Restoring a snapshot creates a **new database instance**, preventing accidental overwriting of the original database.

---

## Conclusion

Amazon RDS snapshots provide a reliable mechanism for **database backup, recovery, and cloning**. Understanding how to create and restore snapshots is essential for implementing effective **disaster recovery strategies** in cloud environments.
