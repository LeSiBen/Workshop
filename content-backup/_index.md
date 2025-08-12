---
title: "Lambda Container Images Workshop"
date: "`r Sys.Date()`" 
weight: 1 
chapter: false
---

# Lambda Container Images với Performance Optimization Workshop

### Tổng quan Workshop
Workshop này sẽ hướng dẫn bạn từng bước chi tiết để xây dựng, tối ưu hóa và deploy AWS Lambda functions sử dụng container images. Bạn sẽ học cách đạt được performance cao, bảo mật tốt và tối ưu chi phí thông qua các kỹ thuật advanced.

![Lambda Container Architecture](/images/lambda-container-architecture.png)

### Mục tiêu Workshop
Sau khi hoàn thành workshop này, bạn sẽ có thể:

- ⚡ **Tạo Lambda functions với startup time < 2 giây**
- 🔒 **Đạt zero security vulnerabilities**
- 💰 **Giảm 50%+ chi phí compute**
- 📊 **Setup monitoring và alerting tự động**
- 🚀 **Xây dựng CI/CD pipeline hoàn chỉnh**
- 🛠️ **Troubleshoot và optimize performance issues**

### Kiến trúc Tổng quan
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Developer     │    │   CI/CD         │    │   AWS Lambda    │
│   Workstation   │───▶│   Pipeline      │───▶│   Container     │
│   (Local Dev)   │    │   (GitHub)      │    │   Functions     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Docker        │    │   Amazon ECR    │    │   CloudWatch    │
│   Images        │    │   Registry      │    │   Monitoring    │
│   (Multi-stage) │    │   (Scanning)    │    │   (Dashboards)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Nội dung Workshop

#### 📚 **Phần 1: Giới thiệu và Chuẩn bị**
1. [Giới thiệu Workshop](1-introduce/)
2. [Chuẩn bị môi trường](2-prerequisites/)
   - [2.1 Setup AWS Account](2-prerequisites/2.1-aws-account-setup/)
   - [2.2 Cài đặt Docker Desktop](2-prerequisites/2.2-install-docker/)
   - [2.3 Cài đặt Terraform](2-prerequisites/2.3-install-terraform/)
   - [2.4 Cài đặt AWS CLI](2-prerequisites/2.4-install-aws-cli/)
   - [2.5 Kiểm tra Installation](2-prerequisites/2.5-verify-installation/)

#### 🐳 **Phần 2: Xây dựng Container Images**
3. [Build Container Images](3-build-container-images/)
   - [3.1 Tạo Lambda Function](3-build-container-images/3.1-create-lambda-function/)
   - [3.2 Tạo Dockerfile](3-build-container-images/3.2-create-dockerfile/)
   - [3.3 Build Images](3-build-container-images/3.3-build-images/)
   - [3.4 Tạo ECR Repository](3-build-container-images/3.4-create-ecr-repository/)
   - [3.5 Push Images lên ECR](3-build-container-images/3.5-push-to-ecr/)

#### 🚀 **Phần 3: Deploy Infrastructure**
4. [Deploy Infrastructure](4-deploy-infrastructure/)
   - [4.1 Setup Terraform](4-deploy-infrastructure/4.1-terraform-setup/)
   - [4.2 Tạo IAM Roles](4-deploy-infrastructure/4.2-create-iam-roles/)
   - [4.3 Deploy Lambda Functions](4-deploy-infrastructure/4.3-deploy-lambda-functions/)
   - [4.4 Verify Deployment](4-deploy-infrastructure/4.4-verify-deployment/)

#### ⚡ **Phần 4: Performance Testing**
5. [Performance Testing](5-performance-testing/)
   - [5.1 Test Cold Start Performance](5-performance-testing/5.1-test-cold-start/)
   - [5.2 Test Warm Start Performance](5-performance-testing/5.2-test-warm-start/)
   - [5.3 So sánh Performance](5-performance-testing/5.3-compare-performance/)
   - [5.4 Optimize Settings](5-performance-testing/5.4-optimize-settings/)

