---
title: "5.2 Test Warm Start Performance"
date: 2024-01-01
weight: 2
---

# 5.2 Test Warm Start Performance

## Warm Start Testing Script

```python
import boto3
import time
import json
from datetime import datetime

def test_warm_start(function_name, iterations=20):
    lambda_client = boto3.client('lambda')
    results = []
    
    # Initial invocation to warm up
    lambda_client.invoke(
        FunctionName=function_name,
        Payload=json.dumps({'warmup': True})
    )
    
    for i in range(iterations):
        start_time = time.time()
        
        response = lambda_client.invoke(
            FunctionName=function_name,
            Payload=json.dumps({'test': f'warm_start_{i}'})
        )
        
        end_time = time.time()
        duration = (end_time - start_time) * 1000
        
        results.append({
            'iteration': i + 1,
            'duration_ms': duration,
            'timestamp': datetime.now().isoformat()
        })
        
        print(f"Warm start {i+1}: {duration:.2f}ms")
        
        # Small delay between invocations
        time.sleep(1)
    
    return results
```

## Run Warm Tests

```bash
# Test both functions
python test_warm_start.py lambda-container-standard
python test_warm_start.py lambda-container-optimized
```

## Performance Metrics

- Consistent execution time
- Lower latency than cold starts
- Throughput measurements