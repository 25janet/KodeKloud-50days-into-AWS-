# Xfusion Cloud Deployment

This project demonstrates deploying a **containerized Python application** using AWS container services.

The application is built using **Docker**, stored in a **private Amazon Elastic Container Registry (ECR)** repository, and deployed using **Amazon Elastic Container Service (ECS) with the Fargate launch type**.

---

# Architecture Overview

The deployment process follows this flow:

Developer Environment (aws-client)
        ↓
Build Docker Image
        ↓
Tag Image for ECR
        ↓
Push Image to Amazon ECR
        ↓
Amazon ECS pulls the image from ECR
        ↓
ECS runs the container using Fargate

---

# What is the aws-client Host?

The **aws-client** host acts as the **development and management environment** used to interact with AWS services.

It is used to:

- Build Docker images from application source code
- Authenticate with AWS using the AWS CLI
- Push container images to Amazon ECR
- Manage ECS clusters and services

Essentially, the **aws-client machine acts like a DevOps workstation**, where developers prepare container images before deploying them to the cloud.

---

# Why We Build a Docker Image

A **Docker image** is a packaged version of an application that includes:

- The application source code
- Required runtime environment (Python, libraries, etc.)
- System dependencies
- Configuration needed to run the application

By building a Docker image, we ensure the application runs **consistently across different environments**.

Without containerization, applications may fail due to missing dependencies or configuration differences between systems.

Docker solves this by packaging everything needed into a single portable image.

---

# Why We Tag the Docker Image

Docker images use **tags to identify versions of the image**.

Example:

docker-image:latest  
docker-image:v1.0

Tagging is important because:

- It allows version control of application images
- AWS ECR requires images to have a specific repository URI and tag
- ECS references images using this tag when deploying containers

In this project, the image is tagged with the **ECR repository URI** so that it can be uploaded to Amazon ECR.

---

# Why We Push the Image to Amazon ECR

Amazon ECS does **not run images stored locally on the aws-client machine**.

Instead, ECS pulls container images from a **container registry**.

Amazon ECR serves as a **secure private repository** for storing Docker images.

The workflow is therefore:

1. Build the image locally on the aws-client host
2. Tag the image with the ECR repository URI
3. Push the image to Amazon ECR
4. ECS pulls the image from ECR and runs it as a container

---

# Deployment Steps

## 1 Create Private ECR Repository

```bash
aws ecr create-repository --repository-name xfusion-ecr --region us-east-1
````

## 2 Build the Docker Image

Navigate to the application directory:

```bash
cd /root/pyapp
```

Build the Docker image:

```bash
docker build -t xfusion-ecr .
```

## 3 Tag the Image for ECR

```bash
docker tag xfusion-ecr:latest ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

## 4 Push the Image to ECR

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com

docker push ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

## 5 Create ECS Cluster

```bash
aws ecs create-cluster --cluster-name xfusion-cluster
```

## 6 Register Task Definition

```bash
aws ecs register-task-definition --cli-input-json file://task-definition.json
```

## 7 Deploy ECS Service

```bash
aws ecs create-service \
--cluster xfusion-cluster \
--service-name xfusion-service \
--task-definition xfusion-taskdefinition \
--desired-count 1 \
--launch-type FARGATE
```

---

# Key Learnings

* Understanding the **Docker image lifecycle**: build → tag → push → deploy
* Using **Amazon ECR as a container registry**
* Deploying containers using **Amazon ECS with Fargate**
* Managing cloud infrastructure through the **AWS CLI**



