---
title: "8.4 Verify Cleanup"
date: 2024-01-01
weight: 4
---

# 8.4 Verify Complete Cleanup

## Verification Script

```bash
#!/bin/bash

echo "=== Lambda Container Workshop Cleanup Verification ==="

# Check Lambda functions
echo "Checking Lambda functions..."
LAMBDA_FUNCTIONS=$(aws lambda list-functions \
    --query 'Functions[?contains(FunctionName, `lambda-container`)]' \
    --output text)

if [ -z "$LAMBDA_FUNCTIONS" ]; then
    echo "✅ No Lambda functions found"
else
    echo "❌ Lambda functions still exist:"
    echo "$LAMBDA_FUNCTIONS"
fi

# Check ECR repositories
echo "Checking ECR repositories..."
ECR_REPOS=$(aws ecr describe-repositories \
    --query 'repositories[?repositoryName==`lambda-container-workshop`]' \
    --output text 2>/dev/null)

if [ -z "$ECR_REPOS" ]; then
    echo "✅ No ECR repositories found"
else
    echo "❌ ECR repository still exists"
fi

# Check IAM roles
echo "Checking IAM roles..."
IAM_ROLE=$(aws iam get-role \
    --role-name lambda-container-execution-role \
    --output text 2>/dev/null)

if [ $? -ne 0 ]; then
    echo "✅ IAM role deleted"
else
    echo "❌ IAM role still exists"
fi

# Check CloudWatch log groups
echo "Checking CloudWatch log groups..."
LOG_GROUPS=$(aws logs describe-log-groups \
    --log-group-name-prefix "/aws/lambda/lambda-container" \
    --query 'logGroups[*].logGroupName' \
    --output text)

if [ -z "$LOG_GROUPS" ]; then
    echo "✅ No log groups found"
else
    echo "❌ Log groups still exist:"
    echo "$LOG_GROUPS"
fi

echo "=== Cleanup Verification Complete ==="
```

## Final Terraform Destroy

```bash
# Destroy all resources
terraform destroy -auto-approve
```

## Cost Verification

```bash
# Check final costs
aws ce get-cost-and-usage \
    --time-period Start=$(date -d "1 day ago" +%Y-%m-%d),End=$(date +%Y-%m-%d) \
    --granularity DAILY \
    --metrics BlendedCost \
    --group-by Type=DIMENSION,Key=SERVICE \
    --query 'ResultsByTime[0].Groups[?Keys[0]==`AWS Lambda` || Keys[0]==`Amazon Elastic Container Registry (ECR)`]'
```

## Cleanup Checklist

- [ ] Lambda functions deleted
- [ ] ECR repository and images deleted
- [ ] IAM roles and policies deleted
- [ ] CloudWatch log groups deleted
- [ ] CloudWatch alarms deleted
- [ ] SNS topics deleted
- [ ] Terraform state cleaned
- [ ] Local Docker images removed
- [ ] Final cost verification

## Workshop Complete!

Congratulations! You have successfully completed the Lambda Container Images Workshop and cleaned up all resources.