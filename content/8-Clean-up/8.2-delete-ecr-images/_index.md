---
title: "8.2 Delete ECR Images"
date: 2024-01-01
weight: 2
---

# 8.2 Delete ECR Images

## List Images

```bash
# List all images in repository
aws ecr list-images \
    --repository-name lambda-container-workshop \
    --region us-east-1
```

## Delete Specific Images

```bash
# Delete standard image
aws ecr batch-delete-image \
    --repository-name lambda-container-workshop \
    --image-ids imageTag=standard \
    --region us-east-1

# Delete optimized image
aws ecr batch-delete-image \
    --repository-name lambda-container-workshop \
    --image-ids imageTag=optimized \
    --region us-east-1
```

## Delete All Images

```bash
# Delete all images in repository
aws ecr list-images \
    --repository-name lambda-container-workshop \
    --query 'imageIds[*]' \
    --output json | \
aws ecr batch-delete-image \
    --repository-name lambda-container-workshop \
    --image-ids file:///dev/stdin
```

## Delete Repository

```bash
# Delete entire repository
aws ecr delete-repository \
    --repository-name lambda-container-workshop \
    --force \
    --region us-east-1
```

## Verify Deletion

```bash
# Verify repository is deleted
aws ecr describe-repositories \
    --query 'repositories[?repositoryName==`lambda-container-workshop`]'
```