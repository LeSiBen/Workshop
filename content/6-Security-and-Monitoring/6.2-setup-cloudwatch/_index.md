---
title: "6.2 Setup CloudWatch"
date: 2024-01-01
weight: 2
---

# 6.2 Setup CloudWatch

## CloudWatch Log Groups

```hcl
resource "aws_cloudwatch_log_group" "lambda_standard_logs" {
  name              = "/aws/lambda/lambda-container-standard"
  retention_in_days = 7
  
  tags = {
    Environment = "workshop"
    Function    = "standard"
  }
}

resource "aws_cloudwatch_log_group" "lambda_optimized_logs" {
  name              = "/aws/lambda/lambda-container-optimized"
  retention_in_days = 7
  
  tags = {
    Environment = "workshop"
    Function    = "optimized"
  }
}
```

## Custom Metrics

```python
import boto3
from datetime import datetime

def publish_custom_metrics(metric_name, value, unit='Count'):
    cloudwatch = boto3.client('cloudwatch')
    
    cloudwatch.put_metric_data(
        Namespace='Lambda/Container/Workshop',
        MetricData=[
            {
                'MetricName': metric_name,
                'Value': value,
                'Unit': unit,
                'Timestamp': datetime.utcnow()
            }
        ]
    )
```

## Log Insights Queries

```sql
-- Find errors
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20

-- Performance analysis
fields @timestamp, @duration
| filter @type = "REPORT"
| stats avg(@duration), max(@duration), min(@duration) by bin(5m)
```

## Monitoring Setup

- Enable detailed monitoring
- Set up log retention policies
- Create custom metrics
- Configure log insights queries