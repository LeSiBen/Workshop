---
title: "3.5 Push Images to ECR"
date: 2024-01-01
weight: 5
---

# 3.5 Push Images to ECR

## Tag Images

```bash
# Tag standard image
docker tag lambda-standard:latest \
    123456789012.dkr.ecr.us-east-1.amazonaws.com/lambda-container-workshop:standard

# Tag optimized image
docker tag lambda-optimized:latest \
    123456789012.dkr.ecr.us-east-1.amazonaws.com/lambda-container-workshop:optimized
```

## Push Images

```bash
# Push standard version
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/lambda-container-workshop:standard

# Push optimized version
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/lambda-container-workshop:optimized
```

## Verify Push

```bash
# List images in repository
aws ecr list-images \
    --repository-name lambda-container-workshop \
    --region us-east-1
```

## Next Steps

Images are now ready for Lambda deployment!