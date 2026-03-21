
---

#  GitHub README

## Project Title: Deploying and Scaling Kubernetes Clusters with Amazon EKS

### Overview

This project demonstrates the creation and management of a Kubernetes cluster using Amazon EKS. It focuses on understanding cluster provisioning, scaling strategies, and workload management in a cloud-native environment.

---

### Objectives

* Create a fully managed Kubernetes cluster using EKS
* Understand the architecture of EKS (control plane and worker nodes)
* Implement scaling strategies at the pod and node level
* Explore autoscaling tools such as Horizontal Pod Autoscaler and Cluster Autoscaler

---

### Technologies Used

* AWS EKS
* Kubernetes
* EC2 (Worker Nodes)
* kubectl
* IAM
* VPC

---

### Steps Performed

#### 1. Cluster Creation

* Created an EKS cluster using AWS Console
* Configured IAM roles and permissions
* Selected appropriate VPC and subnets
* Waited for cluster status to change from *Creating* to *Active*

---

#### 2. Node Group Setup

* Added managed node group (EC2 instances)
* Configured instance type and scaling limits (min, max, desired)

---

#### 3. Kubernetes Configuration

* Connected to the cluster using kubectl
* Verified nodes using:

```bash
kubectl get nodes
```

---

#### 4. Deploying a Sample Application

* Created a deployment:

```bash
kubectl create deployment nginx --image=nginx
```

* Exposed the deployment:

```bash
kubectl expose deployment nginx --type=LoadBalancer --port=80
```

---

#### 5. Scaling the Application

##### Manual Scaling

```bash
kubectl scale deployment nginx --replicas=3
```

##### Horizontal Pod Autoscaler (HPA)

```bash
kubectl autoscale deployment nginx --cpu-percent=50 --min=1 --max=5
```

---

### Key Concepts Learned

#### 1. Multi-Level Scaling in EKS

* Control Plane Scaling (handled by AWS)
* Node Scaling (via Auto Scaling Groups or Cluster Autoscaler)
* Pod Scaling (via HPA)

---

#### 2. Autoscaling

* Horizontal Pod Autoscaler adjusts pods based on CPU usage
* Cluster Autoscaler adjusts nodes when pods cannot be scheduled

---

#### 3. High Availability

* EKS distributes workloads across multiple Availability Zones
* Ensures fault tolerance and reliability

---

### Challenges Faced

* Delay in cluster creation (status remained “Creating” for several minutes)
* Understanding IAM role configurations
* Networking setup within VPC

---

### Key Takeaways

* EKS simplifies Kubernetes management by handling the control plane
* Scaling is a combination of pod-level and infrastructure-level adjustments
* Proper configuration of IAM and networking is critical for successful deployment

---


