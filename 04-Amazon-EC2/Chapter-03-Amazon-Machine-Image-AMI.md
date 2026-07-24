# 📘 Chapter 03 - Amazon Machine Image (AMI)

> **"An AMI is a pre-configured template used to create EC2 instances."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is an AMI?
- Why AMIs are required
- Components of an AMI
- Types of AMIs
- AMI Architecture
- AMI Creation Process
- AMI vs Snapshot
- Benefits of AMIs
- Interview Questions

---

# 📖 What is an AMI?

**AMI (Amazon Machine Image)** is a pre-configured template that contains everything required to launch an EC2 instance.

It includes:

- Operating System
- Root Volume
- Installed Software
- Configuration
- Permissions
- Root Volume Snapshot

Without an AMI, AWS does not know what type of server to create.

---

# 💡 Simple Definition

Think of an AMI as a **master template**.

Whenever you launch an EC2 instance, AWS first copies the AMI and then creates a new virtual machine.

Just like making photocopies from one original document.

```
Original Template

↓

Copy

↓

Copy

↓

Copy
```

Every EC2 instance starts from the same template.

---

# 🌍 Real-Life Example

Imagine a computer shop.

Whenever a customer buys a laptop, the technician installs:

- Windows
- Drivers
- Microsoft Office
- Antivirus
- Browser

Instead of repeating this every time, the technician creates a **Master Image**.

Now every laptop is installed from that master image.

AWS works exactly the same way.

AMI is that master image.

---

# 🏗️ AMI Architecture

```
                 Amazon Machine Image (AMI)
                           │
      ┌────────────────────┼────────────────────┐
      │                    │                    │
      ▼                    ▼                    ▼
Operating System     Configuration      Root Snapshot
      │                    │                    │
      └────────────────────┼────────────────────┘
                           │
                           ▼
                  Launch EC2 Instance
```

---

# 📦 What Does an AMI Contain?

An AMI contains:

- Operating System
- Root Volume Snapshot
- Installed Applications
- Configuration Files
- Package Updates
- User Settings

Example:

```
Ubuntu 22.04

+

Apache

+

Git

+

Docker

+

Java

=

Custom AMI
```

Whenever this AMI is used, every EC2 instance contains the same software.

---

# 🔄 How AMI Works

```
Create AMI

↓

Store in AWS

↓

Launch EC2

↓

AWS Copies AMI

↓

Creates New EC2 Instance
```

The original AMI never changes.

Every instance is an independent copy.

---

# 📂 Types of AMIs

AWS provides three main types of AMIs.

---

## 1️⃣ AWS Managed AMIs

Created and maintained by AWS.

Examples:

- Amazon Linux
- Ubuntu
- Windows Server
- Red Hat Enterprise Linux
- SUSE Linux

Best for:

- Learning
- Production
- General workloads

---

## 2️⃣ AWS Marketplace AMIs

Created by third-party vendors.

Examples:

- WordPress
- Jenkins
- MongoDB
- SQL Server
- Kubernetes
- NGINX Plus

Useful when software is already installed and configured.

---

## 3️⃣ Custom AMIs

Created by AWS users.

Example:

```
Launch Ubuntu

↓

Install Apache

↓

Install Docker

↓

Configure Website

↓

Create AMI
```

Now multiple servers can be launched with the exact same configuration.

---

# 🔄 AMI Creation Process

```
Launch EC2

↓

Install Software

↓

Configure Server

↓

Create AMI

↓

AMI Stored

↓

Launch Multiple EC2 Instances
```

---

# 📊 AMI vs Snapshot

| AMI | Snapshot |
|-----|----------|
| Complete EC2 template | Backup of an EBS volume |
| Used to launch EC2 | Used to restore storage |
| Includes OS configuration | Stores only volume data |
| Can contain one or more snapshots | Represents a single EBS volume |

---

# 🌍 Example

Suppose you configure a web server.

```
Ubuntu

+

Apache

+

PHP

+

Website Files

↓

Create AMI
```

Now you can launch:

```
EC2 Server 1

EC2 Server 2

EC2 Server 3

EC2 Server 4
```

All servers are identical.

---

# ⭐ Benefits of AMIs

- Faster deployment
- Consistent server configuration
- Easy backup
- Easy disaster recovery
- Saves time
- Supports Auto Scaling
- Reduces manual work

---

# 📖 Key Points

- Every EC2 instance requires an AMI.
- An AMI is a template.
- AMI contains the Operating System and configuration.
- AWS provides managed AMIs.
- Users can create custom AMIs.
- Marketplace provides pre-built software images.

---

# 📝 Quick Revision

```
AMI

↓

Operating System

↓

Software

↓

Configuration

↓

Launch EC2 Instance
```

---

# 🎤 Interview Questions

### 1. What is an AMI?

An Amazon Machine Image (AMI) is a pre-configured template used to launch EC2 instances.

---

### 2. Why is an AMI required?

Without an AMI, AWS does not know which operating system or software should be installed on the EC2 instance.

---

### 3. Name the three types of AMIs.

- AWS Managed AMIs
- AWS Marketplace AMIs
- Custom AMIs

---

### 4. Can one AMI launch multiple EC2 instances?

Yes.

One AMI can launch any number of EC2 instances.

---

### 5. What is the difference between an AMI and a Snapshot?

An AMI is used to create EC2 instances, while a Snapshot is a backup of an EBS volume.

---

# 🎯 Chapter Summary

An Amazon Machine Image (AMI) is the foundation of every EC2 instance. It contains the operating system, software, and configuration needed to launch a virtual server. AWS offers managed AMIs, Marketplace AMIs, and Custom AMIs, making it easy to deploy consistent and scalable infrastructure.
