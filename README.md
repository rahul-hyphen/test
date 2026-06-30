
# Phase 1: Launch EC2 Instance

Launch an Ubuntu EC2 instance and paste the following script in **User Data**.

## User Data Script

```bash
#!/bin/bash

apt update -y

apt install git unzip -y

apt install docker.io -y
systemctl start docker
systemctl enable docker

usermod -aG docker ubuntu

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip"

cd /tmp

unzip awscliv2.zip

./aws/install

cd /home/ubuntu

git clone https://github.com/master-ankitt/todo-app-via-aws-services.git

chown -R ubuntu:ubuntu /home/ubuntu/todo-app-via-aws-services

echo "User data execution completed" > /var/log/userdata-success.log
```

---

## Verify Installation

```bash
docker --version
```

```bash
aws --version
```

```bash
git --version
```

```bash
cd ~/todo-app-via-aws-services
```

```bash
ls
```

---

# Phase 2: Create IAM User

Navigate:

```text
IAM
  ↓
Users
  ↓
Create User
```

User Name:

```text
ankit
```

Attach the following permissions:

```text
AmazonElasticContainerRegistryPublicFullAccess
```

```text
AmazonElasticContainerRegistryPublicPowerUser
```

```text
AmazonElasticContainerRegistryPublicReadOnly
```

```text
AmazonEC2FullAccess
```

---

# Phase 3: Create Access Key

Navigate:

```text
IAM
  ↓
Users
  ↓
ankit
  ↓
Security Credentials
  ↓
Create Access Key
```

---

# Phase 4: Configure AWS CLI

Run:

```bash
aws configure
```

Provide:

```text
AWS Access Key ID
```

```text
AWS Secret Access Key
```

```text
ap-south-1
```

Verify:

```bash
aws sts get-caller-identity
```

---

# Phase 5: Create Public ECR Repository

Navigate:

```text
Amazon ECR
  ↓
Public Registry
  ↓
Repositories
  ↓
Create Repository
```

Repository Name:

```text
todo-app
```

Operating System:

```text
Linux
```

Architecture:

```text
x86-64
```

Click:

```text
Create Repository
```

---

## Push Docker Image to ECR

Navigate:

```text
Repository
  ↓
View Push Commands
```

Run the generated commands.

### Login to ECR

```bash
sudo aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/q2k6c3h5
```

### Build Docker Image

```bash
cd ~/todo-app-via-aws-services
```

```bash
sudo docker build -t todo-app .
```

### Tag Docker Image

```bash
sudo docker tag todo-app:latest public.ecr.aws/q2k6c3h5/todo-app:latest
```

### Push Docker Image

```bash
sudo docker push public.ecr.aws/q2k6c3h5/todo-app:latest
```

---

# Phase 6: Create ECS Cluster

Navigate:

```text
Amazon ECS
  ↓
Clusters
  ↓
Create Cluster
```

Select:

```text
Fargate Only
```

Cluster Name:

```text
todo-app-cluster
```

Container Insights:

```text
Container Insights with Enhanced Observability
```

Click:

```text
Create
```

---

# Phase 7: Create Task Definition

Navigate:

```text
Amazon ECS
  ↓
Task Definitions
  ↓
Create Task Definition
```

Task Name:

```text
todo-task-1
```

Launch Type:

```text
AWS Fargate
```

Container Name:

```text
todo-app
```

Image URI:

```text
public.ecr.aws/q2k6c3h5/todo-app:latest
```

Container Port:

```text
8000
```

Protocol:

```text
TCP
```

Log Collection:

```text
Amazon CloudWatch Logs
```

Click:

```text
Create
```

---

# Phase 8: Run ECS Task

Navigate:

```text
Amazon ECS
  ↓
Clusters
  ↓
todo-app-cluster
```

Select:

```text
Run New Task
```

Task Definition:

```text
todo-task-1
```

Enable:

```text
Public IP
```

Security Group Port:

```text
8000
```

Click:

```text
Create
```

---

# Phase 9: Verify Application

Navigate:

```text
Cluster
  ↓
Tasks
  ↓
Running Task
```

Copy:

```text
Public IP Address
```

Open:

```text
http://<PUBLIC-IP>:8000
```

Example:

```text
http://13.232.xxx.xxx:8000
```

---

# Phase 10: View Logs in CloudWatch

Navigate:

```text
CloudWatch
  ↓
Log Groups
```

Select the ECS log group and verify container logs.

---

# Useful Commands

## Verify Docker Images

```bash
docker images
```

## Verify Running Containers

```bash
docker ps
```

## Verify ECS Tasks

```bash
aws ecs list-tasks --cluster todo-app-cluster
```

## Verify AWS Identity

```bash
aws sts get-caller-identity
```
