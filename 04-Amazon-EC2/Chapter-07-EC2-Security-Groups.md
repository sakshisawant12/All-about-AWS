# 📘 Chapter 07 - EC2 Security Groups

> **"A Security Group is a virtual firewall that controls inbound and outbound traffic for an EC2 instance."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is a Security Group?
- Why Security Groups are required
- How Security Groups work
- Inbound Rules
- Outbound Rules
- Protocols
- Port Numbers
- CIDR Blocks
- Stateful Firewall
- Security Group Architecture
- Best Practices
- Interview Questions

---

# 📖 What is a Security Group?

A **Security Group (SG)** is a **virtual firewall** that protects an EC2 instance by controlling the network traffic that can enter or leave it.

Every EC2 instance must be associated with at least one Security Group.

A Security Group allows only the traffic that you explicitly permit. By default, all inbound traffic is denied.

---

# 💡 Simple Definition

Imagine your house.

- The walls protect your home.
- The main gate decides who can enter.
- The exit gate controls who can leave.

A Security Group works in the same way.

It acts as the security gate for your EC2 instance.

Only approved traffic is allowed.

---

# 🌍 Real-Life Example

Imagine a company office.

Visitors must pass through a security guard.

```
Visitor

↓

Security Guard

↓

Check Permission

↓

Allowed?

↓

Yes → Enter

No → Block
```

A Security Group works exactly like that security guard.

---

# 🏗️ Security Group Architecture

```
               Internet
                   │
                   ▼
        ┌─────────────────────┐
        │   Security Group    │
        │                     │
        │ Allow Port 22       │
        │ Allow Port 80       │
        │ Allow Port 443      │
        └─────────────────────┘
                   │
                   ▼
             EC2 Instance
```

---

# 🔄 How Security Groups Work

```
Internet

↓

Request Arrives

↓

Security Group Checks Rules

↓

Allowed?

↓

YES

↓

EC2 Instance

OR

NO

↓

Traffic Blocked
```

Every incoming request is checked before it reaches the EC2 instance.

---

# 📥 Inbound Rules

Inbound Rules control **incoming traffic**.

Example:

```
Laptop

↓

SSH

↓

EC2
```

Examples:

| Port | Service | Purpose |
|------|----------|----------|
| 22 | SSH | Linux Login |
| 80 | HTTP | Website |
| 443 | HTTPS | Secure Website |
| 3389 | RDP | Windows Login |

---

# 📤 Outbound Rules

Outbound Rules control **outgoing traffic**.

Example:

```
EC2

↓

Internet

↓

Download Updates
```

By default, AWS allows all outbound traffic.

---

# 🌐 Protocols

Security Groups support different network protocols.

| Protocol | Purpose |
|----------|----------|
| TCP | Web, SSH, Database |
| UDP | DNS, Video Streaming |
| ICMP | Ping |
| All Traffic | Allows all protocols |

---

# 🔢 Common Port Numbers

| Port | Service |
|------|----------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP |
| 21 | FTP |
| 25 | SMTP |
| 53 | DNS |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 1433 | Microsoft SQL Server |

---

# 🌍 What is CIDR?

CIDR (Classless Inter-Domain Routing) defines which IP addresses are allowed.

Example:

```
0.0.0.0/0
```

Means:

```
Everyone on the Internet
```

Example:

```
192.168.1.0/24
```

Means:

Only devices in that network.

---

# ⚠️ Important CIDR Examples

| CIDR | Meaning |
|------|----------|
| 0.0.0.0/0 | Entire Internet |
| 10.0.0.0/16 | Private VPC Network |
| 192.168.1.0/24 | Private Local Network |
| Your Public IP/32 | Only one specific IP address |

For better security, allow SSH only from **your public IP (/32)** instead of `0.0.0.0/0`.

---

# 🛡️ Stateful Firewall

Security Groups are **Stateful**.

This means:

If inbound traffic is allowed, the return traffic is automatically allowed.

Example:

```
Laptop

↓

SSH Request

↓

EC2

↓

SSH Response

↓

Automatically Allowed
```

You do **not** need to create a separate outbound rule for the response.

---

# 🔄 Example

Security Group Rule:

```
SSH

Port 22

Source

203.0.113.10/32
```

Result:

Only that IP address can connect using SSH.

All other IP addresses are blocked.

---

# 🌍 Real-World Web Server Example

Suppose you host a website.

Required rules:

| Port | Purpose |
|------|----------|
| 22 | SSH Administration |
| 80 | HTTP Website |
| 443 | HTTPS Website |

Diagram:

```
Internet

      │

      ▼

Security Group

│

├── Allow SSH (22)

├── Allow HTTP (80)

└── Allow HTTPS (443)

      │

      ▼

EC2 Web Server
```

---

# ⭐ Best Practices

- Allow only required ports.
- Restrict SSH access to your IP address.
- Never open unnecessary ports.
- Use different Security Groups for different applications.
- Review Security Group rules regularly.
- Follow the principle of least privilege.

---

# ❌ Common Mistakes

- Allowing SSH from `0.0.0.0/0` in production.
- Opening every port.
- Forgetting to remove unused rules.
- Using one Security Group for all applications.
- Exposing database ports directly to the Internet.

---

# 📊 Security Group Summary

| Feature | Description |
|----------|-------------|
| Type | Virtual Firewall |
| Scope | Instance Level |
| Inbound Rules | Incoming Traffic |
| Outbound Rules | Outgoing Traffic |
| Stateful | Yes |
| Default Inbound | Deny All |
| Default Outbound | Allow All |

---

# 🔍 Security Group vs NACL

| Security Group | Network ACL |
|----------------|-------------|
| Instance Level | Subnet Level |
| Stateful | Stateless |
| Allows Rules Only | Allows and Denies Rules |
| Evaluated on Instance | Evaluated on Subnet |

> **Note:** We'll study Network ACLs in detail in the VPC section.

---

# 📝 Quick Revision

```
Internet

↓

Security Group

↓

Checks Rules

↓

Allowed?

↓

Yes

↓

EC2

↓

No

↓

Blocked
```

---

# 🎤 Interview Questions

### 1. What is a Security Group?

A Security Group is a virtual firewall that controls inbound and outbound traffic for an EC2 instance.

---

### 2. Is a Security Group Stateful or Stateless?

Stateful.

---

### 3. What is the default inbound rule?

All inbound traffic is denied unless explicitly allowed.

---

### 4. What is the default outbound rule?

All outbound traffic is allowed.

---

### 5. Which port is used for SSH?

Port **22**.

---

### 6. Which port is used for HTTP?

Port **80**.

---

### 7. Which port is used for HTTPS?

Port **443**.

---

### 8. What does `0.0.0.0/0` mean?

It allows traffic from anywhere on the Internet.

---

### 9. Can one Security Group be attached to multiple EC2 instances?

Yes.

---

### 10. Can one EC2 instance have multiple Security Groups?

Yes.

---

# 🎯 Chapter Summary

A Security Group is an instance-level, stateful virtual firewall that protects EC2 instances by controlling inbound and outbound network traffic. It allows only explicitly permitted traffic, making it a fundamental security feature in AWS. Configuring Security Groups correctly is essential for building secure and reliable cloud infrastructure.
