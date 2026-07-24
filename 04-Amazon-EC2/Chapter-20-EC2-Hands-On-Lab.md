# 📘 Chapter 20 - EC2 Hands-On Lab

> **"The best way to learn Amazon EC2 is by building a real server from scratch."**

---

# 🎯 Lab Objectives

In this lab, you will learn how to:

- Launch an EC2 instance
- Configure Security Groups
- Connect using SSH
- Install Apache Web Server
- Host a simple website
- Create and attach an EBS volume
- Mount the EBS volume
- Create an EBS Snapshot
- Associate an Elastic IP
- Attach an IAM Role
- Verify CloudWatch monitoring

---

# 🛠️ Lab Architecture

```
                     Internet
                         │
                         ▼
                   Elastic IP
                         │
                         ▼
                 Security Group
                         │
                         ▼
                  EC2 Instance
                         │
        ┌────────────────┴────────────────┐
        ▼                                 ▼
   Root EBS Volume                  Extra EBS Volume
        │                                 │
        ▼                                 ▼
   Amazon Linux                    Mounted at /data
                         │
                         ▼
                  Apache Web Server
                         │
                         ▼
                    Web Browser
```

---

# 📋 Prerequisites

Before starting, ensure you have:

- AWS Account
- IAM User (not Root User)
- EC2 Key Pair
- Basic knowledge of Linux commands
- AWS Region selected

---

# 🚀 Step 1 - Launch an EC2 Instance

Open:

```
AWS Console

↓

EC2

↓

Launch Instance
```

Configure:

| Setting | Value |
|----------|-------|
| Name | EC2-Lab |
| AMI | Amazon Linux 2023 |
| Instance Type | t2.micro (Free Tier) |
| Key Pair | Existing or New |
| Storage | 8 GB gp3 |
| Security Group | Create New |

---

# 🔒 Step 2 - Configure Security Group

Allow the following inbound rules:

| Type | Port | Source |
|------|------|--------|
| SSH | 22 | My IP |
| HTTP | 80 | 0.0.0.0/0 |

Launch the instance.

---

# 🌐 Step 3 - Connect Using SSH

```bash
chmod 400 my-key.pem
```

```bash
ssh -i my-key.pem ec2-user@<Public-IP>
```

Verify:

```bash
hostname
```

```bash
whoami
```

Expected output:

```
ec2-user
```

---

# 📦 Step 4 - Update the Server

```bash
sudo dnf update -y
```

---

# 🌍 Step 5 - Install Apache

```bash
sudo dnf install httpd -y
```

Start Apache:

```bash
sudo systemctl start httpd
```

Enable Apache at boot:

```bash
sudo systemctl enable httpd
```

Check status:

```bash
sudo systemctl status httpd
```

---

# 📝 Step 6 - Create a Web Page

```bash
echo "<h1>Welcome to AWS EC2</h1>" | sudo tee /var/www/html/index.html
```

Open your browser:

```
http://<Elastic-IP-or-Public-IP>
```

Expected page:

```
Welcome to AWS EC2
```

---

# 💾 Step 7 - Create an EBS Volume

Open:

```
EC2

↓

Volumes

↓

Create Volume
```

Configuration:

| Setting | Value |
|----------|-------|
| Size | 10 GB |
| Type | gp3 |
| Availability Zone | Same as EC2 |

Create the volume.

---

# 🔗 Step 8 - Attach the EBS Volume

```
Volumes

↓

Attach Volume

↓

Select EC2 Instance
```

Device Name:

```
/dev/xvdf
```

---

# 🔍 Step 9 - Verify the New Disk

```bash
lsblk
```

Example:

```
xvda

xvdf
```

---

# 📂 Step 10 - Format the Volume

```bash
sudo mkfs -t xfs /dev/xvdf
```

---

# 📁 Step 11 - Mount the Volume

Create a mount point:

```bash
sudo mkdir /data
```

Mount:

```bash
sudo mount /dev/xvdf /data
```

Verify:

```bash
df -h
```

---

# ✍️ Step 12 - Test the Volume

```bash
cd /data
```

