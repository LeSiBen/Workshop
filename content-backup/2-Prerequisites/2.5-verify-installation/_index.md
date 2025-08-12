---
title: "Verify Installation"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b>2.5 </b>"
---

# Verify Installation

## Tổng quan

Final step trong prerequisites là comprehensive verification của tất cả tools và configurations. Section này sẽ run thorough tests để ensure workshop environment ready.

### ⏱️ **Thời gian ước tính**: 10-15 phút

## Bước 1: Individual Tool Verification

### **AWS CLI Verification**
```bash
# Version check
aws --version

# Identity check
aws sts get-caller-identity

# Service access check
aws lambda list-functions --region ap-southeast-1
aws ecr describe-repositories --region ap-southeast-1
```

### **Docker Verification**
```bash
# Version check
docker --version

# Functionality check
docker run --rm hello-world

# Build capability check
echo "FROM alpine:latest" | docker build -t test-image -
docker run --rm test-image echo "Docker works!"
docker rmi test-image
```

### **Terraform Verification**
```bash
# Version check
terraform --version

# Provider download test
mkdir temp-tf && cd temp-tf
cat > main.tf << EOF
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
provider "aws" {
  region = "ap-southeast-1"
}
data "aws_caller_identity" "current" {}
output "account_id" {
  value = data.aws_caller_identity.current.account_id
}
EOF

terraform init
terraform plan
cd .. && rm -rf temp-tf
```

## Bước 2: Integration Testing

### **AWS + Docker Integration**
```bash
# Test ECR login capability
aws ecr get-login-password --region ap-southeast-1 | echo "ECR login token received"

# Test Docker + AWS integration
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
echo "Account ID: $ACCOUNT_ID"
echo "ECR URI would be: $ACCOUNT_ID.dkr.ecr.ap-southeast-1.amazonaws.com"
```

### **Terraform + AWS Integration**
```bash
# Create comprehensive test
mkdir integration-test && cd integration-test

cat > test.tf << EOF
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-southeast-1"
}

data "aws_caller_identity" "current" {}
data "aws_region" "current" {}

output "account_id" {
  value = data.aws_caller_identity.current.account_id
}

output "region" {
  value = data.aws_region.current.name
}

output "user_arn" {
  value = data.aws_caller_identity.current.arn
}
EOF

terraform init
terraform plan
terraform apply -auto-approve
terraform destroy -auto-approve
cd .. && rm -rf integration-test
```

## Bước 3: Workshop Environment Setup

### **Create Workshop Directory**
```bash
# Create main workshop directory
mkdir lambda-container-workshop
cd lambda-container-workshop

# Create project structure
mkdir -p {src,docker,terraform,scripts,monitoring}

# Create initial files
touch src/app.py
touch docker/Dockerfile
touch docker/Dockerfile.optimized
touch terraform/main.tf
touch terraform/variables.tf
touch terraform/outputs.tf
touch scripts/build.sh
touch scripts/deploy.sh

# Verify structure
tree . || ls -la
```

### **Create Verification Script**
```bash
cat > verify-workshop-env.sh << 'EOF'
#!/bin/bash

echo "=== Lambda Container Workshop - Environment Verification ==="
echo

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Function to print status
print_status() {
    if [ $1 -eq 0 ]; then
        echo -e "${GREEN}✅ $2${NC}"
    else
        echo -e "${RED}❌ $2${NC}"
        return 1
    fi
}

# Function to print warning
print_warning() {
    echo -e "${YELLOW}⚠️  $1${NC}"
}

# Check AWS CLI
echo "1. Checking AWS CLI..."
aws --version > /dev/null 2>&1
print_status $? "AWS CLI installed"

aws sts get-caller-identity > /dev/null 2>&1
if [ $? -eq 0 ]; then
    ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
    print_status 0 "AWS CLI configured (Account: $ACCOUNT_ID)"
else
    print_status 1 "AWS CLI not configured properly"
    exit 1
fi

# Check Docker
echo "2. Checking Docker..."
docker --version > /dev/null 2>&1
print_status $? "Docker installed"

docker info > /dev/null 2>&1
print_status $? "Docker daemon running"

docker run --rm hello-world > /dev/null 2>&1
print_status $? "Docker functionality verified"

# Check Terraform
echo "3. Checking Terraform..."
terraform --version > /dev/null 2>&1
print_status $? "Terraform installed"

# Test Terraform with AWS
mkdir -p /tmp/tf-test
cd /tmp/tf-test
cat > main.tf << 'TFEOF'
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
provider "aws" {
  region = "ap-southeast-1"
}
data "aws_caller_identity" "current" {}
TFEOF

terraform init > /dev/null 2>&1
terraform plan > /dev/null 2>&1
if [ $? -eq 0 ]; then
    print_status 0 "Terraform + AWS integration working"
else
    print_status 1 "Terraform + AWS integration failed"
fi
cd - > /dev/null
rm -rf /tmp/tf-test

# Check service permissions
echo "4. Checking AWS service permissions..."

aws lambda list-functions --region ap-southeast-1 > /dev/null 2>&1
print_status $? "Lambda service access"

aws ecr describe-repositories --region ap-southeast-1 > /dev/null 2>&1
print_status $? "ECR service access"

aws iam get-user > /dev/null 2>&1
print_status $? "IAM service access"

aws logs describe-log-groups --region ap-southeast-1 --limit 1 > /dev/null 2>&1
print_status $? "CloudWatch Logs access"

# Check workshop directory structure
echo "5. Checking workshop environment..."
if [ -d "src" ] && [ -d "docker" ] && [ -d "terraform" ]; then
    print_status 0 "Workshop directory structure created"
else
    print_status 1 "Workshop directory structure missing"
fi

# Performance checks
echo "6. Performance checks..."
START_TIME=$(date +%s)
aws sts get-caller-identity > /dev/null 2>&1
END_TIME=$(date +%s)
DURATION=$((END_TIME - START_TIME))

if [ $DURATION -lt 5 ]; then
    print_status 0 "AWS CLI response time acceptable ($DURATION seconds)"
else
    print_warning "AWS CLI response time slow ($DURATION seconds)"
fi

START_TIME=$(date +%s)
docker run --rm alpine:latest echo "test" > /dev/null 2>&1
END_TIME=$(date +%s)
DURATION=$((END_TIME - START_TIME))

if [ $DURATION -lt 10 ]; then
    print_status 0 "Docker performance acceptable ($DURATION seconds)"
else
    print_warning "Docker performance slow ($DURATION seconds)"
fi

echo
echo "=== Summary ==="
echo "✅ Prerequisites verification completed!"
echo "🚀 Ready to start Lambda Container Workshop!"
echo
echo "Next steps:"
echo "1. Continue to Build Container Images section"
echo "2. Follow workshop instructions step by step"
echo "3. Remember to clean up resources after workshop"
echo
EOF

chmod +x verify-workshop-env.sh
```

