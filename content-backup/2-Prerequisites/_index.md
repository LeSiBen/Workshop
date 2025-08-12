---
title: "Chuẩn bị môi trường"
date: "`r Sys.Date()`"
weight: 2
chapter: true
pre: "<b>2. </b>"
---

# Chuẩn bị môi trường Workshop

## Tổng quan Prerequisites

Trước khi bắt đầu workshop, bạn cần chuẩn bị đầy đủ môi trường development với các tools và services cần thiết. Phần này sẽ hướng dẫn chi tiết từng bước để đảm bảo bạn có thể hoàn thành workshop thành công.

### ⏱️ **Thời gian ước tính**: 45-60 phút

### 🎯 **Mục tiêu phần này**
Sau khi hoàn thành phần Prerequisites, bạn sẽ có:
- ✅ AWS Account được cấu hình đúng cách
- ✅ Docker Desktop chạy ổn định
- ✅ Terraform installed và configured
- ✅ AWS CLI với proper credentials
- ✅ Tất cả tools được verify hoạt động

## Checklist Tổng quan

### 🔧 **Technical Requirements**

#### **Hardware Requirements**
- **RAM**: Minimum 8GB (16GB khuyến nghị)
- **Storage**: Ít nhất 20GB free space
- **CPU**: 2+ cores (4+ cores khuyến nghị)
- **Network**: Stable internet connection (cho downloads)

#### **Operating System Support**
- **Windows**: 10/11 (Pro edition cho Docker Desktop)
- **macOS**: 10.15+ (Intel hoặc Apple Silicon)
- **Linux**: Ubuntu 18.04+, CentOS 7+, Amazon Linux 2

#### **Software Prerequisites**
- **Web browser**: Chrome, Firefox, Safari, hoặc Edge
- **Terminal/Command line**: PowerShell, Terminal, hoặc bash
- **Text editor**: VS Code khuyến nghị (hoặc bất kỳ editor nào)

### 🏗️ **Architecture Overview**

```
┌─────────────────────────────────────────────────────────────────┐
│                    Your Development Machine                      │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Docker    │  │ Terraform   │  │   AWS CLI   │             │
│  │  Desktop    │  │   Binary    │  │   Config    │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │     Git     │  │   VS Code   │  │   Terminal  │             │
│  │   Client    │  │   Editor    │  │   /Shell    │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼ (Secure Connection)
┌─────────────────────────────────────────────────────────────────┐
│                         AWS Cloud                               │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │     IAM     │  │   Lambda    │  │     ECR     │             │
│  │   Service   │  │   Service   │  │   Service   │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │ CloudWatch  │  │   Cost      │  │   Security  │             │
│  │   Service   │  │ Explorer    │  │   Services  │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
└─────────────────────────────────────────────────────────────────┘
```

## Detailed Setup Roadmap

### 📋 **Step-by-Step Process**

#### **Phase 1: AWS Foundation** (15-20 phút)
1. [**AWS Account Setup**](2.1-aws-account-setup/)
   - Create hoặc verify AWS account
   - Setup billing alerts
   - Understand cost implications
   - Configure account security

#### **Phase 2: Development Tools** (20-25 phút)
2. [**Install Docker Desktop**](2.2-install-docker/)
   - Download và install Docker
   - Configure Docker settings
   - Verify Docker functionality
   - Troubleshoot common issues

3. [**Install Terraform**](2.3-install-terraform/)
   - Choose installation method
   - Install Terraform binary
   - Configure PATH variables
   - Verify installation

4. [**Install AWS CLI**](2.4-install-aws-cli/)
   - Download và install AWS CLI
   - Configure credentials
   - Set default region
   - Test connectivity

#### **Phase 3: Verification** (5-10 phút)
5. [**Verify Installation**](2.5-verify-installation/)
   - Run comprehensive tests
   - Validate all integrations
   - Create workshop directory
   - Final readiness check

