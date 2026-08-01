# Amazon VPC - Hands-On Lab

## Objective

Build a secure AWS network with:

- Custom VPC
- Public Subnet
- Private Subnet
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Network ACL
- Public EC2
- Private EC2

---

# Architecture

                    Internet
                        │
                Internet Gateway
                        │
               Public Route Table
                        │
          Public Subnet (10.0.1.0/24)
                        │
             Public EC2 (Bastion/Web)
                        │
                  NAT Gateway
                        │
              Private Route Table
                        │
         Private Subnet (10.0.2.0/24)
                        │
             Private EC2 (App)

---

# Lab 1 - Create a Custom VPC

CIDR:

10.0.0.0/16

Name:

My-VPC

Verify:

- VPC created successfully.

---

# Lab 2 - Create Public Subnet

CIDR:

10.0.1.0/24

Enable:

Auto-assign Public IPv4

Availability Zone:

Any

---

# Lab 3 - Create Private Subnet

CIDR:

10.0.2.0/24

Do NOT enable Public IP assignment.

---

# Lab 4 - Create Internet Gateway

- Create IGW
- Attach it to My-VPC

Verify:

Status = Attached

---

# Lab 5 - Create Route Tables

## Public Route Table

Routes:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

Associate:

Public Subnet

---

## Private Route Table

Initially:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|

Associate:

Private Subnet

---

# Lab 6 - Launch Public EC2

Configuration:

- Amazon Linux
- Public Subnet
- Auto-assign Public IP = Enabled

Security Group:

- SSH (22) → Your IP
- HTTP (80) → 0.0.0.0/0

Connect:

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

Verify:

```bash
ping google.com
```

Should work.

---

# Lab 7 - Create NAT Gateway

- Allocate Elastic IP
- Create NAT Gateway
- Deploy in Public Subnet

Wait until:

Available

---

# Lab 8 - Update Private Route Table

Add:

| Destination | Target |
|-------------|--------|
|0.0.0.0/0|NAT Gateway|

---

# Lab 9 - Launch Private EC2

Configuration:

- Amazon Linux
- Private Subnet
- No Public IP

Security Group:

Allow SSH only from the Bastion Host Security Group.

---

# Lab 10 - Connect to Private EC2

SSH to Public EC2:

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

Then:

```bash
ssh ec2-user@<Private-IP>
```

Verify:

```bash
hostname

ip addr
```

---

# Lab 11 - Verify Internet Access

Run:

```bash
sudo dnf update -y
```

or

```bash
curl https://aws.amazon.com
```

Expected:

Internet works through the NAT Gateway.

---

# Lab 12 - Test Security Groups

Try:

SSH from another IP.

Expected:

Connection denied.

Try:

SSH from your allowed IP.

Expected:

Connection successful.

---

# Lab 13 - Test Network ACL

Block:

Port 80

Expected:

Website becomes inaccessible.

Remove rule.

Website works again.

---

# Lab 14 - Create S3 Gateway Endpoint

Create:

Gateway Endpoint

Service:

Amazon S3

Associate:

Private Route Table

Verify:

Private EC2 can access S3 without using the NAT Gateway.

---

# Lab 15 - Enable VPC Flow Logs

Destination:

CloudWatch Logs

Generate traffic:

- SSH
- HTTP
- Ping

Verify:

Flow Logs record ACCEPT and REJECT entries.

---

# Final Architecture Checklist

✔ Custom VPC

✔ Public Subnet

✔ Private Subnet

✔ Internet Gateway

✔ NAT Gateway

✔ Route Tables

✔ Security Groups

✔ Network ACL

✔ Public EC2

✔ Private EC2

✔ VPC Endpoint

✔ VPC Flow Logs

---

# Learning Outcomes

After completing this lab, you should be able to:

- Design a secure VPC.
- Configure public and private networking.
- Enable secure internet access.
- Use NAT Gateway.
- Configure Security Groups and NACLs.
- Connect to private EC2 instances.
- Create VPC Endpoints.
- Monitor traffic using VPC Flow Logs.
- Troubleshoot common networking issues.

---

# Challenge Exercises

1. Add another private subnet in a different Availability Zone.

2. Create a second NAT Gateway for High Availability.

3. Launch a second EC2 and verify communication using private IPs.

4. Create a VPC Peering connection between two VPCs.

5. Connect an S3 bucket using a Gateway Endpoint.

6. Restrict SSH using Security Group references.

7. Enable Flow Logs and identify rejected traffic.
