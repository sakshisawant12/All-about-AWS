# 👥 Linux Users and Permissions

> Learn how Linux manages users and file permissions. Understanding users and permissions is essential for securing Linux systems and managing AWS EC2 instances.

---

## 📚 Table of Contents

- [Introduction](#-introduction)
- [Types of Users](#-types-of-users)
- [Basic User Commands](#-basic-user-commands)
- [Understanding File Permissions](#-understanding-file-permissions)
- [Permission Types](#-permission-types)
- [Permission Numbers](#-permission-numbers)
- [Changing Permissions](#-changing-permissions)
- [The sudo Command](#-the-sudo-command)
- [Linux in AWS](#-linux-in-aws)
- [Key Takeaways](#-key-takeaways)
- [Interview Questions](#-interview-questions)

---

# 📖 Introduction

Linux is a **multi-user operating system**, meaning multiple users can access the same system while keeping their files and permissions separate.

Permissions help protect files and directories from unauthorized access.

Every Linux administrator and AWS Cloud Engineer should understand how permissions work.

---

# 👤 Types of Users

Linux mainly has two types of users.

## Root User

The **root user** is the administrator of the Linux system.

The root user has complete control over the operating system, including:

- Installing software
- Managing users
- Changing system settings
- Accessing all files

Home directory:

```text
/root
```

---

## Normal User

A normal user has limited permissions.

They can:

- Create files
- Edit their own files
- Run applications
- Access permitted directories

Home directory example:

```text
/home/janvi
```

---

# 💻 Basic User Commands

## Display Current User

```bash
whoami
```

Example Output

```text
janvi
```

---

## Display Hostname

```bash
hostname
```

Example Output

```text
ip-172-31-15-100
```

---

## Display Current Date and Time

```bash
date
```

Example Output

```text
Fri Jul 17 10:30:15 IST 2026
```

---

# 🔐 Understanding File Permissions

Run the following command:

```bash
ls -l
```

Example Output

```text
-rwxr-xr-- 1 janvi users 2048 Jul 17 notes.txt
```

The first section represents file permissions.

```text
-rwxr-xr--
```

Breakdown:

```text
-
rwx
r-x
r--
```

| Section | Meaning |
|----------|---------|
| `-` | Regular file |
| `rwx` | Owner permissions |
| `r-x` | Group permissions |
| `r--` | Others permissions |

---

# 🛡️ Permission Types

| Symbol | Meaning |
|---------|---------|
| `r` | Read |
| `w` | Write |
| `x` | Execute |

### Read (r)

Allows viewing the file.

---

### Write (w)

Allows modifying the file.

---

### Execute (x)

Allows running the file as a program or script.

---

# 🔢 Permission Numbers

Linux also represents permissions using numbers.

| Number | Permission |
|---------|------------|
| 7 | Read + Write + Execute (rwx) |
| 6 | Read + Write (rw-) |
| 5 | Read + Execute (r-x) |
| 4 | Read Only (r--) |
| 0 | No Permission (---) |

---

## Common Permission Modes

### 755

```text
rwxr-xr-x
```

- Owner: Read, Write, Execute
- Group: Read, Execute
- Others: Read, Execute

Commonly used for directories and executable scripts.

---

### 644

```text
rw-r--r--
```

- Owner: Read, Write
- Group: Read
- Others: Read

Commonly used for text and configuration files.

---

# ⚙️ Changing Permissions

Use the `chmod` command.

### Give Execute Permission

```bash
chmod +x script.sh
```

---

### Set Permission to 755

```bash
chmod 755 script.sh
```

---

### Set Permission to 644

```bash
chmod 644 notes.txt
```

---

# 🛠️ The sudo Command

The `sudo` command allows a normal user to execute commands with administrative (root) privileges.

### Example

```bash
sudo dnf update
```

Another example:

```bash
sudo reboot
```

Without `sudo`, many system-level operations will fail due to insufficient permissions.

---

# ☁️ Linux in AWS

When working with an AWS EC2 instance, you often use permissions to:

- Run administrative commands using `sudo`
- Make shell scripts executable
- Protect configuration files
- Manage application access

Example:

```bash
chmod +x deploy.sh

./deploy.sh
```

Update the system:

```bash
sudo dnf update
```

These commands are part of everyday server administration.

---

# 📝 Key Takeaways

- Linux supports multiple users.
- The root user has full administrative access.
- Normal users have limited permissions.
- File permissions control who can read, write, and execute files.
- `chmod` changes file permissions.
- `sudo` allows temporary administrative access.
- Common permission modes are `755` and `644`.

---

# 🎯 Interview Questions

### 1. What are the two main types of Linux users?

- Root User
- Normal User

---

### 2. What does the `whoami` command do?

It displays the currently logged-in user.

---

### 3. What are the three Linux file permissions?

- Read (r)
- Write (w)
- Execute (x)

---

### 4. What is the difference between 755 and 644?

- **755** gives the owner full access and others read/execute access.
- **644** gives the owner read/write access and others read-only access.

---

### 5. What is the purpose of the `sudo` command?

It allows a normal user to execute commands with administrative (root) privileges.

---
