# 📘 Chapter 09 - EC2 User Data

> **"User Data is a script that automatically runs when an EC2 instance launches for the first time, allowing you to configure the server without manual intervention."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is EC2 User Data?
- Why User Data is required
- How User Data works
- Bootstrapping
- Cloud-init
- Writing User Data scripts
- Real-world examples
- Best Practices
- Interview Questions

---

# 📖 What is EC2 User Data?

**EC2 User Data** is a script or set of commands that AWS executes automatically when an EC2 instance boots for the first time.

It is mainly used to automate server configuration.

Instead of logging into the server manually and installing software, User Data performs those tasks automatically.

---

# 💡 Simple Definition

Imagine buying a new laptop.

Normally you would:

- Install Chrome
- Install VS Code
- Install Git
- Configure settings

This takes time.

Instead, imagine pressing one button and the laptop automatically installs everything.

That is exactly what User Data does for an EC2 instance.

---

# 🌍 Real-Life Example

Suppose a company launches 100 web servers.

Without User Data:

```
Launch Server

↓

Login

↓

Install Apache

↓

Start Apache

↓

Repeat 100 Times
```

With User Data:

```
Launch 100 Servers

↓

User Data Executes

↓

Apache Installed Automatically

↓

Website Ready
```

This saves time and ensures consistency.

---

# 🤔 Why Do We Need User Data?

Without User Data:

- Manual configuration
- Time-consuming
- Human errors
- Different server configurations

With User Data:

- Automatic installation
- Faster deployment
- Consistent configuration
- Less manual work

---

# 🏗️ User Data Architecture

```
                Launch EC2
                     │
                     ▼
            User Data Script
                     │
                     ▼
              EC2 First Boot
                     │
                     ▼
          cloud-init Executes Script
                     │
                     ▼
          Software Installed
                     │
                     ▼
            Server Ready to Use
```

---

# 🌩️ What is Bootstrapping?

**Bootstrapping** is the process of automatically configuring a server when it starts for the first time.

User Data is commonly used for bootstrapping EC2 instances.

Example tasks include:

- Updating packages
- Installing software
- Creating users
- Downloading application files
- Starting services

---

# ⚙️ What is cloud-init?

Most Linux AMIs use **cloud-init**, a package that reads and executes User Data during the first boot.

Flow:

```
EC2 Starts

↓

cloud-init Runs

↓

Reads User Data

↓

Executes Commands

↓

Configuration Complete
```

---

# ✍️ Basic User Data Script

Example:

```bash
#!/bin/bash

yum update -y
yum install httpd -y

systemctl start httpd
systemctl enable httpd
```

This script:

- Updates packages
- Installs Apache
- Starts the Apache service
- Enables Apache to start automatically after reboot

---

# 🐧 Ubuntu Example

```bash
#!/bin/bash

apt update -y
apt install apache2 -y

systemctl start apache2
systemctl enable apache2
```

---

# 🌍 Deploying a Simple Web Page

```bash
#!/bin/bash

yum update -y
yum install httpd -y

echo "<h1>Welcome to AWS EC2</h1>" > /var/www/html/index.html

systemctl start httpd
systemctl enable httpd
```

After the instance launches, visiting its Public IP in a browser displays:

```
Welcome to AWS EC2
```

---

# 🚀 How to Add User Data

Steps:

```
AWS Console

↓

EC2

↓

Launch Instance

↓

Advanced Details

↓

User Data

↓

Paste Script

↓

Launch
```

AWS automatically executes the script during the first boot.

---

# 🔄 User Data Execution Flow

```
Launch EC2

↓

Instance Boots

↓

cloud-init Starts

↓

Reads User Data

↓

Executes Commands

↓

Application Installed

↓

Instance Ready
```

---

# 📌 Common Use Cases

User Data is commonly used to:

- Install Apache
- Install Nginx
- Install Docker
- Install Git
- Install Java
- Configure users
- Clone Git repositories
- Deploy web applications
- Run startup scripts

---

# 🌍 Real-World Example

A DevOps team needs 50 web servers.

Without User Data:

```
50 Servers

↓

Manual Login

↓

Install Software

↓

Configure

↓

Start Services
```

With User Data:

```
50 Servers

↓

Launch

↓

User Data

↓

Servers Automatically Configured
```

Every server is configured in the same way.

---

# ⭐ Advantages

- Fully automated server setup
- Faster deployments
- Consistent configurations
- Reduced manual effort
- Fewer human errors
- Ideal for Auto Scaling environments

---

# ⚠️ Limitations

- By default, User Data runs only on the first boot.
- Debugging failed scripts can be difficult.
- Large scripts are harder to maintain.
- Errors in the script can leave the server partially configured.

---

# 📂 Where Are User Data Logs Stored?

If your User Data script doesn't work, check these logs:

```text
/var/log/cloud-init.log

/var/log/cloud-init-output.log
```

These logs help identify execution errors.

---

# ⭐ Best Practices

- Keep scripts simple.
- Test scripts before production use.
- Store large scripts in version control (e.g., Git).
- Use IAM Roles instead of embedding AWS credentials.
- Check cloud-init logs when troubleshooting.

---

# ❌ Common Mistakes

- Forgetting the `#!/bin/bash` line.
- Using Ubuntu commands on Amazon Linux (or vice versa).
- Assuming User Data runs on every reboot by default.
- Not checking cloud-init logs when troubleshooting.

---

# 📊 User Data Summary

| Feature | Description |
|----------|-------------|
| Purpose | Automate EC2 configuration |
| Execution | First boot (by default) |
| Linux Service | cloud-init |
| Supports | Bash, cloud-init directives, MIME multi-part |
| Common Uses | Install software, configure services, deploy apps |

---

# 📝 Quick Revision

```
Launch EC2

↓

User Data

↓

cloud-init

↓

Execute Script

↓

Install Software

↓

Ready
```

---

# 🎤 Interview Questions

### 1. What is EC2 User Data?

User Data is a script that AWS automatically executes when an EC2 instance launches for the first time.

---

### 2. What is bootstrapping?

Bootstrapping is the automatic configuration of a server during its initial startup.

---

### 3. Which service executes User Data on most Linux AMIs?

**cloud-init**

---

### 4. Does User Data run every time the instance restarts?

No.

By default, it runs only during the first boot.

---

### 5. Where can you view User Data execution logs?

- `/var/log/cloud-init.log`
- `/var/log/cloud-init-output.log`

---

### 6. What are common uses of User Data?

- Install software
- Configure services
- Deploy applications
- Create users
- Automate server setup

---

# 🎯 Chapter Summary

EC2 User Data is a powerful automation feature that allows servers to configure themselves during the first boot. It is commonly used to install software, deploy applications, configure services, and prepare instances without manual login. By combining User Data with cloud-init, AWS enables consistent, repeatable, and scalable server deployments.
