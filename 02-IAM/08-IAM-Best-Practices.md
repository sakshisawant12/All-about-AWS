# 🛡️ IAM Best Practices

> **Chapter 8** | AWS Identity and Access Management (IAM)

---

## 📖 Table of Contents

- Why IAM Best Practices Matter
- Follow the Principle of Least Privilege
- Never Use the Root User for Daily Tasks
- Enable Multi-Factor Authentication (MFA)
- Use IAM Roles Instead of Access Keys
- Rotate Credentials Regularly
- Use Groups to Manage Permissions
- Monitor IAM Activity
- Remove Unused Users and Credentials
- Review Permissions Regularly
- Key Takeaways
- Interview Questions

---

# Why IAM Best Practices Matter?

AWS provides powerful security features, but they are only effective when used correctly.

Following IAM best practices helps you:

- Protect AWS resources
- Prevent unauthorized access
- Reduce security risks
- Meet compliance requirements

---

# 1. Follow the Principle of Least Privilege

Grant users **only the permissions they need** to perform their job.

❌ Bad Example

```
Developer
│
└── AdministratorAccess
```

✅ Good Example

```
Developer
│
├── EC2 Read/Write
├── S3 ReadOnly
└── CloudWatch ReadOnly
```

---

# 2. Never Use the Root User for Daily Tasks

The AWS Root User has unrestricted access to every AWS service.

Use the Root User only for tasks such as:

- Creating the AWS account
- Managing account settings
- Closing the AWS account
- Changing payment methods

Create IAM Users for daily administration.

> [!WARNING]
> Never share Root User credentials.

---

# 3. Enable Multi-Factor Authentication (MFA)

Always enable MFA for:

- AWS Root User
- IAM Users

Benefits:

- Prevents unauthorized access
- Protects against stolen passwords
- Adds an extra layer of security

---

# 4. Use IAM Roles Instead of Access Keys

Avoid storing long-term AWS Access Keys in:

- Applications
- EC2 Instances
- Lambda Functions
- Source code

Instead, assign an IAM Role.

```
EC2
 │
 ▼
IAM Role
 │
 ▼
Temporary Credentials
```

---

# 5. Rotate Credentials Regularly

Regularly rotate:

- Passwords
- Access Keys
- Secrets

Old credentials increase security risks.

---

# 6. Use IAM Groups

Instead of assigning permissions individually:

```
Developers Group
│
├── John
├── Alice
├── David
└── Priya
```

Assign one policy to the group.

Benefits:

- Easier management
- Consistent permissions
- Reduced errors

---

# 7. Monitor IAM Activity

Use AWS services such as:

- AWS CloudTrail
- Amazon CloudWatch

These services help:

- Track login activity
- Detect suspicious actions
- Audit user activity

---

# 8. Remove Unused Users and Credentials

Regularly review your AWS account and remove:

- Unused IAM Users
- Inactive Roles
- Old Access Keys
- Unused Policies

Unused identities increase security risks.

---

# 9. Review Permissions Regularly

Perform regular permission audits.

Ask questions such as:

- Does the user still need this permission?
- Can permissions be reduced?
- Are there unused policies?

Regular reviews improve security.

---

# IAM Security Checklist

| Best Practice | Recommended |
|---------------|-------------|
| Use IAM Users | ✅ |
| Enable MFA | ✅ |
| Use IAM Roles | ✅ |
| Apply Least Privilege | ✅ |
| Rotate Credentials | ✅ |
| Monitor Activity | ✅ |
| Remove Unused Users | ✅ |
| Review Permissions | ✅ |
| Use Root User Daily | ❌ |

---

# Real-World Example

A company hires a new developer.

Instead of giving AdministratorAccess:

- Create an IAM User.
- Add the user to the Developers Group.
- Enable MFA.
- Attach only the required policies.
- Assign IAM Roles when AWS services need access.

This approach keeps the AWS environment secure and manageable.

---

# Key Takeaways

- Protect the Root User.
- Enable MFA for all users.
- Follow the Principle of Least Privilege.
- Prefer IAM Roles over Access Keys.
- Review permissions and remove unused identities regularly.
- Monitor account activity with CloudTrail and CloudWatch.

---

# Quick Summary

| Security Practice | Purpose |
|-------------------|---------|
| Least Privilege | Reduce unnecessary access |
| MFA | Add an extra security layer |
| IAM Roles | Temporary credentials |
| IAM Groups | Simplify permission management |
| CloudTrail | Audit and monitor activity |
| CloudWatch | Monitor events and metrics |

---

# Interview Questions

### 1. What is the Principle of Least Privilege?

### 2. Why shouldn't the Root User be used for daily tasks?

### 3. Why is MFA important?

### 4. Why are IAM Roles preferred over Access Keys?

### 5. How often should permissions be reviewed?

### 6. Which AWS services help monitor IAM activity?

### 7. Why should Access Keys be rotated?

### 8. What are the benefits of IAM Groups?

### 9. Why should unused IAM Users be removed?

### 10. Name five AWS IAM best practices.

---
