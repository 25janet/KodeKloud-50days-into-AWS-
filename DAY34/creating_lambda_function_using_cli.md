# AWS Lambda Function Using AWS CLI

## Project Overview

This project demonstrates how to create and deploy a serverless function using the AWS CLI.

The function was written in Python, packaged into a zip file, and deployed to AWS Lambda using command-line commands.

This exercise is part of my AWS / DevOps hands-on practice.

---

## Technologies Used

* AWS Lambda
* AWS CLI
* Python
* IAM Roles

Services used from AWS:

* Amazon Web Services
* AWS Lambda
* AWS CLI

---

## Project Architecture

User → AWS CLI → AWS Lambda → Response

The Lambda function processes the request and returns a response with a status code and message.

---

## Step 1: Create the Python Script

Create a file named:

```
lambda_function.py
```

Add the following code:

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }
```

---

## Step 2: Zip the Function

Package the function into a zip file for deployment.

```bash
zip function.zip lambda_function.py
```

---

## Step 3: Create the Lambda Function

```bash
aws lambda create-function \
--function-name devops-lambda-cli \
--runtime python3.9 \
--role <IAM_ROLE_ARN> \
--handler lambda_function.lambda_handler \
--zip-file fileb://function.zip
```

---

## Step 4: Test the Lambda Function

Invoke the function using AWS CLI.

```bash
aws lambda invoke \
--function-name devops-lambda-cli \
response.json
```

View the response:

```bash
cat response.json
```

Expected output:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

## Key Learning Outcomes

* Understanding serverless architecture
* Deploying Lambda functions using AWS CLI
* Packaging application code for deployment
* Using IAM roles for Lambda execution

---

## Author

Janet John
Cloud & DevOps Student
