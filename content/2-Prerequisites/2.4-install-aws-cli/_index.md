---
title: "Install AWS CLI"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: "<b>2.4 </b>"
---

# Install AWS CLI

## Tổng quan

AWS CLI (Command Line Interface) là essential tool để interact với AWS services từ terminal. Workshop này requires AWS CLI để manage ECR repositories, Lambda functions, và other AWS resources.

### ⏱️ **Thời gian ước tính**: 10-15 phút
### 📦 **Version required**: AWS CLI v2.x

## Bước 1: Installation Methods

### **Windows Installation**

#### **Method 1: MSI Installer (Khuyến nghị)**
1. **Download AWS CLI v2**:
   - Visit: [https://awscli.amazonaws.com/AWSCLIV2.msi](https://awscli.amazonaws.com/AWSCLIV2.msi)
   - File size: ~30MB

2. **Run Installer**:
   - Double-click downloaded MSI file
   - Follow installation wizard
   - Accept license agreement
   - Choose installation directory (default: `C:\Program Files\Amazon\AWSCLIV2\`)
   - Click "Install"

3. **Verify Installation**:
   ```cmd
   aws --version
   # Expected: aws-cli/2.x.x Python/3.x.x Windows/10 exe/AMD64
   ```

#### **Method 2: PowerShell**
```powershell
# Download and install
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri "https://awscli.amazonaws.com/AWSCLIV2.msi" -OutFile "AWSCLIV2.msi"
Start-Process msiexec.exe -Wait -ArgumentList '/I AWSCLIV2.msi /quiet'
Remove-Item "AWSCLIV2.msi"

# Verify
aws --version
```

### **macOS Installation**

#### **Method 1: PKG Installer**
```bash
# Download and install
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /

# Verify
aws --version
```

#### **Method 2: Homebrew**
```bash
# Install via Homebrew
brew install awscli

# Verify
aws --version
```

### **Linux Installation**

#### **Method 1: Bundled Installer**
```bash
# Download AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

# Extract
unzip awscliv2.zip

# Install
sudo ./aws/install

# Verify
aws --version

# Clean up
rm -rf aws awscliv2.zip
```

#### **Method 2: Package Manager**
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install awscli

# CentOS/RHEL
sudo yum install awscli
```

## Bước 2: Configure AWS CLI

### **Bước 2.1: Basic Configuration**

#### **Run AWS Configure**
```bash
aws configure
```

#### **Enter Configuration Values**:
```
AWS Access Key ID [None]: AKIA...your-access-key
AWS Secret Access Key [None]: your-secret-access-key
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

**Important**: Use IAM user credentials created in [AWS Account Setup](../2.1-aws-account-setup/), NOT root account credentials.

### **Bước 2.2: Verify Configuration**

#### **Test AWS CLI**
```bash
# Get caller identity
aws sts get-caller-identity

# Expected output:
{
    "UserId": "AIDACKCEVSQ6C2EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/workshop-user"
}
```

#### **Test Service Access**
```bash
# List Lambda functions
aws lambda list-functions --region ap-southeast-1

# List ECR repositories
aws ecr describe-repositories --region ap-southeast-1

# Both should return empty lists (no errors)
```

### **Bước 2.3: Advanced Configuration**

#### **Multiple Profiles**
```bash
# Configure additional profile
aws configure --profile workshop
AWS Access Key ID [None]: AKIA...
AWS Secret Access Key [None]: ...
Default region name [None]: ap-southeast-1
Default output format [None]: json

# Use specific profile
aws sts get-caller-identity --profile workshop

# Set default profile
export AWS_PROFILE=workshop
```

#### **Configuration Files**
```bash
# View configuration files
cat ~/.aws/config
cat ~/.aws/credentials

# Example ~/.aws/config:
[default]
region = ap-southeast-1
output = json

[profile workshop]
region = ap-southeast-1
output = json

# Example ~/.aws/credentials:
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

[workshop]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
```

## Bước 3: Test AWS CLI Functionality

### **Bước 3.1: Basic Service Tests**

#### **IAM Test**
```bash
# List IAM users
aws iam list-users

# Get current user
aws iam get-user
```

#### **Lambda Test**
```bash
# List Lambda functions
aws lambda list-functions

# Get account settings
aws lambda get-account-settings
```

#### **ECR Test**
```bash
# Describe repositories
aws ecr describe-repositories

# Get authorization token
aws ecr get-authorization-token
```

### **Bước 3.2: Performance Test**

#### **Response Time Test**
```bash
# Time AWS CLI calls
time aws sts get-caller-identity
time aws lambda list-functions
time aws ecr describe-repositories

# Should complete in < 5 seconds each
```

## Troubleshooting

### **Issue 1: Command Not Found**
```bash
# Check PATH
echo $PATH | grep aws

# Add to PATH (Linux/macOS)
export PATH=$PATH:/usr/local/bin/aws

# Windows: Add to System PATH
```

### **Issue 2: Credentials Not Found**
```bash
# Check credentials file
cat ~/.aws/credentials

# Reconfigure
aws configure

# Use environment variables
export AWS_ACCESS_KEY_ID="your-key"
export AWS_SECRET_ACCESS_KEY="your-secret"
```

### **Issue 3: Permission Denied**
```bash
# Check IAM permissions
aws iam get-user

# Verify policy attachments
aws iam list-attached-user-policies --user-name workshop-user
```

## Next Steps

### **Verification Checklist**
- [ ] `aws --version` shows v2.x
- [ ] `aws sts get-caller-identity` works
- [ ] Can access Lambda và ECR services
- [ ] No permission errors

Ready for [Verify Installation](../2.5-verify-installation/)!