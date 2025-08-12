---
title: "5.1 Test Cold Start Performance"
date: 2024-01-01
weight: 1
---

# 5.1 Test Cold Start Performance

## Cold Start Testing Script

```python
import boto3
import time
import json
from datetime import datetime

def test_cold_start(function_name, iterations=10):
    lambda_client = boto3.client('lambda')
    results = []
    
    for i in range(iterations):
        # Wait to ensure cold start
        time.sleep(60)
        
        start_time = time.time()
        
        response = lambda_client.invoke(
            FunctionName=function_name,
            Payload=json.dumps({'test': f'cold_start_{i}'})
        )
        
        end_time = time.time()
        duration = (end_time - start_time) * 1000
        
        results.append({
            'iteration': i + 1,
            'duration_ms': duration,
            'timestamp': datetime.now().isoformat()
        })
        
        print(f"Cold start {i+1}: {duration:.2f}ms")
    
    return results
```

## Run Tests

```bash
# Test standard function
python test_cold_start.py lambda-container-standard

# Test optimized function  
python test_cold_start.py lambda-container-optimized
```

## Analyze Results

- Average cold start time
- Standard deviation
- Min/Max values
- Performance comparison