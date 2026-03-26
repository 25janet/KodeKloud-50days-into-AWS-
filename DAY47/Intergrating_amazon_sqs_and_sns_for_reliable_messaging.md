#  AWS Priority Queue System (SNS + SQS + Lambda)

##  Project Overview

This project demonstrates how to implement a **priority-based message processing system** using AWS services.

The system routes messages based on priority (high or low) and ensures that high-priority messages are handled first.

---

##  Architecture

SNS Topic → Filters Messages → Routes to SQS Queues → Lambda Processes Messages

* High priority messages → High Priority Queue
* Low priority messages → Low Priority Queue

---

##  Technologies Used

* Amazon SNS
* Amazon SQS
* AWS Lambda
* AWS CloudFormation

---

##  Resources Created

* **SNS Topic**

  * `nautilus-Priority-Queues-Topic`

* **SQS Queues**

  * `nautilus-High-Priority-Queue`
  * `nautilus-Low-Priority-Queue`

* **Lambda Function**

  * `nautilus-priorities-queue-function`

---

##  Key Features

* Message filtering using SNS filter policies
* Separation of workloads based on priority
* Event-driven processing using Lambda
* Infrastructure as Code using CloudFormation

---

##  Testing the System

### Get SNS Topic ARN

```bash
topicarn=$(aws sns list-topics --query "Topics[?contains(TopicArn, 'nautilus-Priority-Queues-Topic')].TopicArn" --output text)
```

### Send High Priority Messages

```bash
aws sns publish --topic-arn $topicarn \
--message 'High Priority message 1' \
--message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}'
```

### Send Low Priority Messages

```bash
aws sns publish --topic-arn $topicarn \
--message 'Low Priority message 1' \
--message-attributes '{"priority":{"DataType":"String","StringValue":"low"}}'
```

---

##  Challenges Faced

### 1. CloudFormation Stack Failure

* Error: `ROLLBACK_COMPLETE`
* Cause: Resource creation failure

### 2. IAM Permission Error

* Error: `iam:PutRolePolicy not authorized`
* Cause: Insufficient permissions to create IAM roles

### 3. File Permission Error

* Error: `E212: Can't open file for writing`
* Cause: Attempted to write to `/root` without sudo

---

##  Solutions Implemented

* Used `describe-stack-events` to debug CloudFormation failures
* Reused an existing IAM role instead of creating a new one
* Used `sudo` to gain permission when editing files
* Simplified Lambda configuration to ensure successful deployment

---

##  Project Structure

```
.
├── nautilus-priority-stack.yml   # CloudFormation template
├── index.py                      # Lambda function code
├── errors.md                     # Documented errors and fixes
```

---

##  Key Learnings

* Debugging CloudFormation is essential for DevOps engineers
* IAM permissions can block deployments if not handled correctly
* SNS filter policies are powerful for routing messages
* Always validate permissions when working in restricted environments

---

##  Conclusion

This project demonstrates how to design a scalable, event-driven system using AWS services while handling real-world deployment challenges.

---
