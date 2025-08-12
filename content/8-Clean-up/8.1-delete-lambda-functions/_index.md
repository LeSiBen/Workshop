---
title: "8.1 Delete Lambda Functions"
date: 2024-01-01
weight: 1
---

# 8.1 Delete Lambda Functions

## Delete via Terraform

```bash
# Destroy specific resources
terraform destroy -target=aws_lambda_function.lambda_standard
terraform destroy -target=aws_lambda_function.lambda_optimized
```

## Delete via AWS CLI

```bash
# Delete standard function
aws lambda delete-function \
    --function-name lambda-container-standard

# Delete optimized function
aws lambda delete-function \
    --function-name lambda-container-optimized
```

## Delete Provisioned Concurrency

```bash
# Delete provisioned concurrency first
aws lambda delete-provisioned-concurrency-config \
    --function-name lambda-container-optimized \
    --qualifier $LATEST
```

## Verify Deletion

```bash
# List remaining functions
aws lambda list-functions \
    --query 'Functions[?contains(FunctionName, `lambda-container`)]'
```

## Clean up Log Groups

```bash
# Delete log groups
aws logs delete-log-group \
    --log-group-name /aws/lambda/lambda-container-standard

aws logs delete-log-group \
    --log-group-name /aws/lambda/lambda-container-optimized
```