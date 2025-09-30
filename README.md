# AWS Security Hub Implementation Lab

## Overview

This lab demonstrates how to implement AWS Security Hub for automated compliance assessment and security monitoring. You'll set up a complete security monitoring solution for SEDIK, LLC, a global healthcare organization that recently migrated to AWS.

## Scenario

SEDIK, LLC is struggling to maintain adherence to industry standards and AWS best practices. This lab implements AWS Security Hub to:
- Automatically assess AWS resources against PCI DSS standards
- Monitor AWS Foundational Security Best Practices (FSBP)
- Send notifications for urgent security breaches

## Architecture

<img width="2118" height="782" alt="image" src="https://github.com/user-attachments/assets/5d8adab5-2722-406b-a44d-64076cea1791" />

The solution includes:
- **AWS Security Hub**: Central security findings aggregation
- **AWS Config**: Configuration compliance monitoring  
- **Amazon SNS**: Email notifications for critical findings
- **Amazon EventBridge**: Event routing and filtering
- **EC2 Security Groups**: Test resources for generating findings

## Prerequisites

- AWS Account with administrative access
- Valid email address for notifications
- Basic understanding of AWS services

## Lab Objectives

By completing this lab, you will:
1. Create and configure an SNS topic with email subscription
2. Set up EventBridge rules for security finding notifications
3. Enable and configure AWS Config
4. Enable AWS Security Hub with compliance standards
5. Generate sample security findings for testing

## Step-by-Step Instructions

### Step 1: Create SNS Topic

1. Navigate to **Simple Notification Service** in AWS Console (us-east-1)
2. Create topic named `eMailMe`
3. Configure access policy:
   - **Publishers**: Everyone
   - **Subscribers**: Everyone

### Step 2: Create Email Subscriber

1. Create subscription with:
   - **Protocol**: Email
   - **Endpoint**: Your email address
2. Confirm subscription via email (may take up to 15 minutes)

### Step 3: Create EventBridge Rule

1. Navigate to **Amazon EventBridge**
2. Create rule with following configuration:
   - **Name**: `SEDIKSecurityHub`
   - **Description**: Rule that sends email when Security Hub findings are generated
   - **AWS Service**: Security Hub
   - **Event Type**: Security Hub Findings - Imported
   - **Compliance Status**: FAILED
   - **Record State**: ACTIVE
   - **Severity**: HIGH and CRITICAL
   - **Target**: SNS topic `eMailMe`

### Step 4: Enable AWS Config

1. Navigate to **AWS Config**
2. Use **1-click setup** option
3. Confirm configuration

### Step 5: Enable AWS Security Hub

1. Navigate to **Security Hub**
2. Click **Get started** and **Enable**
3. Access **AWS Security Hub CSPM**
4. Enable security standards:
   - ✅ **AWS Foundational Security Best Practices**
   - ✅ **PCI DSS v3.2.1**
   - ❌ **CIS AWS Foundations Benchmark v1.2.0** (uncheck)

### Step 6: Generate Sample Findings

Create a misconfigured security group to trigger findings:

1. Navigate to **EC2** > **Security Groups**
2. Create security group named `SEDIKSecurityGroup`
3. Add the following inbound rules:
   - **All ICMP - IPv4** from `0.0.0.0/0`
   - **SSH** from `0.0.0.0/0`
   - **SSH** from `::/0`
   - **RDP** from `0.0.0.0/0`
   - **RDP** from `::/0`

### Step 7: Verify Results

1. Return to **Security Hub** > **Findings**
2. Wait for findings to appear (may take several minutes)
3. Check email for notification alerts
4. Refresh page as needed to see new findings

## Expected Results

After completion, you should see:
- Multiple HIGH severity findings in Security Hub
- Email notifications for critical security issues
- Compliance assessment against PCI DSS and FSBP standards

## Key Security Issues Detected

The lab will generate findings related to:
- Overly permissive security group rules
- Public access to SSH and RDP ports
- ICMP traffic allowed from anywhere
- IPv6 unrestricted access

## Cleanup (Optional)

To avoid ongoing charges:
1. Delete the test security group
2. Disable Security Hub standards
3. Delete EventBridge rule
4. Delete SNS topic and subscription

## Troubleshooting

**Findings not appearing?**
- Wait 10-15 minutes for AWS Config to detect changes
- Refresh the Security Hub Findings page
- Ensure all services are enabled in us-east-1 region

**Email notifications not received?**
- Check spam/junk folder
- Verify SNS subscription is confirmed
- Ensure EventBridge rule is active

## Technologies Used

- AWS Security Hub
- AWS Config
- Amazon SNS
- Amazon EventBridge
- Amazon EC2
- PCI DSS Compliance Standard
- AWS Foundational Security Best Practices

## Security Standards Covered

- **PCI DSS v3.2.1**: Payment card industry data security standards
- **AWS FSBP**: Foundational security best practices for AWS environments

## Learning Outcomes

- Understanding AWS Security Hub centralized monitoring
- Implementing automated compliance checking
- Configuring event-driven security notifications
- Working with AWS Config for resource compliance
- Setting up cross-service integrations for security monitoring

---

**Note**: This lab is designed for educational purposes. In production environments, follow the principle of least privilege and implement more restrictive security policies.
