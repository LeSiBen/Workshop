---
title: "4.3 Deploy Lambda Functions"
date: 2024-01-01
weight: 3
---

# 4.3 Deploy Lambda Functions

## Standard Lambda Function

```hcl
resource "aws_lambda_function" "lambda_standard" {
  function_name = "lambda-container-standard"
  role         = aws_iam_role.lambda_execution_role.arn
  
  package_type = "Image"
  image_uri    = "${var.ecr_repository_url}:standard"
  
  timeout     = 30
  memory_size = 512
  
  environment {
    variables = {
      ENVIRONMENT = "workshop"
      VERSION     = "standard"
    }
  }
}
```

## Optimized Lambda Function

```hcl
resource "aws_lambda_function" "lambda_optimized" {
  function_name = "lambda-container-optimized"
  role         = aws_iam_role.lambda_execution_role.arn
  
  package_type = "Image"
  image_uri    = "${var.ecr_repository_url}:optimized"
  
  timeout     = 30
  memory_size = 512
  
  environment {
    variables = {
      ENVIRONMENT = "workshop"
      VERSION     = "optimized"
    }
  }
}
```

## Deploy

```bash
terraform plan
terraform apply
```