```bash
sudo touch demo.txt
```

```bash
ls
```

Output:

```
demo.txt
```

---

# 📸 Step 13 - Create an EBS Snapshot

```
EC2

↓

Volumes

↓

Actions

↓

Create Snapshot
```

Add a name and description.

Click:

```
Create Snapshot
```

---

# 🌍 Step 14 - Allocate an Elastic IP

```
EC2

↓

Elastic IPs

↓

Allocate Elastic IP
```

---

# 🔗 Step 15 - Associate the Elastic IP

```
Elastic IP

↓

Associate

↓

Select EC2 Instance
```

Now your server has a permanent public IP address.

---

# 👤 Step 16 - Attach an IAM Role

Open:

```
EC2

↓

Actions

↓

Security

↓

Modify IAM Role
```

Attach a role with appropriate permissions (for example, read-only access to S3 for testing).

---

# 📊 Step 17 - Verify CloudWatch Metrics

Open:

```
CloudWatch

↓

Metrics

↓

EC2
```

Check metrics:

- CPU Utilization
- Network In
- Network Out
- Status Checks

---

# ✅ Lab Verification Checklist

| Task | Status |
|------|--------|
| EC2 Running | ✅ |
| SSH Working | ✅ |
| Apache Running | ✅ |
| Website Accessible | ✅ |
| EBS Mounted | ✅ |
| Snapshot Created | ✅ |
| Elastic IP Associated | ✅ |
| IAM Role Attached | ✅ |
| CloudWatch Metrics Visible | ✅ |

---

# 🌍 Final Architecture

```
                    Users
                       │
                       ▼
                  Elastic IP
                       │
                       ▼
               Security Group
                       │
                       ▼
                 EC2 Instance
                       │
        ┌──────────────┴──────────────┐
        ▼                             ▼
   Root EBS Volume              Extra EBS Volume
        │                             │
        └──────────────┬──────────────┘
                       ▼
                 Apache Web Server
                       │
                       ▼
                CloudWatch Metrics
```

---

# 🧹 Clean Up Resources

To avoid unnecessary AWS charges:

- Stop or terminate the EC2 instance.
- Delete the additional EBS volume if no longer needed.
- Delete the EBS Snapshot if it's not required.
- Release the Elastic IP if you won't use it again.

---

# ⭐ Best Practices

- Use IAM Roles instead of Access Keys.
- Restrict SSH access to your IP.
- Keep the operating system updated.
- Enable CloudWatch monitoring.
- Take EBS Snapshots before major changes.
- Use Elastic IP only when a static public IP is required.

---

# ❌ Common Mistakes

- Forgetting to open port 80 in the Security Group.
- Creating an EBS volume in a different Availability Zone.
- Forgetting to format the new EBS volume before mounting.
- Leaving unused Elastic IPs allocated.
- Not cleaning up resources after completing the lab.

---

# 🎤 Interview Questions

### 1. Why must an EBS volume be created in the same Availability Zone as the EC2 instance?

Because EBS volumes can only be attached to EC2 instances in the same Availability Zone.

---

### 2. Which command is used to verify attached disks?

```bash
lsblk
```

---

### 3. Which command mounts a new EBS volume?

```bash
sudo mount /dev/xvdf /data
```

---

### 4. Why do we associate an Elastic IP?

To provide a static public IPv4 address that doesn't change when the instance stops and starts.

---

### 5. Which AWS service monitors EC2 performance?

Amazon CloudWatch.

---

### 6. What should you do after finishing a lab to reduce costs?

Stop or terminate unused instances and delete unnecessary resources such as EBS volumes, snapshots, and Elastic IPs.

---

# 🎯 Chapter Summary

In this hands-on lab, you launched an EC2 instance, secured it with a Security Group, connected using SSH, installed Apache, hosted a website, attached and mounted an Amazon EBS volume, created a snapshot, associated an Elastic IP, attached an IAM Role, and monitored the instance using Amazon CloudWatch. This lab demonstrates a practical, production-style EC2 deployment and reinforces the key concepts covered throughout the EC2 module.
