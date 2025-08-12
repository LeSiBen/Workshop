---
title: "6.3 Create Dashboard"
date: 2024-01-01
weight: 3
---

# 6.3 Create CloudWatch Dashboard

## Dashboard Configuration

```hcl
resource "aws_cloudwatch_dashboard" "lambda_container_dashboard" {
  dashboard_name = "Lambda-Container-Workshop"

  dashboard_body = jsonencode({
    widgets = [
      {
        type   = "metric"
        x      = 0
        y      = 0
        width  = 12
        height = 6

        properties = {
          metrics = [
            ["AWS/Lambda", "Duration", "FunctionName", "lambda-container-standard"],
            [".", ".", ".", "lambda-container-optimized"]
          ]
          view    = "timeSeries"
          stacked = false
          region  = "us-east-1"
          title   = "Function Duration"
          period  = 300
        }
      },
      {
        type   = "metric"
        x      = 0
        y      = 6
        width  = 12
        height = 6

        properties = {
          metrics = [
            ["AWS/Lambda", "Invocations", "FunctionName", "lambda-container-standard"],
            [".", ".", ".", "lambda-container-optimized"]
          ]
          view    = "timeSeries"
          stacked = false
          region  = "us-east-1"
          title   = "Function Invocations"
          period  = 300
        }
      }
    ]
  })
}
```

## Key Metrics to Monitor

- **Duration**: Function execution time
- **Invocations**: Number of function calls
- **Errors**: Error count and rate
- **Throttles**: Throttling events
- **Cold Starts**: Container initialization
- **Memory Usage**: Memory utilization

## Dashboard Widgets

- Time series charts
- Number widgets
- Log widgets
- Custom metrics
- Alarms status