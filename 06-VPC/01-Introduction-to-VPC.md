# Amazon VPC (Virtual Private Cloud)

## What is Amazon VPC?

Amazon Virtual Private Cloud (Amazon VPC) is a service that allows you to create a private, isolated virtual network inside AWS where you can launch and manage your AWS resources securely.

Think of a VPC as your own private data center in the AWS Cloud. You have full control over networking, including IP address ranges, subnets, routing, and security.

---

## Why Do We Need a VPC?

Without a VPC, all AWS resources would exist in a shared environment with no network isolation.

A VPC helps you:

- Isolate your resources from other AWS customers.
- Control inbound and outbound network traffic.
- Design secure cloud architectures.
- Connect your AWS environment with on-premises networks.
- Organize applications into public and private networks.

---

## Real-World Example

Imagine a company has:

- Public website
- Application server
- Database server

The website must be accessible from the internet.

The application server should only communicate with the website.

The database should never be accessible from the internet.

A VPC makes this possible by creating separate networks and controlling how traffic flows between them.

---

## What Can You Control in a VPC?

- IP Address Range (CIDR)
- Public and Private Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Security Groups
- Network ACLs
- Elastic Network Interfaces
- VPN Connections
- Direct Connect
- VPC Endpoints
- Flow Logs

---

## Key Features of Amazon VPC

- Logically isolated network
- Fully customizable networking
- IPv4 and IPv6 support
- High availability across Availability Zones
- Secure communication
- Scalability
- Integration with most AWS services

---

## Default VPC vs Custom VPC

### Default VPC

Created automatically when your AWS account is created.

Features:

- Ready to use
- Internet Gateway already attached
- Default Route Table available
- Default Security Group available
- Public subnets in every Availability Zone
- Instances can access the internet immediately (if assigned a public IP)

Best for:

- Beginners
- Testing
- Learning

---

### Custom VPC

Created manually by the user.

Features:

- Choose your own CIDR block
- Create public and private subnets
- Configure route tables
- Attach Internet Gateway manually
- Configure NAT Gateway
- Full control over security

Best for:

- Production
- Enterprise environments
- Secure applications

---

## Basic VPC Architecture

                Internet
                    │
            Internet Gateway
                    │
          ┌───────────────────┐
          │       VPC         │
          │                   │
          │ Public Subnet     │
          │     EC2           │
          │                   │
          │ Private Subnet    │
          │     Database      │
          └───────────────────┘

---

## Important Characteristics

- A VPC belongs to one AWS Region.
- Subnets belong to one Availability Zone.
- Resources inside the same VPC can communicate using private IP addresses.
- Different VPCs cannot communicate unless connected (e.g., VPC Peering or Transit Gateway).
- A VPC can contain multiple subnets.

---

## Common AWS Resources Inside a VPC

- EC2
- RDS
- Lambda (when configured)
- ECS
- EKS
- Elastic Load Balancer
- NAT Gateway
- Bastion Host

Some AWS services like S3 and IAM are not launched inside a VPC.

---

## Advantages of Amazon VPC

- Better security
- Network isolation
- Flexible IP addressing
- Controlled internet access
- Hybrid cloud support
- Easy scalability
- High availability
- Fine-grained traffic control

---

## Limitations

- A VPC is limited to a single AWS Region.
- Internet access requires an Internet Gateway.
- Private subnets require a NAT Gateway or similar solution for outbound internet access.
- Default service quotas apply (can often be increased).

---

## Best Practices

- Use Custom VPCs for production.
- Keep databases in private subnets.
- Use Security Groups with least privilege.
- Use Network ACLs for an additional security layer.
- Spread workloads across multiple Availability Zones.
- Enable VPC Flow Logs for monitoring.
- Use VPC Endpoints for private access to AWS services when possible.

---

## Interview Questions

### 1. What is Amazon VPC?

Amazon VPC is a logically isolated virtual network in AWS where you can securely launch and manage AWS resources.

---

### 2. Why is VPC important?

It provides network isolation, security, routing control, and customizable networking for AWS resources.

---

### 3. What is the difference between a Default VPC and a Custom VPC?

Default VPC is automatically created and preconfigured, while a Custom VPC is manually created and offers complete networking control.

---

### 4. Can a VPC span multiple Regions?

No. A VPC is limited to a single AWS Region.

---

### 5. Can a subnet span multiple Availability Zones?

No. A subnet exists in only one Availability Zone.

---

## Key Takeaways

- Amazon VPC is your private network in AWS.
- It gives you complete control over networking.
- It improves security through isolation.
- It supports both public and private architectures.
- It is the foundation of almost every AWS cloud deployment.
