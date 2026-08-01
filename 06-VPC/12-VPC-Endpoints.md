# Amazon VPC - VPC Endpoints

## What is a VPC Endpoint?

A VPC Endpoint is a service that allows resources inside your VPC to connect privately to supported AWS services without using an Internet Gateway, NAT Gateway, VPN, or Direct Connect.

Traffic stays entirely on the AWS private network.

---

# Why Do We Need VPC Endpoints?

Suppose a private EC2 instance wants to access an S3 bucket.

Without a VPC Endpoint:

Private EC2

↓

NAT Gateway

↓

Internet Gateway

↓

Amazon S3

Even though S3 is an AWS service, the traffic must go through the NAT Gateway.

With a VPC Endpoint:

Private EC2

↓

VPC Endpoint

↓

Amazon S3

The traffic never leaves AWS's private network.

---

# Benefits of VPC Endpoints

- Private communication with AWS services
- Improved security
- Reduced NAT Gateway usage
- Lower data transfer costs
- Better performance
- Traffic stays on the AWS backbone

---

# Types of VPC Endpoints

AWS provides two main types.

## 1. Gateway Endpoint

Supported Services:

- Amazon S3
- Amazon DynamoDB

Gateway Endpoints are free to use.

They are added to Route Tables.

---

## 2. Interface Endpoint

Supports most AWS services such as:

- EC2 API
- CloudWatch
- Systems Manager (SSM)
- Secrets Manager
- KMS
- ECR
- SNS
- SQS
- Many partner services

An Interface Endpoint creates one or more Elastic Network Interfaces (ENIs) inside your subnet.

Interface Endpoints have hourly and data processing charges.

---

# Gateway Endpoint

Traffic Flow:

Private EC2

↓

Route Table

↓

Gateway Endpoint

↓

Amazon S3

Characteristics:

- Uses Route Tables
- No ENI created
- Free
- Supports only S3 and DynamoDB

---

# Interface Endpoint

Traffic Flow:

Private EC2

↓

Private ENI

↓

AWS Service

Characteristics:

- Creates ENIs
- Uses Security Groups
- Private DNS supported
- Supports many AWS services

---

# Gateway Endpoint vs Interface Endpoint

| Feature | Gateway Endpoint | Interface Endpoint |
|---------|------------------|--------------------|
|Supported Services|S3, DynamoDB|Most AWS services|
|Uses Route Table|Yes|No|
|Creates ENI|No|Yes|
|Uses Security Groups|No|Yes|
|Cost|Free|Hourly + Data Processing|
|Private DNS|No|Yes (optional)|

---

# Private DNS

Interface Endpoints support Private DNS.

Example:

Normally:

s3.amazonaws.com

↓

Public endpoint

With Private DNS enabled:

s3.amazonaws.com

↓

Private IP inside your VPC

Applications do not need any code changes.

---

# Endpoint Policy

A VPC Endpoint can have its own IAM policy.

Example:

Allow access only to:

my-company-backups

Deny access to all other buckets.

This adds another layer of security.

---

# Example Architecture

                    AWS Cloud

Private EC2

↓

VPC Endpoint

↓

Amazon S3

No Internet Gateway

No NAT Gateway

No Public IP

Completely private communication.

---

# Real-World Example

Application Server

↓

Interface Endpoint

↓

AWS Secrets Manager

↓

Retrieve Database Password

The password is retrieved privately without exposing traffic to the internet.

---

# Common Use Cases

### Gateway Endpoint

- S3 backups
- S3 log storage
- DynamoDB access

### Interface Endpoint

- Systems Manager
- CloudWatch
- ECR image pulls
- Secrets Manager
- KMS
- SNS
- SQS

---

# Best Practices

- Use Gateway Endpoints for S3 and DynamoDB whenever possible.
- Use Interface Endpoints for sensitive AWS service communication.
- Enable Private DNS when appropriate.
- Apply least-privilege endpoint policies.
- Restrict Interface Endpoints with Security Groups.

---

# Common Mistakes

❌ Using a NAT Gateway for S3 when a Gateway Endpoint is sufficient.

❌ Forgetting Route Table updates for Gateway Endpoints.

❌ Forgetting Security Group rules for Interface Endpoints.

❌ Assuming all AWS services support Gateway Endpoints.

---

# Troubleshooting

Cannot access S3?

Check:

✅ Gateway Endpoint created.

✅ Route Table associated.

✅ Bucket Policy.

✅ IAM permissions.

Cannot access Systems Manager?

Check:

✅ Interface Endpoint exists.

✅ Security Groups allow HTTPS (443).

✅ Private DNS enabled (if required).

---

# Interview Questions

### What is a VPC Endpoint?

A VPC Endpoint allows private communication between resources in a VPC and supported AWS services without using the public internet.

---

### Which services support Gateway Endpoints?

- Amazon S3
- Amazon DynamoDB

---

### Which services use Interface Endpoints?

Most AWS services, including:

- Systems Manager
- CloudWatch
- Secrets Manager
- ECR
- KMS
- SNS
- SQS

---

### Does a Gateway Endpoint create an ENI?

No.

Only Interface Endpoints create ENIs.

---

### Are Gateway Endpoints free?

Yes.

Gateway Endpoints for Amazon S3 and DynamoDB are free.

---

### Can Interface Endpoints use Security Groups?

Yes.

Because they create ENIs, Security Groups can be attached.

---

# Key Takeaways

- VPC Endpoints provide private access to AWS services.
- Traffic stays on AWS's private backbone.
- Gateway Endpoints support only S3 and DynamoDB.
- Interface Endpoints support most AWS services.
- Gateway Endpoints are free.
- Interface Endpoints create ENIs and support Security Groups.
- VPC Endpoints improve both security and cost efficiency.
