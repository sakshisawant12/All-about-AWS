# 📘 Chapter 06 - EC2 Key Pairs

> **"A Key Pair is a secure authentication mechanism that allows you to connect to an EC2 instance without using a traditional password."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is a Key Pair?
- Why Key Pairs are required
- Public Key vs Private Key
- How SSH authentication works
- .pem vs .ppk
- Creating a Key Pair
- Connecting to Linux & Windows instances
- Best Practices
- Interview Questions

---

# 📖 What is a Key Pair?

A **Key Pair** is a combination of two cryptographic keys used for secure authentication.

It consists of:

- Public Key
- Private Key

AWS uses these keys to verify your identity when connecting to an EC2 instance.

Unlike traditional username-password authentication, EC2 uses **public key cryptography**, which is much more secure.

---

# 💡 Simple Definition

Think of a Key Pair as a **house lock and key**.

- The **lock** is installed on the house.
- The **key** stays with the owner.

Only the correct key can open the lock.

Similarly:

- The **Public Key** is stored on the EC2 instance.
- The **Private Key** remains with you.

Only the matching Private Key can authenticate and allow access.

---

# 🌍 Real-Life Example

Imagine you rent a locker.

```
Locker

↓

Lock Installed

↓

Only Your Key Opens It
```

Even if someone knows where the locker is, they cannot open it without the correct key.

AWS follows the same concept using Public and Private Keys.

---

# 🏗️ Key Pair Architecture

```
               AWS

Create Key Pair

        │
        ▼

 ┌───────────────────────┐
 │                       │
 ▼                       ▼

Public Key          Private Key

Stored in EC2       Downloaded to You

       │                 │
       └────────┬────────┘
                │
                ▼
        Secure Authentication
                │
                ▼
          EC2 Instance Login
```

---

# 🔑 Public Key

The Public Key is automatically stored inside the EC2 instance.

Purpose:

- Identifies trusted users
- Verifies authentication
- Can be shared safely

You do **not** use the Public Key directly when logging in.

---

# 🔒 Private Key

The Private Key is downloaded when the Key Pair is created.

Example:

```
my-key.pem
```

Characteristics:

- Must remain secret
- Never share it
- Required for SSH login
- AWS cannot recover it if lost

---

# ⚖️ Public Key vs Private Key

| Public Key | Private Key |
|------------|-------------|
| Stored on EC2 | Stored with the user |
| Safe to share | Must remain secret |
| Used for verification | Used for authentication |
| Created by AWS | Downloaded once during creation |

---

# 🔄 How Key Pair Authentication Works

```
Create Key Pair

        │
        ▼

Public Key stored on EC2

        │

Private Key downloaded

        │
        ▼

User runs SSH

        │
        ▼

EC2 verifies Private Key

        │
        ▼

Authentication Successful

        │
        ▼

Login Allowed
```

If the keys do not match, access is denied.

---

# 🖥️ Linux EC2 Login

Linux instances use **SSH (Secure Shell)**.

Example:

```bash
ssh -i my-key.pem ec2-user@<Public-IP>
```

For Ubuntu:

```bash
ssh -i my-key.pem ubuntu@<Public-IP>
```

Common usernames:

| AMI | Default Username |
|------|------------------|
| Amazon Linux | ec2-user |
| Ubuntu | ubuntu |
| Red Hat | ec2-user |
| Debian | admin |
| CentOS | centos |

---

# 🪟 Windows EC2 Login

Windows instances use **Remote Desktop Protocol (RDP).**

Workflow:

```
Create Key Pair

↓

Launch Windows EC2

↓

Download RDP File

↓

Decrypt Administrator Password

↓

Login using RDP
```

The Private Key is used to decrypt the Windows Administrator password.

---

# 📄 .pem vs .ppk

AWS allows two key formats.

## .pem

Used by:

- Linux
- macOS
- OpenSSH
- AWS CloudShell

Example:

```
my-key.pem
```

---

## .ppk

Used by:

- PuTTY on Windows

Example:

```
my-key.ppk
```

---

# 🏗️ Creating a Key Pair

Steps:

```
AWS Console

↓

EC2

↓

Key Pairs

↓

Create Key Pair

↓

Enter Name

↓

Choose Format (.pem/.ppk)

↓

Download Private Key

↓

Done
```

Remember:

The Private Key can only be downloaded **once**.

---

# ⚠️ What Happens if You Lose the Private Key?

If you lose the Private Key:

- AWS cannot provide another copy.
- You cannot download it again.
- You cannot log in using that key.

Recovery options include:

- Creating a new EC2 instance from an AMI.
- Attaching the root EBS volume to another instance and updating the authorized SSH keys.
- Using AWS Systems Manager Session Manager (if configured beforehand).

---

# ⭐ Best Practices

- Store Private Keys securely.
- Never share Private Keys.
- Use different Key Pairs for different environments.
- Rotate keys periodically.
- Use IAM and Systems Manager where possible to reduce direct SSH access.
- Restrict SSH access with Security Groups.

---

# ❌ Common Mistakes

- Losing the Private Key.
- Sharing the `.pem` file.
- Storing keys in public GitHub repositories.
- Giving the `.pem` file incorrect permissions.
- Allowing SSH from `0.0.0.0/0` in production.

---

# 📊 Key Pair Summary

| Component | Purpose |
|-----------|---------|
| Public Key | Stored on EC2 |
| Private Key | Stored by User |
| SSH | Linux Login |
| RDP | Windows Login |
| .pem | OpenSSH/Linux/macOS |
| .ppk | PuTTY/Windows |

---

# 📝 Quick Revision

```
Create Key Pair

↓

Public Key

↓

Stored on EC2

↓

Private Key

↓

Stored with User

↓

SSH Authentication

↓

EC2 Login
```

---

# 🎤 Interview Questions

### 1. What is a Key Pair?

A Key Pair is a pair of cryptographic keys (Public Key and Private Key) used for secure authentication to an EC2 instance.

---

### 2. Why do we need a Key Pair?

To securely connect to an EC2 instance without using a traditional password.

---

### 3. Where is the Public Key stored?

On the EC2 instance.

---

### 4. Where is the Private Key stored?

With the user. AWS provides it only once during Key Pair creation.

---

### 5. Can AWS recover a lost Private Key?

No.

---

### 6. Which file format is used for Linux?

`.pem`

---

### 7. Which file format is used with PuTTY on Windows?

`.ppk`

---

### 8. Which protocol is used to connect to a Linux EC2 instance?

SSH (Secure Shell).

---

# 🎯 Chapter Summary

A Key Pair is the primary authentication mechanism for Amazon EC2. It uses public key cryptography, where the Public Key is stored on the instance and the Private Key remains with the user. During login, AWS verifies that both keys match before granting access. Keeping the Private Key secure is critical because AWS cannot recover it if it is lost.
