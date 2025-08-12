---
title: "Build Container Images"
date: "`r Sys.Date()`"
weight: 3
chapter: true
pre: "<b>3. </b>"
---

# Build Container Images

## Tổng quan

Phần này sẽ hướng dẫn chi tiết cách tạo và optimize container images cho AWS Lambda. Bạn sẽ học cách build 2 versions: standard và optimized, sau đó so sánh performance.

### ⏱️ **Thời gian ước tính**: 45-60 phút

## Nội dung Phần này

### 🐍 **Lambda Function Development**
1. [**Create Lambda Function**](3.1-create-lambda-function/)
   - Write Python Lambda handler
   - Add dependencies và requirements
   - Create test events
   - Local testing setup

### 🐳 **Docker Image Creation**
2. [**Create Dockerfile**](3.2-create-dockerfile/)
   - Standard Dockerfile
   - Multi-stage optimized Dockerfile
   - Best practices implementation
   - Size optimization techniques

3. [**Build Images**](3.3-build-images/)
   - Build standard image
   - Build optimized image
   - Compare image sizes
   - Test images locally

### ☁️ **AWS ECR Integration**
4. [**Create ECR Repository**](3.4-create-ecr-repository/)
   - Setup ECR repository
   - Configure security scanning
   - Set lifecycle policies
   - Enable vulnerability scanning

5. [**Push to ECR**](3.5-push-to-ecr/)
   - ECR authentication
   - Tag và push images
   - Verify uploads
   - Review scan results

## Mục tiêu Phần này

Sau khi hoàn thành, bạn sẽ có:
- ✅ **Working Lambda function** với proper error handling
- ✅ **2 container images**: standard (300MB) và optimized (150MB)
- ✅ **ECR repository** với images uploaded
- ✅ **Security scan results** showing vulnerabilities
- ✅ **Ready-to-deploy** images cho Lambda functions

## Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Source Code   │    │   Docker Build  │    │   ECR Registry  │
│                 │    │                 │    │                 │
│  ┌───────────┐  │    │  ┌───────────┐  │    │  ┌───────────┐  │
│  │   app.py  │  │───▶│  │ Standard  │  │───▶│  │  Image 1  │  │
│  └───────────┘  │    │  │   Image   │  │    │  │  300MB    │  │
│                 │    │  └───────────┘  │    │  └───────────┘  │
│  ┌───────────┐  │    │                 │    │                 │
│  │requirements│  │    │  ┌───────────┐  │    │  ┌───────────┐  │
│  │   .txt    │  │───▶│  │Optimized  │  │───▶│  │  Image 2  │  │
│  └───────────┘  │    │  │   Image   │  │    │  │  150MB    │  │
│                 │    │  └───────────┘  │    │  └───────────┘  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Key Concepts

### **Container Image Optimization**
- **Multi-stage builds**: Reduce final image size
- **Layer caching**: Speed up builds
- **Minimal base images**: Reduce attack surface
- **Dependency optimization**: Only include necessary packages

### **AWS Lambda Container Requirements**
- **Base image**: Must implement Lambda Runtime API
- **Handler function**: Entry point for Lambda
- **Size limits**: 10GB maximum (vs 250MB for ZIP)
- **Architecture**: linux/amd64 hoặc linux/arm64

### **Performance Considerations**
- **Cold start time**: Affected by image size
- **Memory allocation**: Impacts performance và cost
- **Initialization code**: Run once per container lifecycle
- **Concurrent executions**: Container reuse patterns

Ready to start building? Let's begin with [Create Lambda Function](3.1-create-lambda-function/)!