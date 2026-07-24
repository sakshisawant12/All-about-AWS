# 📘 Chapter 10 - IAM Roles for EC2

> **"An IAM Role allows an EC2 instance to securely access AWS services without storing AWS Access Keys."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is an IAM Role?
- Why IAM Roles are needed
- IAM Role vs IAM User
- What is an Instance Profile?
- How IAM Roles work
- Temporary Security Credentials
- EC2 to S3 Access Example
- Best Practices
- Interview Questions

---

# 📖 What is an IAM Role?

An **IAM Role** is an AWS Identity that provides **temporary permissions** to AWS resources.

Unlike an IAM User, a Role has **no permanent username or password**.

Instead, AWS automatically provides temporary security credentials whenever the role is used.

---

# 💡 Simple Definition

Imagine you work in a company.

You don't carry every office key with you.

When you need access to a meeting room:

- You request access.
- Security verifies you.
- You receive temporary access.
- After the meeting, the access expires.

An IAM Role works in the same way.

---

# 🤔 Why Do We Need IAM Roles?

Suppose an EC2 instance needs to upload files to Amazon S3.

One option is:

```
Access Key

↓

Store Inside EC2

↓

Use AWS CLI
```

Problems:

- Access Keys can be stolen.
- Keys may accidentally be uploaded to GitHub.
- Keys must be rotated manually.

Instead, AWS recommends using an IAM Role.

---

# 🌍 Real-Life Example

Imagine a hotel.

Guests receive a temporary room key.

```
Guest

↓

Reception

↓

Temporary Key Card

↓

Room Access

↓

Key Expires
```

Guests never receive the hotel's master key.

Similarly, EC2 instances receive temporary credentials instead of permanent Access Keys.

---

# 🏗️ IAM Role Architecture

```
                EC2 Instance
                     │
                     ▼
              IAM Role Attached
                     │
                     ▼
          AWS Security Token Service (STS)
                     │
                     ▼
      Temporary Security Credentials
                     │
                     ▼
                Amazon S3
```

AWS automatically manages the credentials.

---

# 👤 IAM User vs IAM Role

| IAM User | IAM Role |
|-----------|----------|
| Permanent identity | Temporary identity |
| Username & Password | No username or password |
| Long-term Access Keys | Temporary credentials |
| Used by people | Used by AWS services, applications, and workloads |
| Credentials managed manually | Credentials managed automatically |

---

# 📦 What is an Instance Profile?

An **Instance Profile** is a container that allows an IAM Role to be attached to an EC2 instance.

Flow:

```
IAM Role

↓

Instance Profile

↓

EC2 Instance
```

When you attach an IAM Role in the AWS Console, AWS automatically creates and uses an Instance Profile behind the scenes.

---

# 🔄 How IAM Roles Work

```
Launch EC2

↓

Attach IAM Role

↓

EC2 Needs S3 Access

↓

AWS STS Generates

Temporary Credentials

↓

EC2 Accesses S3

↓

Credentials Rotate Automatically
```

The application never needs to store Access Keys.

---

# 🌍 Example - EC2 Uploads Files to S3

Without IAM Role:

```
EC2

↓

Access Key

↓

Amazon S3
```

Risk:

- Keys stored on the server
- Keys can be leaked

---

With IAM Role:

```
EC2

↓

IAM Role

↓

Temporary Credentials

↓

Amazon S3
```

Much more secure.

---

# 🔑 Temporary Security Credentials

IAM Roles use temporary credentials that include:

- Access Key ID
- Secret Access Key
- Session Token

These credentials:

- Expire automatically
- Rotate automatically
- Are managed by AWS

---

# 🚀 How to Attach an IAM Role

Steps:

```
AWS Console

↓

EC2

↓

Select Instance

↓

Actions

↓

Security

↓

Modify IAM Role

↓

Select Role

↓

Save
```

---

# 📌 Common Use Cases

IAM Roles are commonly used for:

- EC2 → Amazon S3
- EC2 → DynamoDB
- EC2 → Systems Manager
- Lambda → S3
- ECS Tasks
- EKS Pods
- Cross-account access

---

# 🌍 Real-World Example

A web application running on EC2 needs to store uploaded images in S3.

```
User Uploads Image

↓

EC2

↓

IAM Role

↓

Temporary Credentials

↓

Amazon S3
```

No Access Keys are stored on the server.

---

# ⭐ Advantages

- No hardcoded Access Keys
- Improved security
- Automatic credential rotation
- Easier permission management
- Follows AWS security best practices

---

# ⚠️ Common Mistakes

- Storing Access Keys inside EC2.
- Giving the role unnecessary permissions.
- Using one IAM Role for every application.
- Ignoring the principle of least privilege.

---

# 📊 IAM Role Summary

| Feature | Description |
|----------|-------------|
| Identity Type | Temporary |
| Credentials | Temporary |
| Password | No |
| Access Keys | No permanent keys |
| Credential Rotation | Automatic |
| Used By | AWS Services & Applications |

---

# 📝 Quick Revision

```
EC2

↓

IAM Role

↓

STS

↓

Temporary Credentials

↓

AWS Service

↓

Access Granted
```

---

# 🎤 Interview Questions

### 1. What is an IAM Role?

An IAM Role is an AWS identity that provides temporary permissions to AWS services and applications without requiring permanent credentials.

---

### 2. Why should EC2 use an IAM Role instead of Access Keys?

Because IAM Roles provide temporary, automatically rotated credentials, reducing the risk of credential exposure.

---

### 3. Which AWS service generates temporary credentials?

**AWS Security Token Service (STS).**

---

### 4. What is an Instance Profile?

An Instance Profile is a container that allows an IAM Role to be attached to an EC2 instance.

---

### 5. Can an IAM Role have permanent Access Keys?

No.

IAM Roles use temporary credentials only.

---

### 6. Which AWS service commonly uses IAM Roles to upload files securely?

Amazon S3.

---

# 🎯 Chapter Summary

IAM Roles provide a secure way for EC2 instances to access AWS services without storing permanent Access Keys. AWS automatically issues temporary credentials through AWS STS, making applications more secure and easier to manage. In production environments, IAM Roles are the recommended method for granting AWS permissions to EC2 instances.
