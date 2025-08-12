---
title: "Create Lambda Function"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>3.1 </b>"
---

# Create Lambda Function

## Tổng quan

Bước đầu tiên là tạo Lambda function code với proper error handling, logging, và performance monitoring. Function này sẽ được package thành container image.

### ⏱️ **Thời gian ước tính**: 15-20 phút

## Bước 1: Create Lambda Handler

### **Bước 1.1: Navigate to Source Directory**
```bash
cd lambda-container-workshop/src
```

### **Bước 1.2: Create Main Lambda Function**
```python
# Create app.py
cat > app.py << 'EOF'
import json
import time
import os
import logging
from datetime import datetime
from typing import Dict, Any

# Configure logging
logger = logging.getLogger()
logger.setLevel(logging.INFO)

# Global variables (initialized once per container)
COLD_START = True
CONTAINER_ID = os.environ.get('AWS_LAMBDA_LOG_STREAM_NAME', 'local')

def lambda_handler(event: Dict[str, Any], context: Any) -> Dict[str, Any]:
    """
    Main Lambda handler function
    
    Args:
        event: Lambda event data
        context: Lambda context object
        
    Returns:
        Dict containing response data
    """
    global COLD_START
    
    # Record start time for performance measurement
    start_time = time.time()
    
    # Log cold start information
    if COLD_START:
        logger.info(f"Cold start - Container ID: {CONTAINER_ID}")
        COLD_START = False
    else:
        logger.info("Warm start - Reusing existing container")
    
    try:
        # Extract request information
        request_id = context.aws_request_id if hasattr(context, 'aws_request_id') else 'local-test'
        function_name = context.function_name if hasattr(context, 'function_name') else 'local-function'
        memory_limit = context.memory_limit_in_mb if hasattr(context, 'memory_limit_in_mb') else 512
        
        # Process the event
        processed_data = process_event(event)
        
        # Calculate processing time
        processing_time = (time.time() - start_time) * 1000  # Convert to milliseconds
        
        # Prepare response
        response = {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json',
                'X-Request-ID': request_id
            },
            'body': json.dumps({
                'message': 'Lambda Container Image Workshop - Function executed successfully',
                'timestamp': datetime.utcnow().isoformat() + 'Z',
                'request_id': request_id,
                'function_name': function_name,
                'memory_limit_mb': memory_limit,
                'processing_time_ms': round(processing_time, 2),
                'container_id': CONTAINER_ID,
                'cold_start': COLD_START,
                'event_data': processed_data,
                'environment': {
                    'aws_region': os.environ.get('AWS_REGION', 'unknown'),
                    'aws_execution_env': os.environ.get('AWS_EXECUTION_ENV', 'local'),
                    'lambda_runtime_dir': os.environ.get('LAMBDA_RUNTIME_DIR', 'local')
                }
            })
        }
        
        # Log successful execution
        logger.info(f"Function executed successfully in {processing_time:.2f}ms")
        
        return response
        
    except Exception as e:
        # Handle errors gracefully
        error_time = (time.time() - start_time) * 1000
        
        logger.error(f"Function execution failed after {error_time:.2f}ms: {str(e)}")
        
        return {
            'statusCode': 500,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': json.dumps({
                'error': 'Internal server error',
                'message': str(e),
                'timestamp': datetime.utcnow().isoformat() + 'Z',
                'processing_time_ms': round(error_time, 2)
            })
        }

def process_event(event: Dict[str, Any]) -> Dict[str, Any]:
    """
    Process the incoming event data
    
    Args:
        event: Raw event data
        
    Returns:
        Processed event data
    """
    # Simulate some processing work
    time.sleep(0.01)  # 10ms of "work"
    
    processed = {
        'event_type': determine_event_type(event),
        'event_size': len(json.dumps(event)),
        'processed_at': datetime.utcnow().isoformat() + 'Z'
    }
    
    # Add specific processing based on event type
    if 'Records' in event:
        processed['record_count'] = len(event['Records'])
    elif 'httpMethod' in event:
        processed['http_method'] = event['httpMethod']
        processed['path'] = event.get('path', '/')
    elif 'source' in event:
        processed['event_source'] = event['source']
    
    return processed

def determine_event_type(event: Dict[str, Any]) -> str:
    """
    Determine the type of Lambda event
    
    Args:
        event: Lambda event data
        
    Returns:
        String describing event type
    """
    if 'Records' in event:
        if event['Records'] and 'eventSource' in event['Records'][0]:
            return f"AWS Service Event - {event['Records'][0]['eventSource']}"
        return "Records Event"
    elif 'httpMethod' in event:
        return "API Gateway Event"
    elif 'source' in event:
        return f"EventBridge Event - {event['source']}"
    elif 'test' in event:
        return "Test Event"
    else:
        return "Unknown Event Type"

def health_check(event: Dict[str, Any], context: Any) -> Dict[str, Any]:
    """
    Health check endpoint for monitoring
    
    Args:
        event: Lambda event data
        context: Lambda context object
        
    Returns:
        Health status response
    """
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/json'
        },
        'body': json.dumps({
            'status': 'healthy',
            'timestamp': datetime.utcnow().isoformat() + 'Z',
            'container_id': CONTAINER_ID,
            'uptime_info': 'Container is running normally'
        })
    }

# For local testing
if __name__ == "__main__":
    # Test event
    test_event = {
        'test': True,
        'message': 'Local test execution',
        'data': {
            'key1': 'value1',
            'key2': 'value2'
        }
    }
    
    # Mock context for local testing
    class MockContext:
        def __init__(self):
            self.aws_request_id = 'local-test-request-id'
            self.function_name = 'local-test-function'
            self.memory_limit_in_mb = 512
    
    # Execute function locally
    result = lambda_handler(test_event, MockContext())
    print(json.dumps(result, indent=2))
EOF
```