#### 🔒 **Phần 5: Security và Monitoring**
6. [Security và Monitoring](6-security-and-monitoring/)
   - [6.1 Security Scanning](6-security-and-monitoring/6.1-security-scanning/)
   - [6.2 Setup CloudWatch](6-security-and-monitoring/6.2-setup-cloudwatch/)
   - [6.3 Tạo Dashboard](6-security-and-monitoring/6.3-create-dashboard/)
   - [6.4 Setup Alarms](6-security-and-monitoring/6.4-setup-alarms/)

#### 💰 **Phần 6: Cost Analysis**
7. [Cost Analysis](7-cost-analysis/)
   - [7.1 Analyze Current Costs](7-cost-analysis/7.1-analyze-costs/)
   - [7.2 Optimize Costs](7-cost-analysis/7.2-optimize-costs/)
   - [7.3 Setup Cost Monitoring](7-cost-analysis/7.3-cost-monitoring/)

#### 🧹 **Phần 7: Clean-up**
8. [Clean-up Resources](8-clean-up/)
   - [8.1 Delete Lambda Functions](8-clean-up/8.1-delete-lambda-functions/)
   - [8.2 Delete ECR Images](8-clean-up/8.2-delete-ecr-images/)
   - [8.3 Delete IAM Roles](8-clean-up/8.3-delete-iam-roles/)
   - [8.4 Verify Clean-up](8-clean-up/8.4-verify-cleanup/)

### Yêu cầu Trước khi Bắt đầu

#### 🔧 **Technical Requirements**
- **AWS Account** với administrative access
- **Computer** với ít nhất 8GB RAM và 20GB free space
- **Internet connection** ổn định (để download images và tools)
- **Basic knowledge** về containers và AWS services

#### 💻 **Operating System Support**
- **Windows 10/11** (Pro edition khuyến nghị cho Docker)
- **macOS 10.15+** (Intel hoặc Apple Silicon)
- **Linux** (Ubuntu 18.04+, CentOS 7+, Amazon Linux 2)

#### ⏱️ **Thời gian Ước tính**
- **Total workshop time**: 3-4 giờ
- **Prerequisites setup**: 45-60 phút
- **Main workshop**: 2-3 giờ
- **Clean-up**: 15-30 phút

### Kết quả Mong đợi

Sau khi hoàn thành workshop, bạn sẽ có:

#### 🎯 **Deliverables**
- **2 Lambda functions** (standard và optimized) chạy với container images
- **ECR repository** với vulnerability scanning enabled
- **Terraform infrastructure** code để reproduce environment
- **CloudWatch dashboard** để monitor performance
- **Cost analysis report** với optimization recommendations
- **CI/CD pipeline** template cho automated deployment

#### 📊 **Performance Metrics**
- **Cold start time**: < 2 seconds
- **Warm execution time**: < 500ms
- **Image size**: Optimized to < 200MB
- **Security score**: Zero HIGH/CRITICAL vulnerabilities
- **Cost reduction**: 50%+ compared to baseline

#### 🛠️ **Skills Acquired**
- Container optimization techniques
- Lambda performance tuning
- Security scanning và remediation
- Infrastructure as Code với Terraform
- Cost optimization strategies
- Monitoring và alerting setup

### Troubleshooting và Support

#### 🆘 **Common Issues**
- Docker installation problems
- AWS credentials configuration
- Terraform state management
- ECR push permission issues
- Lambda function timeout errors

#### 📚 **Additional Resources**
- [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/)

### Bắt đầu Workshop

Sẵn sàng để bắt đầu? Hãy tiếp tục với [Giới thiệu Workshop](1-introduce/) để hiểu rõ hơn về Lambda Container Images và lợi ích của chúng.

---

**⚠️ Lưu ý quan trọng**: Workshop này sẽ tạo ra các AWS resources có thể phát sinh chi phí. Hãy đảm bảo bạn clean-up tất cả resources sau khi hoàn thành để tránh charges không mong muốn.