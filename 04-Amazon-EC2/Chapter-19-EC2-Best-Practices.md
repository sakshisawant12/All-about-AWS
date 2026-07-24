# 📘 Chapter 19 - EC2 Best Practices

> **"Launching an EC2 instance is easy. Running it securely, reliably, and cost-effectively in production is what makes a good Cloud Engineer."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- EC2 Security Best Practices
- Cost Optimization
- Performance Optimization
- Storage Best Practices
- Monitoring Best Practices
- Backup & Disaster Recovery
- High Availability
- Common Mistakes
- Production Checklist
- Interview Questions

---

# 📖 Why Are Best Practices Important?

Launching an EC2 instance is only the first step.

A production server must be:

- Secure
- Highly Available
- Cost Efficient
- Reliable
- Scalable
- Easy to Monitor

Following AWS best practices reduces downtime, improves security, and helps control costs.

---

# 🛡️ 1. Security Best Practices

### ✅ Use IAM Roles

```
EC2

↓

IAM Role

↓

AWS Services
```

Never store AWS Access Keys inside an EC2 instance.

---

### ✅ Follow the Principle of Least Privilege

Grant only the permissions that an application actually needs.

Example:

```
Application

↓

Read Only Access

↓

Amazon S3
```

Not:

```
AdministratorAccess
```

unless absolutely necessary.

---

### ✅ Restrict Security Groups

Instead of:

```
SSH

0.0.0.0/0
```

Prefer:

```
SSH

Your Office IP

or

VPN IP
```

Open only the required ports.

---

### ✅ Enable IMDSv2

Use **IMDSv2** instead of IMDSv1 for improved protection against SSRF attacks.

---

### ✅ Keep the Operating System Updated

Regularly install:

- Security patches
- Kernel updates
- Package updates

Example:

```bash
sudo yum update -y
```

or

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 💰 2. Cost Optimization

### Stop Unused Instances

Development servers don't need to run 24×7.

```
Working Hours

↓

Running

↓

After Office

↓

Stop Instance
```

---

### Choose the Right Instance Type

Don't run a large instance if a smaller one meets your workload requirements.

---

### Use Savings Plans or Reserved Instances

For long-running production workloads:

- Savings Plans
- Reserved Instances

These typically reduce costs compared to On-Demand pricing.

---

### Use Spot Instances

Ideal for:

- Batch processing
- Testing
- Rendering
- Data analytics

---

### Delete Unused Resources

Review and remove:

- Unused EBS volumes
- Old Snapshots
- Unused Elastic IPs
- Idle Load Balancers

---

# ⚡ 3. Performance Best Practices

### Select the Correct Instance Family

Examples:

| Workload | Instance Family |
|-----------|-----------------|
| General Purpose | T, M |
| Compute Intensive | C |
| Memory Intensive | R, X |
| Machine Learning | P, G |
| Storage Intensive | I, D |

---

### Use gp3 for General EBS Workloads

gp3 volumes provide good performance with flexible configuration for most workloads.

---

### Use Placement Groups When Appropriate

| Requirement | Placement Group |
|-------------|-----------------|
| Lowest Latency | Cluster |
| Fault Isolation | Partition |
| Maximum Availability | Spread |

---

### Monitor Resource Utilization

Track:

- CPU
- Memory (CloudWatch Agent)
- Disk
- Network
- Status Checks

---

# 💾 4. Storage Best Practices

### Use Amazon EBS for Important Data

Suitable for:

- Operating systems
- Databases
- Applications
- Persistent storage

---

### Take Regular Snapshots

```
EBS

↓

Snapshot

↓

Amazon S3 (managed internally by AWS)
```

Snapshots help recover data after accidental deletion or corruption.

---

### Encrypt Sensitive Volumes

Use AWS KMS to encrypt EBS volumes storing confidential data.

---

### Don't Store Critical Data on Instance Store

Instance Store is temporary storage.

---

# 📊 5. Monitoring Best Practices

### Enable CloudWatch

