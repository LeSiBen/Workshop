---
title: "6.1 Security Scanning"
date: 2024-01-01
weight: 1
---

# 6.1 Security Scanning

## ECR Vulnerability Scanning

```bash
# Enable scanning on push
aws ecr put-image-scanning-configuration \
    --repository-name lambda-container-workshop \
    --image-scanning-configuration scanOnPush=true
```

## Scan Results

```bash
# Get scan results
aws ecr describe-image-scan-findings \
    --repository-name lambda-container-workshop \
    --image-id imageTag=optimized
```

## Docker Security Scanning

```bash
# Scan with Docker Scout
docker scout cves lambda-optimized

# Scan with Trivy
trivy image lambda-optimized
```

## Security Best Practices

```dockerfile
# Use non-root user
RUN groupadd -r appuser && useradd -r -g appuser appuser
USER appuser

# Remove unnecessary packages
RUN yum remove -y gcc && yum clean all

# Set security labels
LABEL security.scan="enabled"
```

## Remediation

- Update base images regularly
- Remove unnecessary dependencies
- Use minimal base images
- Implement least privilege access
- Regular security audits