---
title: "AWS Account Setup"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>2.1 </b>"
---

# AWS Account Setup và Configuration

## Tổng quan

AWS Account là foundation cho toàn bộ workshop. Bạn cần một AWS account với appropriate permissions để tạo và manage Lambda functions, ECR repositories, IAM roles, và CloudWatch resources.

### ⏱️ **Thời gian ước tính**: 15-20 phút
### 💰 **Chi phí ước tính**: $0.13 - $0.30 cho toàn bộ workshop

## Bước 1: Tạo hoặc Verify AWS Account

### Option A: Tạo AWS Account mới

#### **Bước 1.1: Truy cập AWS Console**
1. Mở web browser và vào [https://aws.amazon.com](https://aws.amazon.com)
2. Click **"Create an AWS Account"** button (góc phải trên)
3. Hoặc vào trực tiếp: [https://portal.aws.amazon.com/billing/signup](https://portal.aws.amazon.com/billing/signup)

![AWS Homepage](/images/prerequisites/aws-homepage.png)

#### **Bước 1.2: Điền thông tin Account**
1. **Email address**: Nhập email address (sẽ là root account)
   ```
   Example: your-email@example.com
   ```

2. **Password**: Tạo strong password
   - Ít nhất 8 characters
   - Bao gồm uppercase, lowercase, numbers
   - Special characters khuyến nghị

3. **AWS account name**: Nhập tên cho account
   ```
   Example: "Lambda Workshop Account"
   ```

4. Click **"Continue"**

![AWS Account Creation](/images/prerequisites/aws-account-creation.png)

#### **Bước 1.3: Contact Information**
1. **Account type**: Chọn **"Personal"** (trừ khi bạn đại diện company)

2. **Full name**: Nhập họ tên đầy đủ
   ```
   Example: John Doe
   ```

3. **Phone number**: Nhập số điện thoại (cần cho verification)
   ```
   Example: +84 123 456 789
   ```

4. **Country/Region**: Chọn country của bạn
   ```
   Example: Vietnam
   ```

5. **Address**: Nhập địa chỉ đầy đủ
   - Street address
   - City
   - State/Province  
   - Postal code

6. **Agreement**: Đọc và check **"AWS Customer Agreement"**

7. Click **"Create Account and Continue"**

![Contact Information](/images/prerequisites/aws-contact-info.png)

#### **Bước 1.4: Payment Information**
1. **Credit/Debit Card**: Nhập thông tin thẻ
   - Card number
   - Expiration date
   - Cardholder name
   - CVV/CVC code

2. **Billing address**: Confirm hoặc nhập billing address

3. Click **"Verify and Continue"**

**⚠️ Important Notes:**
- AWS sẽ charge $1 để verify thẻ (sẽ được refund)
- Thẻ cần có international transaction capability
- Prepaid cards có thể không work

![Payment Information](/images/prerequisites/aws-payment-info.png)

#### **Bước 1.5: Phone Verification**
1. **Phone number**: Confirm số điện thoại
2. **Verification method**: Chọn **"Text message (SMS)"** hoặc **"Voice call"**
3. Click **"Send SMS"** hoặc **"Call me now"**
4. Nhập **verification code** nhận được
5. Click **"Verify Code and Continue"**

![Phone Verification](/images/prerequisites/aws-phone-verification.png)

#### **Bước 1.6: Support Plan Selection**
1. **Basic Support**: Chọn **"Basic support - Free"**
   - Free tier
   - Community support
   - Đủ cho workshop này

2. Click **"Complete sign up"**

![Support Plan](/images/prerequisites/aws-support-plan.png)

#### **Bước 1.7: Account Activation**
1. Đợi email confirmation từ AWS
2. Account activation có thể mất vài phút
3. Check email và click activation link nếu cần

### Option B: Sử dụng AWS Account có sẵn

#### **Bước 1.8: Verify Account Access**
1. Vào [AWS Console](https://console.aws.amazon.com)
2. Sign in với credentials
3. Verify bạn có admin access hoặc sufficient permissions

## Bước 2: Configure Account Security

### **Bước 2.1: Enable MFA (Multi-Factor Authentication)**

#### **Setup MFA cho Root Account**
1. Vào AWS Console → **"My Security Credentials"**
2. Expand **"Multi-factor authentication (MFA)"** section
3. Click **"Activate MFA"**

![MFA Setup](/images/prerequisites/aws-mfa-setup.png)

4. **MFA device options**:
   - **Virtual MFA device** (khuyến nghị - free)
   - **Hardware MFA device**
   - **SMS text message** (deprecated)

5. **For Virtual MFA**:
   - Install authenticator app (Google Authenticator, Authy, etc.)
   - Scan QR code
   - Enter two consecutive codes
   - Click **"Assign MFA"**

### **Bước 2.2: Create IAM User (Khuyến nghị)**

#### **Tại sao cần IAM User?**
- Root account có unlimited permissions
- IAM user có controlled permissions
- Best practice: không sử dụng root cho daily tasks

#### **Create IAM User**
1. Vào **IAM Console**: [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/)
2. Click **"Users"** trong left navigation
3. Click **"Add users"**

![IAM Users](/images/prerequisites/iam-users.png)

4. **User details**:
   - **User name**: `workshop-user`
   - **Access type**: Check **"Programmatic access"** và **"AWS Management Console access"**
   - **Console password**: Choose **"Custom password"** và nhập password
   - **Require password reset**: Uncheck (optional)

![IAM User Creation](/images/prerequisites/iam-user-creation.png)

5. **Set permissions**:
   - Click **"Attach existing policies directly"**
   - Search và select **"PowerUserAccess"**
   - Hoặc **"AdministratorAccess"** nếu cần full permissions

![IAM Permissions](/images/prerequisites/iam-permissions.png)

6. **Tags** (optional):
   - Key: `Purpose`, Value: `Lambda Workshop`
   - Key: `Environment`, Value: `Learning`

7. **Review và Create**:
   - Review settings
   - Click **"Create user"**

8. **Save Credentials**:
   - **Download .csv file** với credentials
   - **Copy Access Key ID và Secret Access Key**
   - Store securely (sẽ cần cho AWS CLI)

![IAM Credentials](/images/prerequisites/iam-credentials.png)

## Bước 3: Setup Billing Alerts

### **Bước 3.1: Enable Billing Alerts**

#### **Why Billing Alerts?**
- Monitor spending real-time
- Prevent unexpected charges
- Workshop costs should be < $1

#### **Setup Process**
1. Vào **Billing Console**: [https://console.aws.amazon.com/billing/](https://console.aws.amazon.com/billing/)
2. Click **"Billing preferences"** trong left navigation
3. Check **"Receive Billing Alerts"**
4. Click **"Save preferences"**

![Billing Preferences](/images/prerequisites/billing-preferences.png)

### **Bước 3.2: Create CloudWatch Billing Alarm**

1. Vào **CloudWatch Console**: [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)
2. **Important**: Switch region to **"US East (N. Virginia)"** - billing metrics chỉ available ở region này
3. Click **"Alarms"** → **"Create alarm"**

![CloudWatch Alarms](/images/prerequisites/cloudwatch-alarms.png)

4. **Select metric**:
   - Click **"Select metric"**
   - Choose **"Billing"** → **"Total Estimated Charge"**
   - Select **"USD"** currency
   - Click **"Select metric"**

![Billing Metrics](/images/prerequisites/billing-metrics.png)

5. **Configure alarm**:
   - **Statistic**: Maximum
   - **Period**: 6 hours
   - **Threshold**: Static
   - **Condition**: Greater than `5` (USD)
   - Click **"Next"**

![Alarm Configuration](/images/prerequisites/alarm-configuration.png)

6. **Configure actions**:
   - **Alarm state trigger**: In alarm
   - **SNS topic**: Create new topic
   - **Topic name**: `billing-alerts`
   - **Email**: Your email address
   - Click **"Create topic"**
   - Click **"Next"**

7. **Name và description**:
   - **Alarm name**: `Workshop-Billing-Alert`
   - **Description**: `Alert when workshop costs exceed $5`
   - Click **"Next"**

8. **Review và create**:
   - Review settings
   - Click **"Create alarm"**

9. **Confirm email subscription**:
   - Check email for SNS confirmation
   - Click confirmation link

## Bước 4: Understand Workshop Costs

### **Bước 4.1: Cost Breakdown**

#### **Expected Costs cho Workshop**
```
Service                    Cost per Unit              Workshop Usage           Estimated Cost
─────────────────────────────────────────────────────────────────────────────────────────────
Lambda Requests           $0.20 per 1M requests      ~100 requests            $0.00002
Lambda Duration           $0.0000166667 per GB-sec   ~50 GB-seconds           $0.0008
ECR Storage              $0.10 per GB per month      ~0.5 GB for 1 day        $0.0016
CloudWatch Logs          $0.50 per GB ingested      ~0.01 GB                 $0.005
Data Transfer            $0.09 per GB                ~0.1 GB                  $0.009
─────────────────────────────────────────────────────────────────────────────────────────────
TOTAL ESTIMATED COST                                                          $0.016 - $0.30
```

#### **Cost Optimization Tips**
1. **Clean up resources** sau workshop
2. **Use smallest memory** settings khi possible
3. **Delete ECR images** sau workshop
4. **Monitor billing dashboard** regularly

### **Bước 4.2: Free Tier Benefits**

#### **AWS Free Tier Includes**
- **Lambda**: 1M requests và 400,000 GB-seconds per month
- **ECR**: 500MB storage per month
- **CloudWatch**: 10 custom metrics và 1,000,000 API requests

**Workshop sẽ stay well within free tier limits!**

## Bước 5: Verify Account Setup

### **Bước 5.1: Test Console Access**

1. **Login test**:
   - Vào [AWS Console](https://console.aws.amazon.com)
   - Login với IAM user credentials (not root)
   - Verify successful login

2. **Service access test**:
   - Navigate to **Lambda Console**
   - Navigate to **ECR Console**  
   - Navigate to **IAM Console**
   - Navigate to **CloudWatch Console**

3. **Region verification**:
   - Check current region (top right)
   - Switch to **Asia Pacific (Singapore)** - `ap-southeast-1`
   - Hoặc region gần bạn nhất

![Region Selection](/images/prerequisites/region-selection.png)

### **Bước 5.2: Test Permissions**

#### **Lambda Permissions Test**
1. Vào **Lambda Console**
2. Click **"Create function"**
3. Nếu thấy create form → permissions OK
4. **Don't actually create** - just testing access
5. Click **"Cancel"**

#### **ECR Permissions Test**
1. Vào **ECR Console**
2. Click **"Create repository"**
3. Nếu thấy create form → permissions OK
4. **Don't actually create** - just testing access
5. Click **"Cancel"**

## Troubleshooting Common Issues

### **Issue 1: Account Verification Pending**

#### **Symptoms**
- Cannot access certain services
- "Account verification in progress" messages

#### **Solutions**
1. **Wait**: Verification có thể mất 24 hours
2. **Check email**: Look for verification emails
3. **Contact support**: Use chat support nếu > 24 hours
4. **Verify payment**: Ensure credit card charge went through

### **Issue 2: Permission Denied Errors**

#### **Symptoms**
- "Access Denied" khi access services
- Cannot create resources

#### **Solutions**
1. **Check IAM permissions**: Ensure PowerUserAccess attached
2. **Use correct user**: Don't use root account
3. **Verify region**: Some services region-specific
4. **Clear browser cache**: Sometimes helps với console issues

### **Issue 3: Billing Alert Not Working**

#### **Symptoms**
- No billing alerts received
- Cannot see billing metrics

#### **Solutions**
1. **Check region**: Billing metrics chỉ ở US East (N. Virginia)
2. **Verify email**: Confirm SNS subscription
3. **Wait**: Billing data có thể delay 6-24 hours
4. **Check spam folder**: Alerts có thể vào spam

### **Issue 4: MFA Problems**

#### **Symptoms**
- Cannot login với MFA
- MFA codes not working

#### **Solutions**
1. **Time sync**: Ensure device time accurate
2. **Try backup codes**: If you saved them
3. **Contact support**: May need to reset MFA
4. **Use different authenticator**: Try different app

## Security Best Practices

### **Account Security Checklist**

#### **✅ Completed Tasks**
- [ ] Root account MFA enabled
- [ ] IAM user created với appropriate permissions
- [ ] Strong passwords used
- [ ] Billing alerts configured
- [ ] Access keys stored securely
- [ ] No credentials committed to code repositories

#### **🔒 Ongoing Security**
1. **Regular key rotation**: Rotate access keys every 90 days
2. **Monitor usage**: Review CloudTrail logs periodically
3. **Least privilege**: Only grant minimum required permissions
4. **Clean up**: Remove unused resources và users

## Next Steps

### **Verification Checklist**

Trước khi tiếp tục, ensure:
- [ ] AWS account active và accessible
- [ ] IAM user created với proper permissions
- [ ] Billing alerts configured
- [ ] Can access Lambda, ECR, IAM, CloudWatch consoles
- [ ] Access keys saved securely
- [ ] MFA enabled (khuyến nghị)

### **Ready for Next Step?**

Nếu tất cả checks pass, bạn ready cho [Install Docker Desktop](../2.2-install-docker/)!

**What's Next**: Docker Desktop installation và configuration để build container images cho Lambda functions.

---

**🎯 Success Criteria**: Bạn có working AWS account với proper permissions và billing monitoring. Time to move to Docker setup!