## Bước 2: Create Requirements File

### **Bước 2.1: Create requirements.txt**
```bash
cat > requirements.txt << 'EOF'
# Core dependencies for Lambda function
boto3==1.34.0
botocore==1.34.0

# Additional utilities
requests==2.31.0
python-dateutil==2.8.2

# Development and testing (will be excluded in optimized build)
pytest==7.4.0
pytest-mock==3.11.1
moto==4.2.0
EOF
```

### **Bước 2.2: Create Production Requirements**
```bash
cat > requirements-prod.txt << 'EOF'
# Production-only dependencies (minimal set)
boto3==1.34.0
botocore==1.34.0
requests==2.31.0
python-dateutil==2.8.2
EOF
```

## Bước 3: Create Test Files

### **Bước 3.1: Create Test Events**
```bash
mkdir test-events

# API Gateway test event
cat > test-events/api-gateway.json << 'EOF'
{
  "httpMethod": "GET",
  "path": "/test",
  "headers": {
    "Accept": "application/json",
    "User-Agent": "Workshop-Test/1.0"
  },
  "queryStringParameters": {
    "param1": "value1"
  },
  "body": null,
  "isBase64Encoded": false
}
EOF

# S3 test event
cat > test-events/s3-event.json << 'EOF'
{
  "Records": [
    {
      "eventVersion": "2.1",
      "eventSource": "aws:s3",
      "eventName": "ObjectCreated:Put",
      "s3": {
        "bucket": {
          "name": "test-bucket"
        },
        "object": {
          "key": "test-object.txt"
        }
      }
    }
  ]
}
EOF

# Simple test event
cat > test-events/simple-test.json << 'EOF'
{
  "test": true,
  "message": "Performance test event",
  "timestamp": "2024-01-01T00:00:00Z",
  "data": {
    "iteration": 1,
    "test_type": "performance"
  }
}
EOF
```

### **Bước 3.2: Create Unit Tests**
```python
cat > test_app.py << 'EOF'
import json
import pytest
from unittest.mock import Mock
from app import lambda_handler, process_event, determine_event_type, health_check

class TestLambdaHandler:
    def test_lambda_handler_success(self):
        """Test successful lambda handler execution"""
        event = {"test": True, "message": "test"}
        context = Mock()
        context.aws_request_id = "test-request-id"
        context.function_name = "test-function"
        context.memory_limit_in_mb = 512
        
        response = lambda_handler(event, context)
        
        assert response['statusCode'] == 200
        body = json.loads(response['body'])
        assert body['message'] == 'Lambda Container Image Workshop - Function executed successfully'
        assert 'processing_time_ms' in body
        assert body['request_id'] == "test-request-id"

    def test_lambda_handler_error(self):
        """Test lambda handler error handling"""
        # Create an event that will cause an error
        event = None  # This should cause an error
        context = Mock()
        
        response = lambda_handler(event, context)
        
        assert response['statusCode'] == 500
        body = json.loads(response['body'])
        assert 'error' in body

    def test_process_event(self):
        """Test event processing"""
        event = {"test": True, "data": "sample"}
        
        result = process_event(event)
        
        assert 'event_type' in result
        assert 'event_size' in result
        assert 'processed_at' in result

    def test_determine_event_type(self):
        """Test event type determination"""
        # Test API Gateway event
        api_event = {"httpMethod": "GET", "path": "/test"}
        assert determine_event_type(api_event) == "API Gateway Event"
        
        # Test S3 event
        s3_event = {"Records": [{"eventSource": "aws:s3"}]}
        assert determine_event_type(s3_event) == "AWS Service Event - aws:s3"
        
        # Test unknown event
        unknown_event = {"unknown": "data"}
        assert determine_event_type(unknown_event) == "Unknown Event Type"

    def test_health_check(self):
        """Test health check endpoint"""
        event = {}
        context = Mock()
        
        response = health_check(event, context)
        
        assert response['statusCode'] == 200
        body = json.loads(response['body'])
        assert body['status'] == 'healthy'

if __name__ == "__main__":
    pytest.main([__file__])
EOF
```

## Bước 4: Local Testing

### **Bước 4.1: Test Function Locally**
```bash
# Run the function directly
python app.py

# Expected output: JSON response with function execution details
```

### **Bước 4.2: Run Unit Tests**
```bash
# Install test dependencies
pip install pytest pytest-mock

# Run tests
python -m pytest test_app.py -v

# Expected output: All tests should pass
```

### **Bước 4.3: Test with Different Events**
```bash
# Test with API Gateway event
python -c "
import json
from app import lambda_handler
from unittest.mock import Mock

with open('test-events/api-gateway.json') as f:
    event = json.load(f)

context = Mock()
context.aws_request_id = 'test-api-request'
context.function_name = 'test-function'
context.memory_limit_in_mb = 512

result = lambda_handler(event, context)
print(json.dumps(result, indent=2))
"
```

## Bước 5: Performance Baseline

### **Bước 5.1: Create Performance Test**
```python
cat > performance_test.py << 'EOF'
import time
import json
import statistics
from app import lambda_handler
from unittest.mock import Mock

def run_performance_test(iterations=10):
    """Run performance test with multiple iterations"""
    
    test_event = {
        "test": True,
        "message": "Performance test",
        "data": {"iteration": 0}
    }
    
    context = Mock()
    context.aws_request_id = "perf-test-request"
    context.function_name = "perf-test-function"
    context.memory_limit_in_mb = 512
    
    execution_times = []
    
    print(f"Running {iterations} iterations...")
    
    for i in range(iterations):
        test_event["data"]["iteration"] = i
        
        start_time = time.time()
        response = lambda_handler(test_event, context)
        end_time = time.time()
        
        execution_time = (end_time - start_time) * 1000  # Convert to ms
        execution_times.append(execution_time)
        
        # Parse response to get internal processing time
        body = json.loads(response['body'])
        internal_time = body.get('processing_time_ms', 0)
        
        print(f"Iteration {i+1}: {execution_time:.2f}ms total, {internal_time:.2f}ms internal")
    
    # Calculate statistics
    avg_time = statistics.mean(execution_times)
    min_time = min(execution_times)
    max_time = max(execution_times)
    median_time = statistics.median(execution_times)
    
    print(f"\nPerformance Summary:")
    print(f"Average: {avg_time:.2f}ms")
    print(f"Minimum: {min_time:.2f}ms")
    print(f"Maximum: {max_time:.2f}ms")
    print(f"Median:  {median_time:.2f}ms")
    
    return {
        'average': avg_time,
        'minimum': min_time,
        'maximum': max_time,
        'median': median_time,
        'iterations': iterations
    }

if __name__ == "__main__":
    results = run_performance_test(20)
    print(f"\nBaseline performance established: {results['average']:.2f}ms average")
EOF

# Run performance test
python performance_test.py
```

## Verification

### **Checklist**
- [ ] `app.py` created với comprehensive error handling
- [ ] `requirements.txt` và `requirements-prod.txt` created
- [ ] Test events created for different scenarios
- [ ] Unit tests pass successfully
- [ ] Local execution works without errors
- [ ] Performance baseline established
- [ ] Function handles various event types

### **Expected Results**
- Function executes in < 50ms locally
- All unit tests pass
- Proper JSON responses for all event types
- Error handling works correctly
- Performance metrics collected

Ready for [Create Dockerfile](../3.2-create-dockerfile/)!