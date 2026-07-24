# 📘 Chapter 02 - EC2 Components

> **"An EC2 instance is not just a virtual machine. It is a combination of multiple AWS resources working together."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What are EC2 Components?
- Why EC2 requires multiple AWS resources
- Purpose of each EC2 component
- How these components work together
- EC2 architecture
- EC2 launch workflow

---

# 📖 What are EC2 Components?

When you launch an EC2 instance, AWS doesn't simply create a virtual machine.

Instead, AWS combines several services and resources to build a complete server.

Each component has a specific responsibility such as:

- Providing the Operating System
- Providing CPU and Memory
- Providing Storage
- Providing Networking
- Providing Security
- Providing AWS Permissions

Together, these resources form one EC2 Instance.

---

# 💡 Simple Example

Think about buying a new laptop.

A laptop is not just a processor.

It also requires:

- Operating System
- RAM
- Storage
- Keyboard
- Network Card
- Password

Only after all these components are combined does it become a usable computer.

Similarly, an EC2 Instance is created using multiple AWS resources.

---

# 🏗️ EC2 Components Overview

```
                 Amazon EC2 Instance
                         │
 ┌──────────────┬─────────┼───────────┬──────────────┐
 │              │         │           │              │
 ▼              ▼         ▼           ▼              ▼
AMI      Instance Type  EBS      Security Group   Key Pair
 │              │         │           │              │
 │              │         │           │              │
Operating     CPU &     Storage    Firewall     Secure Login
System         RAM
 │
 ▼
IAM Role (Optional)
 │
AWS Permissions
 │
 ▼
Network Interface (ENI)
 │
Private/Public IP Address
```

---

# 📦 Main Components of an EC2 Instance

## 1️⃣ Amazon Machine Image (AMI)

An **AMI (Amazon Machine Image)** is a template used to launch an EC2 instance.

It contains:

- Operating System
- Root Volume Snapshot
- Software
- Configuration

Without an AMI, AWS doesn't know which operating system to install.

### Example

```
Ubuntu AMI

↓

Launch Instance

↓

Ubuntu Linux Server
```

---

## 2️⃣ Instance Type

An Instance Type defines the hardware configuration of your virtual machine.

It specifies:

- vCPU
- Memory (RAM)
- Network Performance
- Storage Performance

Example:

```
t3.micro

↓

2 vCPU

↓

1 GB RAM
```

Different workloads require different instance types.

---

## 3️⃣ Amazon EBS Volume

Amazon EBS provides **persistent storage** for an EC2 instance.

It stores:

- Operating System
- Applications
- Files
- Databases
- Logs

Without storage, an operating system cannot run.

Example:

```
EC2 Instance

↓

30 GB gp3 EBS Volume

↓

Ubuntu Installed
```

---

## 4️⃣ Security Group

A Security Group acts as a **virtual firewall**.

It controls:

- Inbound Traffic
- Outbound Traffic

Example:

```
Internet

↓

Port 22 (SSH)

↓

Port 80 (HTTP)

↓

EC2 Instance
```

Only allowed traffic can reach the instance.

---

## 5️⃣ Key Pair

A Key Pair provides secure authentication to connect to an EC2 instance.

For Linux:

- SSH
- Private Key (.pem)

For Windows:

- RDP
- Password decrypted using Private Key

Without the private key, you cannot securely log in to most Linux EC2 instances.

---

## 6️⃣ Network Interface (ENI)

Every EC2 instance has at least one **Elastic Network Interface (ENI).**

It provides:

- Private IP Address
- Public IP Address (Optional)
- MAC Address
- Security Group Association

Think of it as the network card of your virtual machine.

---

## 7️⃣ IAM Role (Optional)

IAM Roles allow an EC2 instance to securely access AWS services without storing Access Keys.

Example:

```
EC2

↓

IAM Role

↓

Amazon S3
```

Instead of saving credentials inside the server, AWS automatically provides temporary credentials.

---

## 8️⃣ User Data (Optional)

User Data is a startup script that automatically executes when an EC2 instance launches for the first time.

It is commonly used to:

- Install Packages
- Configure Servers
- Deploy Applications
- Create Users
- Update the Operating System

Example:

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl enable httpd
systemctl start httpd
```

---

# 🔄 How EC2 Components Work Together

```
User
 │
 ▼
Launch EC2 Instance
 │
 ▼
Select AMI
 │
 ▼
Choose Instance Type
 │
 ▼
Attach EBS Volume
 │
 ▼
Configure Network (ENI)
 │
 ▼
Attach Security Group
 │
 ▼
Create/Select Key Pair
 │
 ▼
(Optional) Attach IAM Role
 │
 ▼
Launch EC2 Instance
```

Every component plays an important role during instance creation.

---

# 🌍 Real-World Example

Imagine building a house.

| House Component | EC2 Component |
|-----------------|---------------|
| House Design | AMI |
| Size of House | Instance Type |
| Storage Room | EBS Volume |
| Main Gate | Security Group |
| House Key | Key Pair |
| Electricity Connection | Network Interface |
| Security Guard | IAM Role |

A house is complete only when all these components work together.

Similarly, an EC2 instance requires multiple AWS resources.

---

# 📊 EC2 Components Summary

| Component | Purpose |
|-----------|---------|
| AMI | Operating System Template |
| Instance Type | CPU & Memory |
| EBS Volume | Persistent Storage |
| Security Group | Virtual Firewall |
| Key Pair | Secure Login |
| Network Interface (ENI) | Network Connectivity |
| IAM Role | AWS Service Permissions |
| User Data | Startup Automation |

---

# 📖 Key Points

- An EC2 Instance is built using multiple AWS resources.
- AMI provides the Operating System.
- Instance Type provides CPU and Memory.
- EBS provides storage.
- Security Groups control network traffic.
- Key Pairs enable secure login.
- IAM Roles securely access AWS services.
- ENI connects the instance to the network.
- User Data automates server setup.

---

# 📝 Quick Revision

```
AMI
↓

Instance Type
↓

EBS Volume
↓

Security Group
↓

Key Pair
↓

Network Interface

↓

IAM Role (Optional)

↓

User Data (Optional)

↓

EC2 Instance
```

---

# 🎤 Interview Questions

### 1. What are the main components of an EC2 instance?

- AMI
- Instance Type
- EBS Volume
- Security Group
- Key Pair
- Network Interface (ENI)
- IAM Role
- User Data

---

### 2. Which component provides the Operating System?

**Amazon Machine Image (AMI).**

---

### 3. Which component stores the data?

**Amazon EBS Volume.**

---

### 4. Which component acts as a virtual firewall?

**Security Group.**

---

### 5. Which component allows secure login to a Linux EC2 instance?

**Key Pair (.pem file).**

---

### 6. Which component allows an EC2 instance to access AWS services securely?

**IAM Role.**

---

### 7. Which component provides network connectivity?

**Elastic Network Interface (ENI).**

---

# 🎯 Chapter Summary

In this chapter, you learned that an EC2 instance is not just a virtual machine but a combination of several AWS resources. Each component has a specific responsibility, such as providing the operating system, compute power, storage, networking, security, or AWS permissions. Understanding these components makes it much easier to learn EC2 instance creation, networking, storage, and security in the upcoming chapters.
