---
title: "3.4 Tạo ECR Repository"
date: 2024-01-01
weight: 4
---

# 3.4 Tạo ECR Repository

## Tạo Repository

```bash
# Create ECR repository
aws ecr create-repository \
    --repository-name lambda-container-workshop \
    --region us-east-1
```

## Configure Docker Login

```bash
# Get login token
aws ecr get-login-password --region us-east-1 | \
    docker login --username AWS --password-stdin \
    123456789012.dkr.ecr.us-east-1.amazonaws.com
```

## Verify Repository

```bash
# List repositories
aws ecr describe-repositories --region us-east-1
```

## Repository Settings

- Enable vulnerability scanning
- Set lifecycle policies
- Configure permissions