## Bước 4: Run Comprehensive Verification

### **Execute Verification Script**
```bash
# Run verification
./verify-workshop-env.sh
```

**Expected Output:**
```
=== Lambda Container Workshop - Environment Verification ===

1. Checking AWS CLI...
✅ AWS CLI installed
✅ AWS CLI configured (Account: 123456789012)

2. Checking Docker...
✅ Docker installed
✅ Docker daemon running
✅ Docker functionality verified

3. Checking Terraform...
✅ Terraform installed
✅ Terraform + AWS integration working

4. Checking AWS service permissions...
✅ Lambda service access
✅ ECR service access
✅ IAM service access
✅ CloudWatch Logs access

5. Checking workshop environment...
✅ Workshop directory structure created

6. Performance checks...
✅ AWS CLI response time acceptable (2 seconds)
✅ Docker performance acceptable (5 seconds)

=== Summary ===
✅ Prerequisites verification completed!
🚀 Ready to start Lambda Container Workshop!
```

## Bước 5: Final Checklist

### **Manual Verification**

#### **✅ AWS Account & CLI**
- [ ] AWS CLI v2.x installed
- [ ] AWS credentials configured
- [ ] Can access AWS Console
- [ ] IAM user has PowerUserAccess
- [ ] Billing alerts configured
- [ ] Account ID known: `_______________`

#### **✅ Docker Desktop**
- [ ] Docker Desktop installed và running
- [ ] Can run `docker run hello-world`
- [ ] Can build simple Dockerfile
- [ ] No permission errors
- [ ] Performance acceptable

#### **✅ Terraform**
- [ ] Terraform v1.x installed
- [ ] Can run `terraform init`
- [ ] AWS provider downloads successfully
- [ ] Can plan AWS resources
- [ ] Auto-completion configured (optional)

#### **✅ Workshop Environment**
- [ ] Workshop directory created
- [ ] Project structure in place
- [ ] Verification script passes
- [ ] All integrations working
- [ ] Ready for hands-on work

### **Troubleshooting Quick Reference**

#### **Common Issues & Solutions**
1. **AWS CLI not found**: Check PATH, reinstall
2. **Docker permission denied**: Add user to docker group
3. **Terraform provider issues**: Check internet, clear cache
4. **AWS access denied**: Verify IAM permissions
5. **Slow performance**: Check system resources

## Success Criteria

### **🎯 You're Ready When:**

1. **All verification checks pass** ✅
2. **No error messages** in any tool
3. **Can access all required AWS services**
4. **Workshop directory structure created**
5. **Integration between tools working**

### **📊 Expected Performance Benchmarks**
- AWS CLI calls: < 5 seconds
- Docker container start: < 10 seconds  
- Terraform init: < 30 seconds
- All tools respond without errors

## Next Steps

### **🚀 Workshop Ready!**

Congratulations! Your environment is fully prepared for the Lambda Container Images Workshop.

**What's Next:**
1. Continue to [Build Container Images](../../3-build-container-images/)
2. Start creating Lambda function code
3. Build optimized Docker images
4. Deploy to AWS Lambda

### **📝 Keep This Information Handy**
- **AWS Account ID**: `$(aws sts get-caller-identity --query Account --output text)`
- **AWS Region**: `ap-southeast-1`
- **Workshop Directory**: `lambda-container-workshop/`
- **Verification Script**: `verify-workshop-env.sh`

---

**🎉 Prerequisites Complete!** You're now ready to dive into the exciting world of Lambda Container Images optimization!