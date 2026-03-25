
# AWS S3 File Automation with Lambda and DynamoDB

## Overview

This project demonstrates how to automate file transfers between two S3 buckets using **AWS Lambda** and track operations using **DynamoDB**. The workflow ensures that files uploaded to a public bucket are automatically copied to a private bucket, with logs stored securely for auditing purposes.

## Architecture

1. **Public S3 Bucket**: `datacenter-public-2795` — used for uploading files. Public access is enabled only for uploads.
2. **Private S3 Bucket**: `datacenter-private-2095` — used to store files securely. Public access is blocked.
3. **Lambda Function**: `datacenter-copyfunction` — triggered on file upload to the public bucket. Copies files to the private bucket and logs transfer details in DynamoDB.
4. **DynamoDB Table**: `datacenter-S3CopyLogs` — stores log entries with fields:
   - `LogID` (Partition Key)
   - `source_bucket`
   - `destination_bucket`
   - `object_key`

## Steps Taken

1. **Created the public and private S3 buckets** with appropriate access controls.
2. **Created the DynamoDB table** to store transfer logs.
3. **Created an IAM role (`lambda_execution_role`)** with permissions for:
   - Reading from the public bucket
   - Writing to the private bucket
   - Writing to DynamoDB
   - Logging to CloudWatch
4. **Edited the Lambda function** (`lambda-function.py`) to include:
   ```python
   table_name = "datacenter-S3CopyLogs"
   destination_bucket = "datacenter-private-2095"


5. **Zipped the Lambda function** and uploaded it to the Lambda console.
6. **Added an S3 trigger** to invoke the Lambda function on file uploads.
7. **Tested the workflow** by uploading `sample.zip` to the public bucket and verifying that:

   * File was copied to the private bucket
   * Log entry was created in DynamoDB

## Challenges Encountered

* Attempted to upload `.py` file directly to Lambda instead of `.zip` — caused `--zip-file must be a zip file` error. Resolved by zipping the file.
* Typo in Lambda function name caused `Function not found` error — fixed by checking exact function names with `aws lambda list-functions`.
* Balancing **public and private bucket access** to maintain security while allowing automation.

## Outcome

* Fully automated workflow between public and private S3 buckets.
* Logs stored in DynamoDB for audit and visibility.
* Reinforced practical knowledge of **AWS Lambda triggers, IAM policies, S3 permissions, and DynamoDB integration**.




