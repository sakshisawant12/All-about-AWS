# 📘 Chapter 21 - Secure AWS Web Server Project

> **"A real Cloud Engineer learns by building. This project demonstrates how to securely deploy and manage a web server on AWS using Amazon EC2, IAM, EBS, and Apache HTTP Server."**

---

# 🎯 Project Objectives

This project demonstrates how to:

- Launch an Amazon EC2 instance
- Secure access using IAM and Security Groups
- Connect to the server using SSH
- Install and configure Apache HTTP Server
- Host a static website
- Attach and mount an Amazon EBS volume
- Create an EBS Snapshot
- Understand secure cloud deployment practices

---

# 🛠️ Technologies Used

| Service | Purpose |
|----------|---------|
| Amazon EC2 | Virtual Server |
| Amazon EBS | Persistent Block Storage |
| IAM | Secure AWS Access |
| Security Groups | Firewall |
| SSH | Remote Server Access |
| Apache HTTP Server | Web Server |
| Amazon Linux | Operating System |

---

# 📖 Project Overview

A secure Linux web server was deployed on AWS using Amazon EC2.

The server was configured to:

- Allow secure SSH access
- Host a web application using Apache
- Store persistent data using Amazon EBS
- Protect access with Security Groups
- Create storage backups using EBS Snapshots

This project demonstrates the basic infrastructure setup commonly used in AWS environments.

---

# 🏗️ Project Architecture

```
                      Internet
                          │
                          ▼
                  Security Group
                 (SSH 22 / HTTP 80)
                          │
                          ▼
                  Amazon EC2 Instance
                   (Amazon Linux)
                          │
         ┌────────────────┴────────────────┐
         ▼                                 ▼
   Root EBS Volume                 Additional EBS Volume
         │                                 │
         ▼                                 ▼
  Operating System                  Mounted at /data
                          │
                          ▼
                 Apache HTTP Server
                          │
                          ▼
                     Static Website
```

---

# 🚀 Project Implementation

## Step 1 - Launch EC2 Instance

Configuration:

| Setting | Value |
|----------|-------|
| AMI | Amazon Linux 2023 |
| Instance Type | t2.micro |
| Storage | 8 GB gp3 |
| Key Pair | Existing Key Pair |
| Authentication | SSH |

---

## Step 2 - Configure Security Group

Inbound Rules

| Port | Service | Source |
|------|----------|--------|
| 22 | SSH | My IP |
| 80 | HTTP | 0.0.0.0/0 |

This allows:

- Secure SSH login
- Public website access

---

## Step 3 - Connect Using SSH

```bash
chmod 400 my-key.pem

ssh -i my-key.pem ec2-user@<Public-IP>
```

---

## Step 4 - Update Packages

```bash
sudo dnf update -y
```

---

## Step 5 - Install Apache

```bash
sudo dnf install httpd -y
```

Start Apache

```bash
sudo systemctl start httpd
```

Enable Apache

```bash
sudo systemctl enable httpd
```

Verify

```bash
sudo systemctl status httpd
```

---

## Step 6 - Deploy Website

Create a simple web page

```bash
echo "<h1>Welcome to AWS EC2</h1>" | sudo tee /var/www/html/index.html
```

Verify in browser

```
http://<Public-IP>
```

---

## Step 7 - Create an Additional EBS Volume

Configuration

| Setting | Value |
|----------|-------|
| Size | 10 GB |
| Type | gp3 |
| AZ | Same as EC2 |

Attach the volume to the instance.

---

## Step 8 - Verify the Disk

```bash
lsblk
```

Example Output

```
xvda

xvdf
```

---

## Step 9 - Format the Volume

```bash
sudo mkfs -t xfs /dev/xvdf
```

---

## Step 10 - Mount the Volume

```bash
sudo mkdir /data

sudo mount /dev/xvdf /data
```

Verify

```bash
df -h
```

---

## Step 11 - Test Storage

```bash
cd /data

sudo touch project.txt

ls
```

Output

```
project.txt
```

---

## Step 12 - Create an EBS Snapshot

```
EC2

↓

Volumes

↓

Create Snapshot
```

Snapshot successfully created.

---

# 🔐 Security Features

- IAM User used instead of Root User
- SSH restricted to a trusted IP address
- Security Group configured with minimum required ports
- Persistent storage using Amazon EBS
- Backup created using EBS Snapshot

---

# 💡 Linux Commands Used

```bash
ssh
chmod
dnf update
dnf install
systemctl
lsblk
mkfs
mkdir
mount
df
touch
```

---

# 📊 Project Workflow

```
Launch EC2
      │
      ▼
Configure Security Group
      │
      ▼
Connect via SSH
      │
      ▼
Install Apache
      │
      ▼
Deploy Website
      │
      ▼
Create EBS Volume
      │
      ▼
Attach Volume
      │
      ▼
Format Volume
      │
      ▼
Mount Volume
      │
      ▼
Create Snapshot
```

---

# 📸 Screenshots to Include

- EC2 Dashboard
- Running EC2 Instance
- Security Group Rules
- SSH Terminal
- Apache Running
- Website in Browser
- EBS Volume
- Mounted Disk (`lsblk`)
- Snapshot Created

---

# 🌍 Real-World Use Case

A startup wants to deploy a simple company website on AWS.

Using this architecture they can:

- Host the website on Amazon EC2
- Store application data on Amazon EBS
- Protect the instance with Security Groups
- Connect securely using SSH
- Back up storage with EBS Snapshots

This forms the foundation of many production web server deployments.

---

# ⭐ Skills Demonstrated

- Amazon EC2
- Amazon EBS
- IAM
- Linux Administration
- SSH
- Apache HTTP Server
- Security Groups
- AWS Storage
- Cloud Infrastructure Deployment

---

# 🎤 Interview Questions

### 1. Why was Amazon EBS used?

To provide persistent storage that remains available even if the EC2 instance is stopped.

---

### 2. Why restrict SSH to your own IP?

To reduce the attack surface and prevent unauthorized access.

---

### 3. Which Linux command verifies attached disks?

```bash
lsblk
```

---

### 4. Why create an EBS Snapshot?

To back up the EBS volume so it can be restored if data is lost or corrupted.

---

### 5. Which AWS service acts as a virtual firewall?

Security Groups.

---

### 6. Which AWS service provides the virtual machine?

Amazon EC2.

---

# 🎯 Project Summary

This project demonstrates how to deploy and secure a web server on AWS using Amazon EC2. It covers launching an instance, configuring secure access, installing Apache, hosting a website, attaching and mounting an Amazon EBS volume, and creating snapshots for backup. The project showcases essential AWS Cloud Engineer skills and serves as a practical foundation for building more advanced cloud architectures.
