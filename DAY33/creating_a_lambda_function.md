# Day 33 – Creating an AWS Lambda Function

## Overview
In this lab, I created a serverless function using AWS Lambda as part of my cloud engineering learning journey with KodeKloud.

AWS Lambda allows developers to run code without provisioning or managing servers. The service automatically handles scaling and only runs the code when triggered by an event.

## Objective
Create a Lambda function with the following specifications:

- Function Name: datacenter-lambda
- Runtime: Python
- IAM Role: lambda_execution_role
- Response Status Code: 200
- Response Body: Welcome to KKE AWS Labs!

## Implementation

### Lambda Function Code

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }