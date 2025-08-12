---
title: "Giới thiệu Workshop"
date: "`r Sys.Date()`"
weight: 1
chapter: true
pre: "<b>1. </b>"
---

# Giới thiệu Lambda Container Images Workshop

## Tại sao Lambda Container Images?

### Giới hạn của Lambda ZIP Packages

Trước khi có Container Images, Lambda functions chỉ có thể deploy bằng ZIP packages với những giới hạn:

#### 📦 **ZIP Package Limitations**
- **Maximum size**: 250MB (uncompressed)
- **Runtime restrictions**: Chỉ supported runtimes
- **Dependency management**: Phức tạp với large libraries
- **Layer limitations**: Maximum 5 layers, 250MB total
- **Cold start**: Không optimize được base runtime

```bash
# Ví dụ ZIP package structure (limited)
my-function.zip
├── lambda_function.py          # 10KB
├── requests/                   # 500KB
├── numpy/                      # 15MB
├── pandas/                     # 25MB
├── scikit-learn/              # 50MB
└── other-dependencies/         # 159MB
# Total: ~250MB (at limit!)
```

### Container Images Advantages

#### 🐳 **Container Benefits**
- **Maximum size**: 10GB (40x larger!)
- **Any runtime**: Custom runtimes, any language
- **Familiar tooling**: Docker, existing CI/CD
- **Layer caching**: Faster builds và deployments
- **Consistency**: Same image dev → prod

```dockerfile
# Ví dụ Container approach (flexible)
FROM public.ecr.aws/lambda/python:3.11

# Install large ML libraries
RUN pip install \
    numpy==1.24.0 \
    pandas==1.5.0 \
    scikit-learn==1.2.0 \
    tensorflow==2.11.0 \
    torch==1.13.0
    # Total: 2GB+ (no problem!)

COPY app.py ${LAMBDA_TASK_ROOT}
CMD ["app.lambda_handler"]
```

## Workshop Architecture Deep Dive

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Developer Workstation                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Source    │  │   Docker    │  │     Git     │             │
│  │    Code     │  │   Desktop   │  │  Repository │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                        CI/CD Pipeline                           │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   GitHub    │  │   Build     │  │   Security  │             │
│  │   Actions   │  │   Images    │  │   Scanning  │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                         AWS Cloud                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Amazon    │  │   AWS       │  │ CloudWatch  │             │
│  │     ECR     │  │   Lambda    │  │ Monitoring  │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
└─────────────────────────────────────────────────────────────────┘
```

### Detailed Component Breakdown

#### 🏗️ **Development Layer**
1. **Source Code Management**
   - Python Lambda function code
   - Dockerfile configurations
   - Terraform infrastructure code
   - GitHub repository với version control

2. **Local Development Tools**
   - Docker Desktop cho container development
   - AWS CLI cho cloud interaction
   - Terraform cho infrastructure management
   - VS Code với AWS extensions

#### 🔄 **CI/CD Pipeline Layer**
1. **Automated Build Process**
   - GitHub Actions workflows
   - Multi-stage Docker builds
   - Image optimization techniques
   - Automated testing

2. **Security Integration**
   - Container vulnerability scanning
   - Secrets management
   - Code quality checks
   - Compliance validation

#### ☁️ **AWS Cloud Layer**
1. **Container Registry (ECR)**
   - Private Docker registry
   - Image vulnerability scanning
   - Lifecycle policies
   - Cross-region replication

2. **Compute Layer (Lambda)**
   - Container-based functions
   - Auto-scaling capabilities
   - Event-driven execution
   - Performance monitoring

3. **Monitoring Layer (CloudWatch)**
   - Real-time metrics
   - Custom dashboards
   - Automated alerting
   - Cost tracking

## Performance Optimization Strategies

### Cold Start Optimization

#### 🥶 **Understanding Cold Starts**
Cold start xảy ra khi:
- Function chưa được invoke trong 15+ phút
- Concurrency scaling up
- Code hoặc configuration thay đổi

```python
# Example: Cold start timeline
import time
import json

def lambda_handler(event, context):
    # Cold start: 0-2000ms (container init)
    start_time = time.time()
    
    # Your business logic: 10-100ms
    result = process_data(event)
    
    # Total time: Cold start + execution
    total_time = time.time() - start_time
    
    return {
        'statusCode': 200,
        'body': json.dumps({
            'result': result,
            'execution_time': total_time
        })
    }
```

#### ⚡ **Optimization Techniques**
1. **Multi-stage Docker builds**
2. **Minimal base images**
3. **Dependency optimization**
4. **Code initialization patterns**
5. **Memory allocation tuning**

### Image Size Optimization

#### 📏 **Size Impact on Performance**
- **Smaller images** = Faster cold starts
- **Layer caching** = Faster deployments
- **Network transfer** = Reduced latency

```dockerfile
# Before optimization (500MB+)
FROM python:3.11
RUN apt-get update && apt-get install -y \
    gcc g++ make cmake \
    libssl-dev libffi-dev \
    python3-dev
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["app.lambda_handler"]

# After optimization (150MB)
FROM public.ecr.aws/lambda/python:3.11 as builder
COPY requirements.txt .
RUN pip install -r requirements.txt -t /var/task

FROM public.ecr.aws/lambda/python:3.11
COPY --from=builder /var/task /var/task
COPY app.py /var/task
CMD ["app.lambda_handler"]
```

## Security Best Practices

### Container Security Model

#### 🔒 **Security Layers**
1. **Base Image Security**
   - Use official AWS base images
   - Regular security updates
   - Minimal attack surface

2. **Dependency Management**
   - Vulnerability scanning
   - Version pinning
   - License compliance

3. **Runtime Security**
   - No root privileges
   - Read-only filesystem
   - Network isolation

#### 🛡️ **Scanning Integration**
```yaml
# GitHub Actions security scanning
name: Security Scan
on: [push, pull_request]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build image
        run: docker build -t my-lambda .
        
      - name: Run Trivy scanner
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'my-lambda'
          format: 'sarif'
          output: 'trivy-results.sarif'
          
      - name: Upload to GitHub Security
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: 'trivy-results.sarif'
```

## Cost Optimization Framework

### Cost Components

#### 💰 **Lambda Pricing Model**
1. **Request charges**: $0.20 per 1M requests
2. **Duration charges**: $0.0000166667 per GB-second
3. **Provisioned concurrency**: $0.0000097222 per GB-second

#### 📊 **Cost Optimization Strategies**
```python
# Example: Cost calculation
def calculate_lambda_cost(
    requests_per_month: int,
    avg_duration_ms: int,
    memory_mb: int
) -> dict:
    
    # Request cost
    request_cost = (requests_per_month / 1_000_000) * 0.20
    
    # Duration cost
    gb_seconds = (memory_mb / 1024) * (avg_duration_ms / 1000) * requests_per_month
    duration_cost = gb_seconds * 0.0000166667
    
    total_cost = request_cost + duration_cost
    
    return {
        'request_cost': request_cost,
        'duration_cost': duration_cost,
        'total_cost': total_cost,
        'cost_per_request': total_cost / requests_per_month
    }

# Example calculation
standard_cost = calculate_lambda_cost(
    requests_per_month=1_000_000,
    avg_duration_ms=2810,  # Before optimization
    memory_mb=512
)

optimized_cost = calculate_lambda_cost(
    requests_per_month=1_000_000,
    avg_duration_ms=1240,  # After optimization
    memory_mb=512
)

savings = standard_cost['total_cost'] - optimized_cost['total_cost']
savings_percentage = (savings / standard_cost['total_cost']) * 100

print(f"Monthly savings: ${savings:.2f} ({savings_percentage:.1f}%)")
```

## Workshop Learning Outcomes

### Technical Skills

#### 🎯 **Primary Skills**
1. **Container Development**
   - Multi-stage Dockerfile creation
   - Image optimization techniques
   - Layer caching strategies

2. **AWS Lambda Expertise**
   - Container image deployment
   - Performance tuning
   - Event-driven architecture

3. **Infrastructure as Code**
   - Terraform best practices
   - Resource management
   - State management

4. **DevOps Integration**
   - CI/CD pipeline setup
   - Automated testing
   - Security scanning

#### 🔧 **Secondary Skills**
1. **Monitoring và Observability**
   - CloudWatch metrics
   - Custom dashboards
   - Alerting strategies

2. **Cost Management**
   - Cost analysis techniques
   - Optimization strategies
   - Budget monitoring

3. **Security Practices**
   - Vulnerability management
   - Compliance checking
   - Access control

### Business Value

#### 💼 **Organizational Benefits**
1. **Reduced Operational Costs**
   - 50%+ reduction in compute costs
   - Faster development cycles
   - Improved resource utilization

2. **Enhanced Security Posture**
   - Automated vulnerability scanning
   - Compliance adherence
   - Risk mitigation

3. **Improved Developer Productivity**
   - Familiar Docker tooling
   - Consistent environments
   - Faster deployment cycles

## Next Steps

Bây giờ bạn đã hiểu về Lambda Container Images và lợi ích của chúng, hãy tiếp tục với [Chuẩn bị môi trường](../2-prerequisites/) để setup các tools cần thiết cho workshop.

### What's Coming Next

1. **Environment Setup** (45-60 phút)
   - AWS account configuration
   - Tool installation và verification
   - Project structure setup

2. **Hands-on Development** (2-3 giờ)
   - Container image creation
   - Performance optimization
   - Security implementation
   - Cost analysis

3. **Production Readiness** (30-45 phút)
   - Monitoring setup
   - CI/CD integration
   - Best practices review

---

**🚀 Ready to start?** Let's move to [Prerequisites](../2-prerequisites/) to set up your development environment!