#  AWS Priority Queue System (SNS + SQS + Lambda)

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Python](https://img.shields.io/badge/Python-3.9-blue)
![Lambda](https://img.shields.io/badge/Lambda-Serverless-purple)
![CloudFormation](https://img.shields.io/badge/CloudFormation-IaC-red)

---

##  Project Overview

This project demonstrates a **priority-based message processing system** using AWS services. High-priority messages are processed first, while low-priority messages are handled afterward.  

The system uses:

- **Amazon SNS** – Distributes messages to the right queue based on priority  
- **Amazon SQS** – Holds messages in high and low priority queues  
- **AWS Lambda** – Processes messages automatically  
- **CloudFormation** – Deploys the entire architecture as code  

---

##  Message Flow

1. Producer sends a message to the SNS topic  
2. SNS evaluates the message attribute `priority` (high/low)  
3. SNS routes messages using **filter policies**:  
   - High → High Priority Queue  
   - Low → Low Priority Queue  
4. Lambda function polls both queues  
5. High-priority messages are processed first  

---

##  System Diagram

```mermaid
flowchart TD
                ┌──────────────────────────┐
                │        SNS Topic         │
                │  (Message Distributor)   │
                └──────────┬───────────────┘
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
┌──────────────────────┐         ┌──────────────────────┐
│ High Priority Queue  │         │ Low Priority Queue   │
│   (Critical Tasks)   │         │   (Normal Tasks)     │
└──────────┬───────────┘         └──────────┬───────────┘
           │                                │
           └──────────────┬─────────────────┘
                          │
                ┌──────────────────────────┐
                │     Lambda Function      │
                │   (Message Processor)    │
                └──────────────────────────┘