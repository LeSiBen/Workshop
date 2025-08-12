---
title: "7.2 Optimize Costs"
date: 2024-01-01
weight: 2
---

# 7.2 Optimize Costs

## Memory Optimization

```python
def find_optimal_memory(function_name):
    lambda_client = boto3.client('lambda')
    memory_sizes = [128, 256, 512, 1024, 1536, 2048, 3008]
    results = []
    
    for memory in memory_sizes:
        # Update function memory
        lambda_client.update_function_configuration(
            FunctionName=function_name,
            MemorySize=memory
        )
        
        # Wait for update
        time.sleep(10)
        
        # Test performance
        start_time = time.time()
        response = lambda_client.invoke(
            FunctionName=function_name,
            Payload=json.dumps({'test': 'memory_optimization'})
        )
        duration = time.time() - start_time
        
        # Calculate cost
        gb_seconds = (memory / 1024) * (duration)
        cost = gb_seconds * 0.0000166667
        
        results.append({
            'memory': memory,
            'duration': duration,
            'cost': cost,
            'cost_per_invocation': cost
        })
    
    return results
```

## Provisioned Concurrency Optimization

```hcl
# Only use for predictable workloads
resource "aws_lambda_provisioned_concurrency_config" "optimized" {
  count                             = var.enable_provisioned_concurrency ? 1 : 0
  function_name                     = aws_lambda_function.lambda_optimized.function_name
  provisioned_concurrent_executions = var.provisioned_concurrency_count
  qualifier                         = aws_lambda_function.lambda_optimized.version
}
```

## Cost Optimization Strategies

### 1. Right-size Memory
- Test different memory configurations
- Find sweet spot for price/performance
- Monitor actual memory usage

### 2. Optimize Duration
- Reduce cold start time
- Optimize code efficiency
- Use connection pooling

### 3. Smart Provisioning
- Use provisioned concurrency only when needed
- Monitor utilization patterns
- Scale based on demand

### 4. Image Optimization
- Minimize image size
- Use multi-stage builds
- Remove unnecessary dependencies

## Cost Monitoring

```hcl
resource "aws_budgets_budget" "lambda_budget" {
  name         = "lambda-container-budget"
  budget_type  = "COST"
  limit_amount = "10"
  limit_unit   = "USD"
  time_unit    = "MONTHLY"
  
  cost_filters = {
    Service = ["Amazon Elastic Container Registry (ECR)", "AWS Lambda"]
  }
}
```