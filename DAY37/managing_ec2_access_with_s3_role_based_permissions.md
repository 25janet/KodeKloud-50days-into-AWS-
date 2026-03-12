# AWS EC2 Access to S3 using IAM Roles

This project demonstrates how to securely allow an EC2 instance to interact with an S3 bucket using IAM roles instead of storing AWS credentials on the server.

## Architecture

EC2 Instance → IAM Role → S3 Bucket

The EC2 instance assumes an IAM role that grants permission to access the S3 bucket.

## Services Used

- Amazon EC2
- Amazon S3
- AWS Identity and Access Management (IAM)
- AWS CLI

## Lab Objectives

1. Configure secure SSH access to an EC2 instance.
2. Create a private S3 bucket.
3. Create an IAM policy with restricted S3 permissions.
4. Create an IAM role and attach the policy.
5. Attach the IAM role to the EC2 instance.
6. Verify file upload and access using AWS CLI.

## Step 1: Generate SSH Key Pair

```bash
ssh-keygen -t rsa
````

This creates:

```
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

The public key is added to the EC2 instance for secure login.

## Step 2: Create Private S3 Bucket

Bucket name:

```
datacenter-s3-4327
```

Public access is blocked to ensure the bucket remains private.

## Step 3: Create IAM Policy

Example policy allowing access to the S3 bucket:

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Action": ["s3:ListBucket"],
   "Resource": "arn:aws:s3:::datacenter-s3-4327"
  },
  {
   "Effect": "Allow",
   "Action": ["s3:PutObject", "s3:GetObject"],
   "Resource": "arn:aws:s3:::datacenter-s3-4327/*"
  }
 ]
}
```

## Step 4: Create IAM Role

Role name:

```
datacenter-role
```

The role is configured for **EC2** and attached to the created policy.

## Step 5: Attach Role to EC2 Instance

The IAM role is attached to the existing EC2 instance:

```
datacenter-ec2
```

This allows the instance to obtain temporary AWS credentials automatically.

## Step 6: Test Access

Create a test file:

```bash
echo "hello nautilus" > test.txt
```

Upload to S3:

```bash
aws s3 cp test.txt s3://datacenter-s3-4327/
```

List objects in the bucket:

```bash
aws s3 ls s3://datacenter-s3-4327/
```

Successful output confirms the EC2 instance has permission to interact with the S3 bucket.

## Key Learning

Using **IAM roles** instead of storing AWS access keys on servers improves security by providing temporary credentials and enabling better permission management.


