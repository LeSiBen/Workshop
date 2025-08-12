---
title: "4.4 Verify Deployment"
date: 2024-01-01
weight: 4
---

# 4.4 Verify Deployment

## Test Lambda Functions

```bash
# Test standard function
aws lambda invoke \
    --function-name lambda-container-standard \
    --payload '{"test": "data"}' \
    response-standard.json

# Test optimized function
aws lambda invoke \
    --function-name lambda-container-optimized \
    --payload '{"test": "data"}' \
    response-optimized.json
```

## Check Responses

```bash
# View responses
cat response-standard.json
cat response-optimized.json
```

## Verify in AWS Console

- Lambda Functions list
- CloudWatch Logs
- Function configurations

## Troubleshooting

- Check IAM permissions
- Verify image URIs
- Review CloudWatch logs