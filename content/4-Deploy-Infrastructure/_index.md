---
title: "Deploy Infrastructure"
date: "`r Sys.Date()`"
weight: 4
chapter: true
pre: "<b>4. </b>"
---

# Deploy Infrastructure

## Tổng quan

Sử dụng Terraform để deploy AWS infrastructure including Lambda functions, IAM roles, và CloudWatch resources.

### ⏱️ **Thời gian ước tính**: 30-45 phút

## Sections

1. [**Terraform Setup**](4.1-terraform-setup/) - Configure Terraform với AWS provider
2. [**Create IAM Roles**](4.2-create-iam-roles/) - Setup Lambda execution roles
3. [**Deploy Lambda Functions**](4.3-deploy-lambda-functions/) - Create Lambda functions từ container images
4. [**Verify Deployment**](4.4-verify-deployment/) - Test deployed functions

## Mục tiêu

- ✅ Infrastructure as Code với Terraform
- ✅ Lambda functions deployed từ ECR images
- ✅ Proper IAM roles và permissions
- ✅ CloudWatch logging enabled

Ready to deploy? Start with [Terraform Setup](4.1-terraform-setup/)!