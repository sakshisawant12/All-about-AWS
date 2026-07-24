# 📘 Chapter 22 - EC2 Interview Questions

> **"Knowledge gets you started. Practical understanding helps you succeed in interviews."**

---

# 🎯 Learning Objectives

After completing this chapter, you will be able to:

- Revise all important EC2 concepts
- Answer common AWS interview questions
- Solve basic troubleshooting scenarios
- Understand real-world EC2 use cases

---

# 🟢 Beginner Level Questions

## 1. What is Amazon EC2?

Amazon EC2 (Elastic Compute Cloud) is a web service that provides scalable virtual servers in the AWS Cloud.

---

## 2. What is an EC2 Instance?

An EC2 Instance is a virtual machine running in AWS.

---

## 3. What is an AMI?

An Amazon Machine Image (AMI) is a template used to launch EC2 instances. It includes the operating system, software, and configuration.

---

## 4. What are Instance Types?

Instance Types define the CPU, memory, storage, and networking capacity of an EC2 instance.

Example:

- t2.micro
- t3.small
- m5.large
- c6i.large

---

## 5. What is a Key Pair?

A Key Pair consists of:

- Public Key (stored by AWS)
- Private Key (.pem or .ppk file stored by the user)

It is used for secure SSH access to Linux EC2 instances.

---

## 6. What is a Security Group?

A Security Group is a virtual firewall that controls inbound and outbound traffic for an EC2 instance.

---

## 7. What is an Elastic IP?

An Elastic IP is a static public IPv4 address that can be associated with an EC2 instance.

---

## 8. What is Amazon EBS?

Amazon Elastic Block Store (EBS) is persistent block storage for EC2 instances.

---

## 9. What is an EBS Snapshot?

An EBS Snapshot is a point-in-time backup of an EBS volume.

---

## 10. What is SSH?

SSH (Secure Shell) is a protocol used to securely connect to Linux servers.

Example:

```bash
ssh -i key.pem ec2-user@Public-IP
```

---

# 🟡 Intermediate Level Questions

## 11. What is the difference between Security Groups and NACLs?

| Security Group | NACL |
|----------------|------|
| Instance Level | Subnet Level |
| Stateful | Stateless |
| Allow Rules Only | Allow & Deny Rules |
| Evaluates All Rules | Evaluates Rules in Order |

---

## 12. What is the difference between EBS and Instance Store?

| Amazon EBS | Instance Store |
|------------|----------------|
| Persistent | Temporary |
| Network Attached | Local Storage |
| Supports Snapshots | No Snapshots |
| Detachable | Not Detachable |

---

## 13. What happens when you stop an EC2 instance?

- Instance shuts down.
- Root EBS volume remains.
- Public IPv4 address (if not Elastic IP) changes on restart.
- Instance Store data is lost.

---

## 14. What happens when you terminate an EC2 instance?

- Instance is permanently deleted.
- Root EBS volume is deleted by default (unless configured otherwise).
- Instance Store data is lost permanently.

---

## 15. What is User Data?

User Data is a script that automatically runs during the first boot of an EC2 instance.

---

## 16. What is an IAM Role?

An IAM Role provides temporary AWS credentials to an EC2 instance without storing access keys.

---

## 17. What is an Elastic Network Interface (ENI)?

An ENI is a virtual network interface that can be attached to an EC2 instance.

---

## 18. What is CloudWatch?

Amazon CloudWatch is the monitoring service used to collect metrics, logs, and alarms for AWS resources.

---

## 19. What is IMDS?

The Instance Metadata Service (IMDS) provides information about the running EC2 instance. IMDSv2 is the recommended version because it improves security.

---

## 20. What is an Availability Zone?

An Availability Zone (AZ) is an isolated data center within an AWS Region.

---

# 🟠 Advanced Level Questions

## 21. Explain the EC2 Instance Lifecycle.

```
Pending
   ↓
Running
   ↓
Stopping
   ↓
Stopped
   ↓
Pending
   ↓
Running
   ↓
Shutting-down
   ↓
Terminated
```

---

## 22. What are Placement Groups?

Placement Groups control how EC2 instances are placed on AWS infrastructure.

Types:

- Cluster
- Spread
- Partition

---

## 23. What are EC2 Purchasing Options?

