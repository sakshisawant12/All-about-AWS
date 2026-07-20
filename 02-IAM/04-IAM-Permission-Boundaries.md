# 🚧 IAM Permission Boundaries

> **Chapter 4** | AWS Identity and Access Management (IAM)

---

## 📖 Table of Contents

- What are Permission Boundaries?
- Why Do We Need Permission Boundaries?
- How Permission Boundaries Work
- Permission Boundary vs IAM Policy
- Real-World Example
- Advantages
- Limitations
- Best Practices
- Key Takeaways
- Interview Questions

---

# What are Permission Boundaries?

An **IAM Permission Boundary** is an advanced IAM feature that defines the **maximum permissions** an IAM User or IAM Role can have.

Think of it as a **permission limit**.

Even if an IAM Policy grants permissions, the user **cannot exceed** the permissions defined in the boundary.

---

# Why Do We Need Permission Boundaries?

In large organizations:

- Many administrators create IAM Users and Roles.
- Developers may accidentally assign excessive permissions.
- Organizations need a way to limit the maximum permissions that can be granted.

Permission Boundaries solve this problem by setting an upper limit.

---

# How Permission Boundaries Work

A user receives permissions only when **both** of these allow the action:

- IAM Policy
- Permission Boundary

If either one denies the action, access is denied.

```
          IAM Policy
              │
              ▼
      Permission Boundary
              │
              ▼
      Final Permissions
```

---

# Example

Suppose an IAM User has this policy:

```
Allow:
- EC2 Full Access
- S3 Full Access
```

But the Permission Boundary allows only:

```
- EC2 Full Access
```

### Result

| Service | Access |
|----------|--------|
| EC2 | ✅ Allowed |
| S3 | ❌ Denied |

Even though the IAM Policy allows S3, the Permission Boundary blocks it.

---

# Permission Boundary vs IAM Policy

| Feature | IAM Policy | Permission Boundary |
|----------|------------|---------------------|
| Grants Permissions | ✅ Yes | ❌ No |
| Sets Maximum Limit | ❌ No | ✅ Yes |
| Used For | Access Control | Permission Control |
| Can Increase Permissions | ✅ Yes | ❌ No |

---

# Real-World Example

Imagine a company hires interns.

The IT team wants interns to create EC2 instances for testing.

However, interns should never be able to:

- Delete IAM Users
- Modify Billing
- Delete Production Resources

The company creates:

### IAM Policy

```
Allow EC2
Allow S3
```

### Permission Boundary

```
Allow EC2 Only
```

Result:

- EC2 ✅
- S3 ❌
- IAM ❌

This ensures interns never receive excessive permissions.

---

# Advantages

- Prevents privilege escalation
- Improves security
- Simplifies permission management
- Ideal for large organizations
- Helps enforce company security policies

---

# Limitations

- Does not grant permissions by itself
- Requires IAM Policies to work
- Can make permission troubleshooting more complex

---

# Best Practices

- Use Permission Boundaries for developers and interns.
- Combine with the Principle of Least Privilege.
- Review boundaries regularly.
- Use descriptive names for boundary policies.
- Test permissions before deployment.

---

# Key Takeaways

- Permission Boundaries define the maximum permissions.
- They do not grant permissions.
- Final permissions are the intersection of the IAM Policy and the Permission Boundary.
- Commonly used in enterprise AWS environments.

---

# Quick Summary

| Term | Meaning |
|------|---------|
| IAM Policy | Grants permissions |
| Permission Boundary | Limits maximum permissions |
| Final Access | Intersection of both |

---

# Interview Questions

### 1. What is an IAM Permission Boundary?

### 2. Does a Permission Boundary grant permissions?

### 3. Why are Permission Boundaries used?

### 4. What happens if the IAM Policy allows an action but the Permission Boundary does not?

### 5. What is the difference between an IAM Policy and a Permission Boundary?

### 6. Where are Permission Boundaries commonly used?

### 7. Can a Permission Boundary increase permissions?

### 8. Explain Permission Boundaries with a real-world example.

---
