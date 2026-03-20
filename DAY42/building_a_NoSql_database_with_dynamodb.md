#  DynamoDB NoSQL Database - DevOps Tasks

##  Project Overview
This project demonstrates the creation and management of a NoSQL database using **Amazon DynamoDB**.  
The goal is to simulate a simple task management system using cloud-native services.

---

##  Technologies Used
- AWS DynamoDB
- AWS CLI
- JSON

---

##  Table Details

- **Table Name:** devops-tasks
- **Primary Key:** taskId (String)

---

##  Data Inserted

### Task 1
```json
{
  "taskId": "1",
  "description": "Learn DynamoDB",
  "status": "completed"
}
````

### Task 2

```json
{
  "taskId": "2",
  "description": "Build To-Do App",
  "status": "in-progress"
}
```

---

##  Steps Performed

### 1. Create Table

```bash
aws dynamodb create-table \
  --table-name devops-tasks \
  --attribute-definitions AttributeName=taskId,AttributeType=S \
  --key-schema AttributeName=taskId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

---

### 2. Insert Data

```bash
aws dynamodb put-item \
  --table-name devops-tasks \
  --item '{
    "taskId": {"S": "1"},
    "description": {"S": "Learn DynamoDB"},
    "status": {"S": "completed"}
  }'
```

---

### 3. Retrieve Data

```bash
aws dynamodb get-item \
  --table-name devops-tasks \
  --key '{"taskId": {"S": "1"}}'
```

---

## 🔍 Key Learnings

* Understanding NoSQL database concepts
* Working with schema-less data structures
* Using AWS CLI for database operations
* Difference between Query and Scan operations

---





