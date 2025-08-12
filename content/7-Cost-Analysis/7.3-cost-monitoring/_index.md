---
title: "7.3 Cost Monitoring"
date: 2024-01-01
weight: 3
---

# 7.3 Cost Monitoring

## AWS Cost Explorer

```bash
# Get cost data via CLI
aws ce get-cost-and-usage \
    --time-period Start=2024-01-01,End=2024-01-31 \
    --granularity MONTHLY \
    --metrics BlendedCost \
    --group-by Type=DIMENSION,Key=SERVICE
```

## Cost Allocation Tags

```hcl
resource "aws_lambda_function" "lambda_optimized" {
  # ... other configuration ...
  
  tags = {
    Project     = "lambda-container-workshop"
    Environment = "workshop"
    CostCenter  = "engineering"
    Owner       = "workshop-participant"
  }
}
```

## Billing Alerts

```hcl
resource "aws_cloudwatch_metric_alarm" "billing_alarm" {
  alarm_name          = "lambda-container-billing-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "1"
  metric_name         = "EstimatedCharges"
  namespace           = "AWS/Billing"
  period              = "86400"
  statistic           = "Maximum"
  threshold           = "5.00"
  alarm_description   = "This metric monitors estimated charges"
  alarm_actions       = [aws_sns_topic.billing_alerts.arn]

  dimensions = {
    Currency = "USD"
  }
}
```

## Cost Dashboard

```python
import boto3
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

def create_cost_dashboard():
    ce = boto3.client('ce')
    
    end_date = datetime.now().strftime('%Y-%m-%d')
    start_date = (datetime.now() - timedelta(days=30)).strftime('%Y-%m-%d')
    
    response = ce.get_cost_and_usage(
        TimePeriod={
            'Start': start_date,
            'End': end_date
        },
        Granularity='DAILY',
        Metrics=['BlendedCost'],
        GroupBy=[
            {
                'Type': 'DIMENSION',
                'Key': 'SERVICE'
            }
        ]
    )
    
    # Process and visualize data
    dates = []
    lambda_costs = []
    ecr_costs = []
    
    for result in response['ResultsByTime']:
        dates.append(result['TimePeriod']['Start'])
        
        lambda_cost = 0
        ecr_cost = 0
        
        for group in result['Groups']:
            service = group['Keys'][0]
            cost = float(group['Metrics']['BlendedCost']['Amount'])
            
            if 'Lambda' in service:
                lambda_cost += cost
            elif 'ECR' in service:
                ecr_cost += cost
        
        lambda_costs.append(lambda_cost)
        ecr_costs.append(ecr_cost)
    
    # Create visualization
    plt.figure(figsize=(12, 6))
    plt.plot(dates, lambda_costs, label='Lambda Costs', marker='o')
    plt.plot(dates, ecr_costs, label='ECR Costs', marker='s')
    plt.xlabel('Date')
    plt.ylabel('Cost (USD)')
    plt.title('Daily AWS Costs - Lambda Container Workshop')
    plt.legend()
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.savefig('cost_dashboard.png')
    plt.show()
```

## Cost Optimization Recommendations

- Set up billing alerts
- Use cost allocation tags
- Monitor usage patterns
- Regular cost reviews
- Implement automated cost controls