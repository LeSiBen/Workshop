---
title: "3.2 Tạo Dockerfile"
date: 2024-01-01
weight: 2
---

# 3.2 Tạo Dockerfile

## Tổng quan

Trong bước này, chúng ta sẽ tạo Dockerfile để build container image cho Lambda function.

## Tạo Dockerfile cơ bản

```dockerfile
FROM public.ecr.aws/lambda/python:3.9

# Copy requirements và install dependencies
COPY requirements.txt ${LAMBDA_TASK_ROOT}
RUN pip install -r requirements.txt

# Copy function code
COPY app.py ${LAMBDA_TASK_ROOT}

# Set handler
CMD [ "app.lambda_handler" ]
```

## Tạo Dockerfile tối ưu

```dockerfile
FROM public.ecr.aws/lambda/python:3.9

# Install system dependencies
RUN yum update -y && \
    yum install -y gcc && \
    yum clean all

# Copy và install Python dependencies
COPY requirements.txt ${LAMBDA_TASK_ROOT}
RUN pip install --no-cache-dir -r requirements.txt && \
    pip cache purge

# Copy application code
COPY app.py ${LAMBDA_TASK_ROOT}

# Set handler
CMD [ "app.lambda_handler" ]
```

## Best Practices

- Sử dụng AWS base images
- Optimize layer caching
- Minimize image size
- Use multi-stage builds khi cần thiết