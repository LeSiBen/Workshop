---
title: "Install Terraform"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>2.3 </b>"
---

# Install Terraform

## Tổng quan

Terraform là Infrastructure as Code (IaC) tool để manage AWS resources một cách consistent và reproducible. Workshop này sử dụng Terraform để create Lambda functions, ECR repositories, IAM roles, và CloudWatch resources.

### ⏱️ **Thời gian ước tính**: 10-15 phút
### 📦 **Version required**: Terraform >= 1.0

## Bước 1: Choose Installation Method

### **Installation Options**

#### **Option A: Package Manager (Khuyến nghị)**
- **Windows**: Chocolatey hoặc Scoop
- **macOS**: Homebrew
- **Linux**: APT, YUM, hoặc package managers

#### **Option B: Binary Download**
- Download từ HashiCorp website
- Manual installation và PATH configuration

#### **Option C: Version Manager**
- **tfenv**: Terraform version manager
- **tfswitch**: Terraform switcher

## Bước 2: Windows Installation

### **Bước 2.1: Using Chocolatey (Khuyến nghị)**

#### **Install Chocolatey (nếu chưa có)**
1. **Open PowerShell as Administrator**
2. **Check execution policy**:
   ```powershell
   Get-ExecutionPolicy
   ```

3. **Set execution policy** (nếu Restricted):
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force
   ```

4. **Install Chocolatey**:
   ```powershell
   [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
   iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```

5. **Verify Chocolatey**:
   ```powershell
   choco --version
   ```

![Chocolatey Installation](/images/prerequisites/chocolatey-install.png)

#### **Install Terraform với Chocolatey**
```powershell
# Install Terraform
choco install terraform

# Verify installation
terraform --version
```

### **Bước 2.2: Using Scoop (Alternative)**

#### **Install Scoop**
```powershell
# In PowerShell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex
```

#### **Install Terraform với Scoop**
```powershell
# Install Terraform
scoop install terraform

# Verify installation
terraform --version
```

### **Bước 2.3: Manual Installation (Windows)**

#### **Download Terraform Binary**
1. **Visit Terraform Downloads**:
   - Go to [https://www.terraform.io/downloads](https://www.terraform.io/downloads)
   - Click **"Windows"** → **"AMD64"**
   - Download ZIP file (~50MB)

![Terraform Download](/images/prerequisites/terraform-download.png)

2. **Extract Binary**:
   - Create directory: `C:\terraform`
   - Extract `terraform.exe` to `C:\terraform\`

3. **Add to PATH**:
   - Open **System Properties** → **Advanced** → **Environment Variables**
   - Edit **System PATH** variable
   - Add `C:\terraform`
   - Click **OK** và restart Command Prompt

![Windows PATH](/images/prerequisites/windows-path.png)

4. **Verify Installation**:
   ```cmd
   terraform --version
   ```

## Bước 3: macOS Installation

### **Bước 3.1: Using Homebrew (Khuyến nghị)**

#### **Install Homebrew (nếu chưa có)**
```bash
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Verify Homebrew
brew --version
```

#### **Install Terraform với Homebrew**
```bash
# Add HashiCorp tap
brew tap hashicorp/tap

# Install Terraform
brew install hashicorp/tap/terraform

# Verify installation
terraform --version
```

### **Bước 3.2: Using tfenv (Version Manager)**

#### **Install tfenv**
```bash
# Install tfenv
brew install tfenv

# List available Terraform versions
tfenv list-remote

# Install latest version
tfenv install latest

# Use latest version
tfenv use latest

# Verify installation
terraform --version
```

### **Bước 3.3: Manual Installation (macOS)**

#### **Download và Install**
```bash
# Download Terraform (replace with latest version)
curl -O https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_darwin_amd64.zip

# For Apple Silicon Macs:
# curl -O https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_darwin_arm64.zip

# Extract
unzip terraform_1.6.0_darwin_amd64.zip

# Move to PATH
sudo mv terraform /usr/local/bin/

# Verify installation
terraform --version

# Clean up
rm terraform_1.6.0_darwin_amd64.zip
```

## Bước 4: Linux Installation

### **Bước 4.1: Ubuntu/Debian Installation**

#### **Using APT Repository**
```bash
# Update package index
sudo apt-get update

# Install prerequisites
sudo apt-get install -y gnupg software-properties-common

# Add HashiCorp GPG key
wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

# Verify key fingerprint
gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint

# Add HashiCorp repository
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list

# Update package index
sudo apt update

# Install Terraform
sudo apt-get install terraform

# Verify installation
terraform --version
```

### **Bước 4.2: CentOS/RHEL Installation**

#### **Using YUM Repository**
```bash
# Install yum-config-manager
sudo yum install -y yum-utils

# Add HashiCorp repository
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo

# Install Terraform
sudo yum -y install terraform

# Verify installation
terraform --version
```

### **Bước 4.3: Manual Installation (Linux)**

#### **Download và Install**
```bash
# Download Terraform (replace with latest version)
wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip

# Extract
unzip terraform_1.6.0_linux_amd64.zip

# Move to PATH
sudo mv terraform /usr/local/bin/

# Verify installation
terraform --version

# Clean up
rm terraform_1.6.0_linux_amd64.zip
```

## Bước 5: Configure Terraform

### **Bước 5.1: Basic Configuration**

#### **Create Terraform Directory**
```bash
# Create workspace directory
mkdir -p ~/terraform-workspace
cd ~/terraform-workspace

# Create test configuration
cat > main.tf << EOF
terraform {
  required_version = ">= 1.0"
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

# Test data source
data "aws_caller_identity" "current" {}

output "account_id" {
  value = data.aws_caller_identity.current.account_id
}
EOF
```

#### **Initialize Terraform**
```bash
# Initialize Terraform
terraform init

# Expected output:
# Terraform has been successfully initialized!
```

![Terraform Init](/images/prerequisites/terraform-init.png)

### **Bước 5.2: Verify AWS Integration**

#### **Test AWS Provider**
```bash
# Validate configuration
terraform validate

# Plan (should show account ID)
terraform plan

# Expected output shows your AWS account ID
```

#### **Clean Up Test**
```bash
# Remove test files
rm -rf .terraform*
rm main.tf
cd ~
```

## Bước 6: Advanced Configuration

### **Bước 6.1: Configure Auto-completion**

#### **Bash Auto-completion**
```bash
# Install auto-completion
terraform -install-autocomplete

# Reload shell
source ~/.bashrc
```

#### **Zsh Auto-completion**
```bash
# For Zsh users
terraform -install-autocomplete

# Reload shell
source ~/.zshrc
```

#### **PowerShell Auto-completion (Windows)**
```powershell
# Install PSReadLine module
Install-Module -Name PSReadLine -AllowPrerelease -Force

# Add to PowerShell profile
Add-Content $PROFILE "Register-ArgumentCompleter -Native -CommandName terraform -ScriptBlock {
    param(`$wordToComplete, `$commandAst, `$cursorPosition)
    `$Local:word = `$wordToComplete.Replace('`"', '```"')
    `$Local:ast = `$commandAst.ToString().Replace('`"', '```"')
    terraform -install-autocomplete 2>&1 | Out-Null
    (terraform -install-autocomplete) -split '`n' | ForEach-Object {
        [System.Management.Automation.CompletionResult]::new(`$_, `$_, 'ParameterValue', `$_)
    }
}"
```

### **Bước 6.2: Configure Logging**

#### **Enable Debug Logging**
```bash
# Set environment variables
export TF_LOG=DEBUG
export TF_LOG_PATH=./terraform.log

# For Windows PowerShell:
# $env:TF_LOG="DEBUG"
# $env:TF_LOG_PATH="./terraform.log"
```

#### **Log Levels**
- **TRACE**: Most verbose
- **DEBUG**: Debug information
- **INFO**: General information
- **WARN**: Warning messages
- **ERROR**: Error messages only

### **Bước 6.3: Configure Backend (Optional)**

#### **S3 Backend Configuration**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state-bucket"
    key    = "lambda-workshop/terraform.tfstate"
    region = "ap-southeast-1"
    
    # Optional: DynamoDB table for state locking
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
  }
}
```

**Note**: Backend configuration sẽ được covered chi tiết trong workshop main sections.

## Bước 7: Verify Installation

### **Bước 7.1: Version Check**

#### **Check Terraform Version**
```bash
# Check version
terraform --version

# Expected output:
# Terraform v1.6.0
# on linux_amd64
```

#### **Check Available Commands**
```bash
# List all commands
terraform -help

# Get help for specific command
terraform plan -help
terraform apply -help
```

### **Bước 7.2: Provider Test**

#### **Test AWS Provider**
```bash
# Create test directory
mkdir terraform-test
cd terraform-test

# Create simple configuration
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

output "account_id" {
  value = data.aws_caller_identity.current.account_id
}

output "caller_arn" {
  value = data.aws_caller_identity.current.arn
}

output "caller_user" {
  value = data.aws_caller_identity.current.user_id
}
EOF

# Initialize
terraform init

# Plan
terraform plan

# Should show your AWS account information
```

### **Bước 7.3: Performance Test**

#### **Test Terraform Performance**
```bash
# Time the initialization
time terraform init

# Should complete in < 30 seconds

# Time the planning
time terraform plan

# Should complete in < 10 seconds
```

## Troubleshooting Common Issues

### **Issue 1: Command Not Found**

#### **Symptoms**
- `terraform: command not found`
- `'terraform' is not recognized as an internal or external command`

#### **Solutions**
1. **Check PATH**:
   ```bash
   # Linux/macOS
   echo $PATH | grep terraform
   
   # Windows
   echo $env:PATH | Select-String terraform
   ```

2. **Reinstall with Package Manager**:
   ```bash
   # macOS
   brew reinstall hashicorp/tap/terraform
   
   # Windows
   choco uninstall terraform
   choco install terraform
   ```

3. **Manual PATH Fix**:
   ```bash
   # Add to ~/.bashrc or ~/.zshrc
   export PATH=$PATH:/usr/local/bin
   
   # Windows: Add to System PATH via GUI
   ```

### **Issue 2: Provider Download Issues**

#### **Symptoms**
- "Failed to install provider"
- "Could not retrieve the list of available versions"

#### **Solutions**
1. **Check Internet Connection**:
   ```bash
   # Test connectivity
   curl -I https://registry.terraform.io
   ```

2. **Configure Proxy** (nếu behind corporate firewall):
   ```bash
   export HTTP_PROXY=http://proxy.company.com:8080
   export HTTPS_PROXY=http://proxy.company.com:8080
   ```

3. **Clear Provider Cache**:
   ```bash
   # Remove provider cache
   rm -rf .terraform
   rm .terraform.lock.hcl
   
   # Re-initialize
   terraform init
   ```

### **Issue 3: Version Conflicts**

#### **Symptoms**
- "version constraint conflict"
- "required version not available"

#### **Solutions**
1. **Check Version Constraints**:
   ```hcl
   terraform {
     required_version = ">= 1.0"
     required_providers {
       aws = {
         source  = "hashicorp/aws"
         version = "~> 5.0"  # Allow 5.x versions
       }
     }
   }
   ```

2. **Update Terraform**:
   ```bash
   # macOS
   brew upgrade hashicorp/tap/terraform
   
   # Windows
   choco upgrade terraform
   ```

3. **Use Version Manager**:
   ```bash
   # Install tfenv
   brew install tfenv
   
   # Install specific version
   tfenv install 1.6.0
   tfenv use 1.6.0
   ```

### **Issue 4: AWS Provider Issues**

#### **Symptoms**
- "No valid credential sources found"
- "Access denied" errors

#### **Solutions**
1. **Check AWS Credentials**:
   ```bash
   # Verify AWS CLI works
   aws sts get-caller-identity
   
   # Check credentials file
   cat ~/.aws/credentials
   ```

2. **Set Environment Variables**:
   ```bash
   export AWS_ACCESS_KEY_ID="your-access-key"
   export AWS_SECRET_ACCESS_KEY="your-secret-key"
   export AWS_DEFAULT_REGION="ap-southeast-1"
   ```

3. **Use AWS Profile**:
   ```bash
   # Set profile
   export AWS_PROFILE=workshop-user
   
   # Or in Terraform configuration
   provider "aws" {
     profile = "workshop-user"
     region  = "ap-southeast-1"
   }
   ```

## Terraform Best Practices

### **Code Organization**
```bash
# Recommended directory structure
terraform-project/
├── main.tf              # Main configuration
├── variables.tf         # Input variables
├── outputs.tf           # Output values
├── terraform.tfvars     # Variable values
├── versions.tf          # Provider versions
└── modules/             # Reusable modules
    └── lambda/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

### **Version Pinning**
```hcl
terraform {
  required_version = "~> 1.6.0"  # Pin Terraform version
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"          # Pin provider version
    }
  }
}
```

### **State Management**
```bash
# Always use remote state for team projects
terraform {
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "project/terraform.tfstate"
    region = "ap-southeast-1"
  }
}

# Use state locking
terraform {
  backend "s3" {
    # ... other config
    dynamodb_table = "terraform-locks"
  }
}
```

## Next Steps

### **Verification Checklist**

Trước khi tiếp tục, ensure:
- [ ] `terraform --version` shows version >= 1.0
- [ ] `terraform init` works without errors
- [ ] Can download AWS provider
- [ ] `terraform plan` shows AWS account info
- [ ] Auto-completion configured (optional)
- [ ] No permission errors

### **Ready for Next Step?**

Nếu tất cả checks pass, bạn ready cho [Install AWS CLI](../2.4-install-aws-cli/)!

**What's Next**: AWS CLI installation và configuration để interact với AWS services từ command line.

---

**🎯 Success Criteria**: Terraform installed và configured, can initialize projects và download providers. Ready for Infrastructure as Code!