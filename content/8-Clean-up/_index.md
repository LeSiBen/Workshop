---
title: "Clean-up Resources"
date: "`r Sys.Date()`"
weight: 8
chapter: true
pre: "<b>8. </b>"
---

# Clean-up Resources

## Tổng quan

Properly clean up tất cả AWS resources để avoid ongoing charges. Critical step để prevent unexpected billing.

### ⏱️ **Thời gian ước tính**: 15-20 phút
### 💰 **Cost Impact**: Prevents ongoing charges

## Sections

1. [**Delete Lambda Functions**](8.1-delete-lambda-functions/) - Remove Lambda functions và logs
2. [**Delete ECR Images**](8.2-delete-ecr-images/) - Clean up container images và repositories
3. [**Delete IAM Roles**](8.3-delete-iam-roles/) - Remove IAM roles và policies
4. [**Verify Cleanup**](8.4-verify-cleanup/) - Confirm all resources deleted

## Cleanup Checklist

- [ ] Lambda functions deleted
- [ ] ECR repositories emptied và deleted
- [ ] IAM roles và policies removed
- [ ] CloudWatch log groups deleted
- [ ] Terraform state cleaned
- [ ] No ongoing charges

**⚠️ Important**: Complete cleanup để avoid charges!

Start with [Delete Lambda Functions](8.1-delete-lambda-functions/)!