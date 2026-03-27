# 🚀 Nautilus Lambda Deployment with CloudFormation

This project demonstrates how to deploy an AWS Lambda function using **Infrastructure as Code (IaC)** with AWS CloudFormation.

---

##  Project Overview

The Nautilus DevOps team required a serverless solution to deploy a Lambda function using a CloudFormation stack.

This implementation automates:
- IAM Role creation
- Lambda function deployment
- Inline Python function execution

---

##  Architecture

CloudFormation Stack → IAM Role → Lambda Function

---

##  Resources Created

###  IAM Role
- **Name:** lambda_execution_role
- Provides permissions for Lambda execution
- Uses AWS managed policy:
  - `AWSLambdaBasicExecutionRole`

---

###  Lambda Function
- **Name:** nautilus-lambda
- **Runtime:** Python 3.9
- **Handler:** index.lambda_handler

####  Function Behavior
Returns:
```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

### Deployment Steps
---
Save the template:

/root/nautilus-lambda.yml

### Deploy using AWS CLI:
```
 aws cloudformation create-stack \
  --stack-name nautilus-lambda-app \
  --template-body file:///root/nautilus-lambda.yml \
  --capabilities CAPABILITY_NAMED_IAM
```