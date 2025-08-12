---
title: "8.3 Delete IAM Roles"
date: 2024-01-01
weight: 3
---

# 8.3 Delete IAM Roles

## Detach Policies First

```bash
# List attached policies
aws iam list-attached-role-policies \
    --role-name lambda-container-execution-role

# Detach managed policies
aws iam detach-role-policy \
    --role-name lambda-container-execution-role \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

## Delete Inline Policies

```bash
# List inline policies
aws iam list-role-policies \
    --role-name lambda-container-execution-role

# Delete inline policy
aws iam delete-role-policy \
    --role-name lambda-container-execution-role \
    --policy-name lambda-additional-permissions
```

## Delete Role

```bash
# Delete the role
aws iam delete-role \
    --role-name lambda-container-execution-role
```

## Delete via Terraform

```bash
# Destroy IAM resources
terraform destroy -target=aws_iam_role.lambda_execution_role
terraform destroy -target=aws_iam_role_policy.lambda_additional_permissions
terraform destroy -target=aws_iam_role_policy_attachment.lambda_basic_execution
```

## Verify Deletion

```bash
# Verify role is deleted
aws iam get-role \
    --role-name lambda-container-execution-role
```