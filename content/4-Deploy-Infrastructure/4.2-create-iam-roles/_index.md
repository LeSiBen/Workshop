---
title: "4.2 Create IAM Roles"
date: 2024-01-01
weight: 2
---

# 4.2 Create IAM Roles

## Lambda Execution Role

```hcl
resource "aws_iam_role" "lambda_execution_role" {
  name = "lambda-container-execution-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "lambda.amazonaws.com"
        }
      }
    ]
  })
}
```

## Attach Policies

```hcl
resource "aws_iam_role_policy_attachment" "lambda_basic_execution" {
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
  role       = aws_iam_role.lambda_execution_role.name
}
```

## Additional Permissions

```hcl
resource "aws_iam_role_policy" "lambda_additional_permissions" {
  name = "lambda-additional-permissions"
  role = aws_iam_role.lambda_execution_role.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "logs:CreateLogGroup",
          "logs:CreateLogStream",
          "logs:PutLogEvents"
        ]
        Resource = "arn:aws:logs:*:*:*"
      }
    ]
  })
}
```