Monitor:

- CPU
- Network
- Disk Metrics
- Status Checks

---

### Create CloudWatch Alarms

Example:

```
CPU > 80%

↓

CloudWatch Alarm

↓

Amazon SNS

↓

Email Notification
```

---

### Collect Logs

Use the CloudWatch Agent to send:

- System logs
- Application logs
- Web server logs

to CloudWatch Logs.

---

# 🔄 6. Backup & Disaster Recovery

### Use EBS Snapshots

Create backups regularly.

```
EBS

↓

Snapshot

↓

Restore Anytime
```

---

### Test Backup Restoration

A backup is valuable only if it can be restored successfully.

---

### Use Multiple Availability Zones

Distribute critical workloads across Availability Zones for higher availability.

---

# 🌐 7. High Availability Best Practices

For production applications:

```
Users

↓

Application Load Balancer

↓

EC2 (AZ-1)

↓

EC2 (AZ-2)
```

Benefits:

- Fault tolerance
- Improved uptime
- Better user experience

---

### Enable Auto Scaling

Automatically:

- Launch new instances when demand increases
- Remove instances when demand decreases

---

# 📋 Production Deployment Checklist

| Task | Status |
|------|--------|
| IAM Role Attached | ✅ |
| IMDSv2 Enabled | ✅ |
| Security Groups Reviewed | ✅ |
| EBS Encrypted | ✅ |
| Backups Configured | ✅ |
| CloudWatch Enabled | ✅ |
| CloudWatch Alarms Created | ✅ |
| Elastic IP Used (If Needed) | ✅ |
| Auto Scaling Configured | ✅ |
| Load Balancer Configured | ✅ |

---

# 🌍 Real-World Architecture

```
                  Users
                     │
                     ▼
          Application Load Balancer
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
     EC2 Instance          EC2 Instance
      (AZ-1)                (AZ-2)
          │                     │
          └──────────┬──────────┘
                     ▼
               Amazon EBS
                     │
                     ▼
          CloudWatch Monitoring
```

---

# ❌ Common Mistakes

- Using the root user for daily work.
- Hardcoding AWS Access Keys in applications.
- Opening Security Groups to `0.0.0.0/0` unnecessarily.
- Ignoring CloudWatch alarms.
- Running oversized EC2 instances.
- Forgetting EBS backups.
- Storing important data on Instance Store.
- Leaving unused Elastic IPs allocated.

---

# 📝 Quick Revision

```
Security
↓

IAM Roles

↓

Least Privilege

↓

Security Groups

--------------------

Performance

↓

Correct Instance Type

↓

Placement Groups

--------------------

Storage

↓

Amazon EBS

↓

Snapshots

--------------------

Monitoring

↓

CloudWatch

↓

Alarms

--------------------

Availability

↓

Load Balancer

↓

Auto Scaling
```

---

# 🎤 Interview Questions

### 1. What is the most secure way for an EC2 instance to access AWS services?

Using an **IAM Role** with temporary credentials.

---

### 2. Which service is commonly used to monitor EC2 instances?

**Amazon CloudWatch**

---

### 3. What is the recommended storage type for production databases?

**Amazon EBS**

---

### 4. Why should IMDSv2 be enabled?

It improves security by requiring session tokens, helping protect against SSRF attacks.

---

### 5. Which AWS services help improve EC2 availability?

- Application Load Balancer (ALB)
- Auto Scaling
- Multiple Availability Zones

---

### 6. What is the purpose of EBS Snapshots?

To create backups that can be restored if data is lost or corrupted.

---

# 🎯 Chapter Summary

Running EC2 instances in production requires more than simply launching a virtual machine. By following AWS best practices—using IAM Roles, securing Security Groups, enabling IMDSv2, monitoring with CloudWatch, backing up EBS volumes, optimizing costs with the right purchasing options, and designing for high availability using Load Balancers and Auto Scaling—you can build secure, reliable, scalable, and cost-efficient cloud infrastructure.
