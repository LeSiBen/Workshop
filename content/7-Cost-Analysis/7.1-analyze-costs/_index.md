---
title: "7.1 Analyze Current Costs"
date: 2024-01-01
weight: 1
---

# 7.1 Analyze Current Costs

## Cost Components

### Lambda Costs
- **Requests**: $0.20 per 1M requests
- **Duration**: $0.0000166667 per GB-second
- **Provisioned Concurrency**: $0.0000041667 per GB-second

### ECR Costs
- **Storage**: $0.10 per GB per month
- **Data Transfer**: $0.09 per GB

## Cost Analysis Script

```python
import boto3
from datetime import datetime, timedelta

def analyze_lambda_costs(function_name, days=30):
    cloudwatch = boto3.client('cloudwatch')
    
    end_time = datetime.utcnow()
    start_time = end_time - timedelta(days=days)
    
    # Get invocation count
    invocations = cloudwatch.get_metric_statistics(
        Namespace='AWS/Lambda',
        MetricName='Invocations',
        Dimensions=[{'Name': 'FunctionName', 'Value': function_name}],
        StartTime=start_time,
        EndTime=end_time,
        Period=86400,
        Statistics=['Sum']
    )
    
    # Get duration
    duration = cloudwatch.get_metric_statistics(
        Namespace='AWS/Lambda',
        MetricName='Duration',
        Dimensions=[{'Name': 'FunctionName', 'Value': function_name}],
        StartTime=start_time,
        EndTime=end_time,
        Period=86400,
        Statistics=['Average']
    )
    
    total_invocations = sum([point['Sum'] for point in invocations['Datapoints']])
    avg_duration = sum([point['Average'] for point in duration['Datapoints']]) / len(duration['Datapoints'])
    
    # Calculate costs
    request_cost = (total_invocations / 1000000) * 0.20
    duration_cost = (total_invocations * avg_duration / 1000) * (512 / 1024) * 0.0000166667
    
    return {
        'function_name': function_name,
        'total_invocations': total_invocations,
        'avg_duration_ms': avg_duration,
        'request_cost': request_cost,
        'duration_cost': duration_cost,
        'total_cost': request_cost + duration_cost
    }
```

## Cost Comparison

| Function | Invocations | Avg Duration | Request Cost | Duration Cost | Total |
|----------|-------------|--------------|--------------|---------------|-------|
| Standard | 10,000 | 3000ms | $0.002 | $0.025 | $0.027 |
| Optimized | 10,000 | 1500ms | $0.002 | $0.013 | $0.015 |

## Savings Analysis

- **44% cost reduction** with optimized version
- Primarily from reduced duration costs
- Additional savings from faster cold starts