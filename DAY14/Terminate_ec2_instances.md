# KodeKloud Challenge – Day 13  
## Terminating an EC2 Instance

## Overview
Day 13 of the KodeKloud challenge focused on **terminating an Amazon EC2 instance**. This task highlights the importance of proper resource lifecycle management in AWS to avoid unnecessary costs and maintain a clean cloud environment.

## What Does Terminating an EC2 Instance Mean?
Terminating an EC2 instance means **permanently deleting** the instance from AWS. Once an instance is terminated:
- It **cannot be recovered**
- Any data stored on **instance store volumes is lost**
- Attached **EBS volumes may be deleted** depending on their delete-on-termination setting
- Billing for the instance **stops immediately**


## Steps to Terminate an EC2 Instance
1. Log in to the **AWS Management Console**
2. Navigate to the **EC2 Dashboard**
3. Select **Instances** from the left-hand menu
4. Choose the EC2 instance to terminate
5. Click **Instance state**
6. Select **Terminate instance**
7. Confirm the termination

## Important Notes
- Termination is **irreversible**
- Always verify the instance before terminating, especially in production
- Create an **AMI or EBS snapshot** if the instance data is needed later

## Key Learning Outcome
This challenge emphasized the importance of **cost optimization, safe cleanup of resources, and effective EC2 lifecycle management** in AWS.
