# IAM Security Audit Tool

## Overview
An automated AWS Lambda function that scans all IAM users daily 
for security misconfigurations and sends a detailed report via email.

## What it checks
- Users with no MFA enabled
- Access keys older than 90 days
- Users with AdministratorAccess attached

## Architecture
EventBridge (Scheduled) → Lambda → IAM Scan → SNS → Email Report

## Services Used
- AWS Lambda (Python 3.12)
- Amazon EventBridge (Scheduled trigger)
- AWS IAM
- Amazon SNS
- Amazon CloudWatch

## How it works
1. EventBridge triggers Lambda every day at 9am UTC
2. Lambda scans all IAM users in the account
3. Checks for security misconfigurations
4. Sends a detailed report via SNS email

## Schedule
Runs daily at 9am UTC using cron expression: `0 9 * * ? *`

## IAM Permissions Required
- Lambda role: `IAMReadOnlyAccess`, `AmazonSNSFullAccess`

## Challenges Faced
- Lambda role missing `iam:ListUsers` permission — fixed by 
  attaching `IAMReadOnlyAccess` policy
- CloudWatch log group not created until Lambda executes at least once

## Demo
[Add your screen recording or screenshot here]

## Future Improvements
- Check for unused IAM users (no login in 90+ days)
- Check for overly permissive S3 bucket policies
- Generate PDF report instead of plain text email