- On-Demand
- Reserved Instances
- Savings Plans
- Spot Instances
- Dedicated Instances
- Dedicated Hosts
- Capacity Reservations

---

## 24. Which purchasing option offers the lowest cost?

Spot Instances, but they can be interrupted by AWS.

---

## 25. Why use Reserved Instances or Savings Plans?

To reduce costs for long-running, predictable workloads.

---

## 26. What is the difference between Dedicated Instances and Dedicated Hosts?

Dedicated Instances run on hardware dedicated to one customer but without host-level control. Dedicated Hosts provide an entire physical server with visibility into sockets and cores, making them useful for licensing and compliance.

---

## 27. Can an EBS volume be attached to multiple EC2 instances?

Generally, an EBS volume can be attached to only one EC2 instance at a time. (Certain io1/io2 Multi-Attach volumes are an exception for supported instance types and workloads.)

---

## 28. Can you move an EBS volume to another Availability Zone?

Not directly.

Create a Snapshot, then create a new EBS volume from that Snapshot in the target Availability Zone.

---

## 29. Why should SSH not be open to `0.0.0.0/0`?

Because it exposes the server to the internet, increasing the risk of unauthorized access.

---

## 30. Why use IAM Roles instead of Access Keys?

IAM Roles provide temporary credentials that are automatically rotated, improving security and eliminating the need to store long-term access keys.

---

# 🔵 Scenario-Based Questions

## 31. Your website is not loading. What do you check?

- EC2 instance state
- Security Group rules (HTTP/HTTPS)
- Web server status (`systemctl status httpd` or `nginx`)
- Network ACLs
- Route Table
- Internet Gateway
- Application logs

---

## 32. SSH connection times out. What could be wrong?

- Port 22 not allowed
- Wrong Security Group
- Incorrect key pair
- Instance has no public IP or Elastic IP
- Route table or Internet Gateway issue
- Network ACL blocking traffic

---

## 33. You accidentally deleted a file from an EBS volume. How can you recover it?

Restore from an EBS Snapshot if one exists.

---

## 34. CPU utilization stays above 90%. What should you do?

- Investigate the workload.
- Resize to a larger instance if needed.
- Optimize the application.
- Consider Auto Scaling for variable workloads.

---

## 35. Your company needs a static public IP. Which AWS service would you use?

Elastic IP.

---

## 36. Which storage should host a production database?

Amazon EBS.

---

## 37. Which storage is best for temporary cache files?

Instance Store.

---

## 38. Which service monitors EC2 metrics?

Amazon CloudWatch.

---

## 39. Which service protects EC2 at the instance level?

Security Groups.

---

## 40. Which AWS feature automatically launches more EC2 instances during high traffic?

Auto Scaling.

---

# 🟣 Practical Linux Questions

## 41. Check attached disks

```bash
lsblk
```

---

## 42. Check mounted file systems

```bash
df -h
```

---

## 43. Check Apache status

```bash
sudo systemctl status httpd
```

---

## 44. Restart Apache

```bash
sudo systemctl restart httpd
```

---

## 45. Display current user

```bash
whoami
```

---

## 46. Check IP address

```bash
ip addr
```

---

## 47. View running processes

```bash
ps -ef
```

---

## 48. Check memory usage

```bash
free -h
```

---

## 49. Check disk usage

```bash
du -sh /var/www/html
```

---

## 50. Update packages

Amazon Linux 2023:

```bash
sudo dnf update -y
```

Ubuntu:

```bash
sudo apt update && sudo apt upgrade -y
```

---

# ⭐ EC2 Quick Revision Sheet

| Topic | Key Point |
|--------|-----------|
| EC2 | Virtual Machine |
| AMI | Instance Template |
| Instance Type | CPU + Memory + Network |
| Security Group | Stateful Firewall |
| NACL | Stateless Firewall |
| EBS | Persistent Storage |
| Instance Store | Temporary Storage |
| Snapshot | EBS Backup |
| Elastic IP | Static Public IPv4 |
| ENI | Virtual Network Interface |
| IAM Role | Temporary AWS Credentials |
| IMDSv2 | Secure Instance Metadata |
| CloudWatch | Monitoring & Alarms |
| Placement Groups | Instance Placement Strategy |
| Spot Instance | Lowest Cost, Interruptible |
| Savings Plans | Flexible Long-Term Discounts |
| Reserved Instance | Discount for Predictable Usage |

---

