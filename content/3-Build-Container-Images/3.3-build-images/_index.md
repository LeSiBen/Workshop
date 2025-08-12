---
title: "3.3 Build Images"
date: 2024-01-01
weight: 3
---

# 3.3 Build Container Images

## Build Standard Image

```bash
# Build standard version
docker build -t lambda-standard .

# Check image size
docker images lambda-standard
```

## Build Optimized Image

```bash
# Build optimized version
docker build -f Dockerfile.optimized -t lambda-optimized .

# Compare sizes
docker images | grep lambda
```

## Test Images Locally

```bash
# Run standard container
docker run -p 9000:8080 lambda-standard

# Test function
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" \
  -d '{"test": "data"}'
```

## Troubleshooting

- Kiểm tra Dockerfile syntax
- Verify base image availability
- Check build context