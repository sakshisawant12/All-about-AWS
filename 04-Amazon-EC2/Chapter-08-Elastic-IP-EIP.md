# 📘 Chapter 08 - Elastic IP (EIP)

> **"An Elastic IP (EIP) is a static public IPv4 address provided by AWS that you can associate with an EC2 instance."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is an Elastic IP?
- Why Elastic IP is required
- Public IP vs Private IP vs Elastic IP
- How Elastic IP works
- Allocate and Associate an Elastic IP
- Elastic IP Architecture
- Advantages
- Limitations
- Best Practices
- Interview Questions

---

# 📖 What is an Elastic IP?

An **Elastic IP (EIP)** is a **static public IPv4 address** provided by AWS.

Unlike a normal Public IP, an Elastic IP remains the same even if you stop and start your EC2 instance.

You can also move an Elastic IP from one EC2 instance to another within the same AWS Region.

---

# 💡 Simple Definition

Think of your home address.

If you move to another house, your address changes.

But imagine owning a permanent address that you can transfer to any house you live in.

An Elastic IP works in the same way.

It is a permanent public IP address that you can attach to different EC2 instances.

---

# 🌍 Real-Life Example

Imagine your company has a public office phone number.

Employees may change offices, but customers always call the same number.

```
Customer

↓

Company Phone Number

↓

Current Office
```

Similarly, users always connect using the same Elastic IP, even if the EC2 instance changes.

---

# 🌐 Types of IP Addresses in EC2

Every EC2 instance can have different types of IP addresses.

### 1️⃣ Private IP Address

- Used for communication inside the VPC.
- Assigned automatically.
- Cannot be accessed directly from the Internet.
- Remains the same throughout the life of the network interface.

Example:

```
10.0.1.15
```

---

### 2️⃣ Public IP Address

- Used for Internet access.
- Assigned automatically (if enabled).
- Changes when the instance is stopped and started.

Example:

```
54.123.45.210
```

---

### 3️⃣ Elastic IP Address

- Static Public IPv4 address.
- Does not change when the instance stops or starts.
- Can be reassigned to another EC2 instance in the same Region.

Example:

```
18.204.120.15
```

---

# 📊 Private IP vs Public IP vs Elastic IP

| Feature | Private IP | Public IP | Elastic IP |
|----------|------------|-----------|------------|
| Internet Accessible | ❌ No | ✅ Yes | ✅ Yes |
| Changes After Stop/Start | ❌ No | ✅ Yes | ❌ No |
| Static | ✅ Yes | ❌ No | ✅ Yes |
| Can Move to Another Instance | ❌ No | ❌ No | ✅ Yes |

---

# 🤔 Why Do We Need an Elastic IP?

Suppose you host a website.

```
www.example.com

↓

Public IP

↓

EC2 Instance
```

If the EC2 instance is stopped and started:

- The normal Public IP changes.
- Users cannot reach the website until DNS is updated.

With an Elastic IP:

- The IP address remains the same.
- No DNS changes are required.

---

# 🏗️ Elastic IP Architecture

```
                 Internet
                      │
                      ▼
               Elastic IP (EIP)
                      │
                      ▼
               EC2 Instance
                      │
                      ▼
                Private IP
                      │
                      ▼
                     VPC
```

---

# 🔄 How Elastic IP Works

```
Allocate Elastic IP

        │
        ▼

Associate with EC2

        │
        ▼

Internet Traffic

        │
        ▼

Elastic IP

        │
        ▼

EC2 Instance
```

If the instance fails:

```
Disassociate EIP

↓

Associate EIP

↓

New EC2 Instance
```

The public IP remains the same.

---

# 🚀 How to Allocate an Elastic IP

Steps:

```
AWS Console

↓

EC2 Dashboard

↓

Elastic IPs

↓

Allocate Elastic IP Address

↓

Allocate
```

AWS reserves a static public IPv4 address for your account.

---

# 🔗 How to Associate an Elastic IP

After allocation:

```
Elastic IP

↓

Actions

↓

Associate Elastic IP

↓

Select EC2 Instance

↓

Associate
```

The Elastic IP is now linked to your EC2 instance.

---

# ❌ How to Release an Elastic IP

If you no longer need it:

```
Elastic IP

↓

Disassociate

↓

Release Elastic IP
```

Releasing the Elastic IP returns it to AWS.

---

# ⭐ Advantages of Elastic IP

- Static public IP address
- Easy disaster recovery
- No DNS updates after instance replacement
- Can move between EC2 instances
- Ideal for production applications

---

# ⚠️ Limitations

- Only IPv4 addresses are supported.
- Limited number of Elastic IPs per Region (quota applies).
- AWS charges for unused Elastic IPs.
- An Elastic IP can be associated with only one resource at a time.

---

# 💰 Pricing Note

AWS encourages efficient use of Elastic IP addresses.

You may incur charges for:

- Unassociated Elastic IPs
- Excessive Elastic IP usage beyond service quotas

Always check the latest AWS pricing before using Elastic IPs in production.

---

# 🌍 Real-World Example

A company hosts its website on EC2.

Without Elastic IP:

```
Website

↓

Public IP Changes

↓

Users Cannot Connect
```

With Elastic IP:

```
Website

↓

Elastic IP

↓

EC2 Instance

↓

Same Public IP Always
```

---

# ⭐ Best Practices

- Use Elastic IPs only when a static public IP is required.
- Release unused Elastic IPs.
- Use Route 53 for DNS management.
- Use Load Balancers instead of assigning Elastic IPs to multiple application servers.
- Monitor Elastic IP usage to avoid unnecessary costs.

---

# ❌ Common Mistakes

- Confusing Public IP with Elastic IP.
- Forgetting to release unused Elastic IPs.
- Assuming Elastic IPs are unlimited.
- Using Elastic IPs for every EC2 instance unnecessarily.

---

# 📊 Elastic IP Summary

| Feature | Description |
|----------|-------------|
| Type | Static Public IPv4 Address |
| Scope | AWS Region |
| Internet Access | Yes |
| Changes After Stop/Start | No |
| Can Move Between Instances | Yes |
| Charged When Unused | Yes |

---

# 📝 Quick Revision

```
Allocate Elastic IP

↓

Associate

↓

EC2 Instance

↓

Static Public IP

↓

Internet Access
```

---

# 🎤 Interview Questions

### 1. What is an Elastic IP?

An Elastic IP is a static public IPv4 address that can be associated with an EC2 instance.

---

### 2. Why do we use an Elastic IP?

To provide a permanent public IP address that does not change when the EC2 instance is stopped, started, or replaced.

---

### 3. Does a normal Public IP change?

Yes.

A normal Public IP may change after the instance is stopped and started.

---

### 4. Can one Elastic IP be associated with multiple EC2 instances at the same time?

No.

An Elastic IP can be associated with only one resource at a time.

---

### 5. Can an Elastic IP be moved to another EC2 instance?

Yes.

It can be disassociated from one instance and associated with another within the same AWS Region.

---

### 6. Is an Elastic IP free?

Not always.

AWS may charge for unused Elastic IPs and for usage beyond applicable quotas.

---

# 🎯 Chapter Summary

An Elastic IP is a static public IPv4 address that provides consistent Internet connectivity for AWS resources. Unlike a regular Public IP, it remains the same across stop/start operations and can be reassigned to another EC2 instance. Elastic IPs are useful for production workloads, disaster recovery, and services that require a fixed public IP address.
