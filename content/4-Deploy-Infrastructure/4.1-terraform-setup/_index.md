---
title: "4.1 Terraform Setup"
date: 2024-01-01
weight: 1
---

# 4.1 Terraform Setup

## Initialize Terraform

```bash
# Create terraform directory
mkdir terraform
cd terraform

# Initialize Terraform
terraform init
```

## Provider Configuration

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}
```

## Variables

```hcl
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "ecr_repository_url" {
  description = "ECR repository URL"
  type        = string
}
```