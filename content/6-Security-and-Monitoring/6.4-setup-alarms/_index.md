---
title: "6.4 Setup Alarms"
date: 2024-01-01
weight: 4
---

# 6.4 Setup CloudWatch Alarms

## Error Rate Alarm

```hcl
resource "aws_cloudwatch_metric_alarm" "lambda_error_rate" {
  alarm_name          = "lambda-container-error-rate"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "2"
  metric_name         = "Errors"
  namespace           = "AWS/Lambda"
  period              = "300"
  statistic           = "Sum"
  threshold           = "5"
  alarm_description   = "This metric monitors lambda error rate"
  alarm_actions       = [aws_sns_topic.alerts.arn]

  dimensions = {
    FunctionName = "lambda-container-optimized"
  }
}
```

## Duration Alarm

```hcl
resource "aws_cloudwatch_metric_alarm" "lambda_duration" {
  alarm_name          = "lambda-container-duration"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "2"
  metric_name         = "Duration"
  namespace           = "AWS/Lambda"
  period              = "300"
  statistic           = "Average"
  threshold           = "10000"
  alarm_description   = "This metric monitors lambda duration"
  alarm_actions       = [aws_sns_topic.alerts.arn]

  dimensions = {
    FunctionName = "lambda-container-optimized"
  }
}
```

## SNS Topic for Alerts

```hcl
resource "aws_sns_topic" "alerts" {
  name = "lambda-container-alerts"
}

resource "aws_sns_topic_subscription" "email_alerts" {
  topic_arn = aws_sns_topic.alerts.arn
  protocol  = "email"
  endpoint  = "your-email@example.com"
}
```

## Alarm Types

- **Error Rate**: Monitor function errors
- **Duration**: Track execution time
- **Throttles**: Detect throttling events
- **Memory Usage**: Monitor memory consumption
- **Cold Starts**: Track initialization time