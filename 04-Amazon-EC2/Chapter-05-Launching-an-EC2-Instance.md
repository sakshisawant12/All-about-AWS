# 📘 Chapter 05 - Launching an EC2 Instance

> **"Launching an EC2 instance means creating a virtual server by selecting an operating system, hardware configuration, storage, networking, and security settings."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- How to launch an EC2 instance
- EC2 Launch Wizard
- Every option available during launch
- Best practices
- Common mistakes
- Complete launch workflow
- Interview Questions

---

# 📖 What Does "Launch an EC2 Instance" Mean?

Launching an EC2 instance means creating a new virtual server inside AWS.

During the launch process, AWS asks you to configure:

- Name
- Operating System (AMI)
- Instance Type
- Key Pair
- Network
- Security Group
- Storage
- Advanced Settings

Once completed, AWS creates your virtual machine within a few minutes.

---

# 🏗️ EC2 Launch Workflow

```
Open AWS Console

        │
        ▼

Amazon EC2

        │
        ▼

Launch Instance

        │
        ▼

Choose Name

        │
        ▼

Choose AMI

        │
        ▼

Choose Instance Type

        │
        ▼

Select Key Pair

        │
        ▼

Configure Network

        │
        ▼

Configure Security Group

        │
        ▼

Configure Storage

        │
        ▼

Advanced Details (Optional)

        │
        ▼

Launch Instance
```

---

# 🚀 Step 1 - Open EC2 Dashboard

Login to the AWS Management Console.

Navigate to:

```
AWS Console

↓

Services

↓

EC2

↓

Launch Instance
```

This opens the EC2 Launch Wizard.

---

# 🏷️ Step 2 - Enter Instance Name

Give your EC2 instance a meaningful name.

Example:

```
WebServer

Ubuntu-Server

Production-App

Testing-Server
```

The name is only for identification in the AWS Console.

It does not affect the instance's functionality.

---

# 💿 Step 3 - Select an Amazon Machine Image (AMI)

Choose the operating system for your virtual machine.

Examples:

- Amazon Linux
- Ubuntu
- Red Hat Enterprise Linux
- Windows Server

Example:

```
Ubuntu Server 24.04 LTS
```

AWS creates the EC2 instance using the selected AMI.

---

# ⚙️ Step 4 - Choose an Instance Type

Select the hardware configuration.

Example:

```
t3.micro
```

This defines:

- CPU
- RAM
- Network Performance

For practice, choose a Free Tier eligible instance type (if your account qualifies).

---

# 🔑 Step 5 - Select or Create a Key Pair

A Key Pair is required to securely connect to the instance.

Options:

- Create New Key Pair
- Select Existing Key Pair

AWS downloads a private key file (for example, `.pem`) when you create a new key pair.

Keep this file safe because it cannot be downloaded again.

---

# 🌐 Step 6 - Configure Network Settings

Choose where your EC2 instance will run.

Network settings include:

- VPC
- Subnet
- Auto-assign Public IP
- Firewall (Security Group)

Example:

```
VPC

↓

Public Subnet

↓

Public IP Enabled
```

---

# 🛡️ Step 7 - Configure Security Group

A Security Group controls inbound and outbound traffic.

Common inbound rules:

| Port | Protocol | Purpose |
|------|----------|----------|
| 22 | SSH | Linux Remote Login |
| 80 | HTTP | Website Access |
| 443 | HTTPS | Secure Website |
| 3389 | RDP | Windows Remote Login |

Example:

```
Internet

↓

Port 80

↓

Security Group

↓

EC2
```

Only allowed traffic reaches the instance.

---

# 💾 Step 8 - Configure Storage

Attach an Amazon EBS volume.

Example:

```
30 GB

gp3

Encrypted (Optional)
```

The EBS volume stores:

- Operating System
- Applications
- Files
- Logs

---

# ⚙️ Step 9 - Advanced Details (Optional)

Advanced settings include:

- IAM Role
- User Data Script
- Shutdown Behavior
- Detailed Monitoring
- Termination Protection

These are optional but useful for automation and production environments.

---

# 🚀 Step 10 - Launch Instance

Click:

```
Launch Instance
```

AWS starts creating the virtual machine.

The instance state changes as follows:

```
Pending

↓

Running
```

Once the status is **Running**, the server is ready to use.

---

# 🌍 Complete Launch Process

```
Launch Instance

        │
        ▼

Name

        │
        ▼

AMI

        │
        ▼

Instance Type

        │
        ▼

Key Pair

        │
        ▼

Network

        │
        ▼

Security Group

        │
        ▼

Storage

        │
        ▼

Advanced Details

        │
        ▼

Launch

        │
        ▼

Running Instance
```

---

# 📌 Best Practices

- Use meaningful instance names.
- Choose the correct AMI.
- Select the appropriate instance type.
- Never lose your private key.
- Allow only required ports in the Security Group.
- Use IAM Roles instead of storing AWS Access Keys.
- Enable monitoring for production workloads.
- Delete unused instances to reduce costs.

---

# ⚠️ Common Mistakes

- Losing the private key file.
- Opening all ports (`0.0.0.0/0`) unnecessarily.
- Choosing an incorrect instance type.
- Forgetting to stop or terminate unused instances.
- Attaching larger EBS volumes than required.
- Launching resources in the wrong AWS Region.

---

# 📊 EC2 Launch Summary

| Step | Purpose |
|------|---------|
| Name | Identify the instance |
| AMI | Operating System |
| Instance Type | CPU & Memory |
| Key Pair | Secure Login |
| Network | Connectivity |
| Security Group | Firewall |
| Storage | EBS Volume |
| Advanced Details | Automation & IAM |
| Launch | Create the EC2 Instance |

---

# 📝 Quick Revision

```
Launch Instance

↓

Name

↓

AMI

↓

Instance Type

↓

Key Pair

↓

Network

↓

Security Group

↓

Storage

↓

Launch

↓

Running
```

---

# 🎤 Interview Questions

### 1. What is the first step when launching an EC2 instance?

Open the EC2 Dashboard and click **Launch Instance**.

---

### 2. Why is an AMI required?

An AMI provides the operating system and software template for the EC2 instance.

---

### 3. Why do we need a Key Pair?

To securely connect to the EC2 instance using SSH (Linux) or to decrypt the Administrator password (Windows).

---

### 4. Which component acts as the firewall during launch?

The Security Group.

---

### 5. What happens after clicking **Launch Instance**?

AWS provisions the virtual machine, changes the instance state from **Pending** to **Running**, and makes it available for use.

---

# 🎯 Chapter Summary

Launching an EC2 instance is the process of creating a virtual server in AWS. During this process, you choose the operating system, hardware configuration, networking, security, and storage. Once launched, AWS provisions the instance and makes it available within minutes. Understanding each step in the launch wizard is essential for working with EC2 in both learning and production environments.