## Important Considerations

### 💰 **Cost Awareness**

#### **Estimated Workshop Costs**
```
Service                 Estimated Cost    Duration
─────────────────────────────────────────────────
Lambda Invocations     $0.01 - $0.05     3-4 hours
ECR Storage           $0.10 - $0.20     1 day
CloudWatch Logs       $0.01 - $0.02     1 day
Data Transfer         $0.01 - $0.03     Workshop
─────────────────────────────────────────────────
TOTAL                 $0.13 - $0.30     Workshop
```

**⚠️ Important**: Costs có thể vary based on region và usage patterns. Workshop được design để minimize costs, nhưng hãy monitor spending và clean up resources sau khi hoàn thành.

### 🔒 **Security Considerations**

#### **Best Practices**
1. **AWS Credentials**
   - Never commit credentials to Git
   - Use IAM users, not root account
   - Enable MFA when possible
   - Rotate keys regularly

2. **Local Security**
   - Keep tools updated
   - Use secure networks
   - Enable firewall protection
   - Regular system updates

### 🚨 **Common Pitfalls**

#### **Issues to Avoid**
1. **Docker Problems**
   - WSL2 not enabled (Windows)
   - Insufficient disk space
   - Network proxy issues
   - Permission problems

2. **AWS Configuration**
   - Wrong region selection
   - Insufficient permissions
   - Credential conflicts
   - Service limits

3. **Terraform Issues**
   - Version compatibility
   - Provider conflicts
   - State file problems
   - Resource naming conflicts

## Pre-Workshop Checklist

### ✅ **Before Starting Each Section**

#### **General Preparation**
- [ ] Stable internet connection
- [ ] Administrator/sudo access on your machine
- [ ] At least 2 hours of uninterrupted time
- [ ] Backup important work (just in case)

#### **AWS Preparation**
- [ ] Valid credit card for AWS account
- [ ] Understanding of basic AWS concepts
- [ ] Willingness to incur small costs (~$0.30)
- [ ] Access to email for account verification

#### **Technical Preparation**
- [ ] Comfortable with command line
- [ ] Basic understanding of containers
- [ ] Familiarity with text editors
- [ ] Patience for downloads và installations

## Success Criteria

### 🎯 **How to Know You're Ready**

Bạn sẽ biết mình đã sẵn sàng khi:

1. **All Commands Work**
   ```bash
   aws sts get-caller-identity     # Shows your AWS account
   docker run hello-world          # Shows Docker success message
   terraform --version             # Shows Terraform version
   git --version                   # Shows Git version
   ```

2. **AWS Access Confirmed**
   - Can access AWS Console
   - Can create/list resources
   - Billing dashboard accessible
   - No permission errors

3. **Development Environment Ready**
   - Docker containers can build
   - Terraform can initialize
   - AWS CLI can make API calls
   - All tools in PATH

## Getting Help

### 🆘 **If You Get Stuck**

#### **Troubleshooting Resources**
1. **Each section** có detailed troubleshooting guide
2. **Common errors** được documented với solutions
3. **Alternative methods** provided cho different OS
4. **Verification steps** để confirm success

#### **Additional Support**
- AWS Documentation links
- Community forums
- Stack Overflow tags
- GitHub issues (nếu có)

---

## Ready to Start?

Bây giờ bạn đã hiểu overview của prerequisites, hãy bắt đầu với [AWS Account Setup](2.1-aws-account-setup/) để tạo foundation cho workshop.

### 🚀 **Next Steps Preview**

1. **AWS Account** - Foundation cho tất cả AWS services
2. **Docker Desktop** - Container development platform  
3. **Terraform** - Infrastructure as Code tool
4. **AWS CLI** - Command line access to AWS
5. **Verification** - Ensure everything works together

**Estimated total time**: 45-60 phút để complete tất cả prerequisites.

Let's get started! 👉 [AWS Account Setup](2.1-aws-account-setup/)