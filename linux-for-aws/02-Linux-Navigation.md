# 🧭 Linux Navigation

> Learn how to navigate through the Linux file system using essential commands. Navigation is one of the first skills every Linux user and AWS Cloud Engineer must master.

---

## 📚 Table of Contents

- [What is Linux Navigation?](#-what-is-linux-navigation)
- [Present Working Directory (pwd)](#-present-working-directory-pwd)
- [List Directory Contents (ls)](#-list-directory-contents-ls)
- [Change Directory (cd)](#-change-directory-cd)
- [Absolute vs Relative Path](#-absolute-vs-relative-path)
- [Linux in AWS](#-linux-in-aws)
- [Key Takeaways](#-key-takeaways)
- [Interview Questions](#-interview-questions)

---

# 📖 What is Linux Navigation?

Linux Navigation means moving between directories and locating files using commands.

Unlike Windows, Linux navigation is primarily performed through the **command-line interface (CLI)**.

Cloud Engineers use navigation commands every day while managing Linux servers and AWS EC2 instances.

---

# 📍 Present Working Directory (`pwd`)

The `pwd` command displays the **current directory** you are working in.

### Syntax

```bash
pwd
```

### Example

```bash
$ pwd
```

Output

```text
/home/janvi
```

---

# 📂 List Directory Contents (`ls`)

The `ls` command lists files and directories.

### Syntax

```bash
ls
```

Example

```bash
$ ls
```

Output

```text
Documents
Downloads
Desktop
notes.txt
```

---

## Common Options

### Detailed List

```bash
ls -l
```

Example

```text
-rw-r--r--  notes.txt
drwxr-xr-x  Documents
```

---

### Show Hidden Files

```bash
ls -la
```

Hidden files begin with a `.`

Example

```text
.bashrc
.profile
.gitconfig
```

---

### Human Readable Sizes

```bash
ls -lh
```

Example

```text
4.0K notes.txt
12K report.pdf
```

---

# 📂 Change Directory (`cd`)

The `cd` command changes the current working directory.

### Open a Folder

```bash
cd Documents
```

---

### Go Back One Directory

```bash
cd ..
```

---

### Go to Home Directory

```bash
cd ~
```

---

### Go to Root Directory

```bash
cd /
```

---

### Go to Previous Directory

```bash
cd -
```

---

# 📌 Absolute vs Relative Path

## Absolute Path

An **Absolute Path** starts from the **Root Directory (`/`)**.

Example

```text
/home/janvi/Documents/Linux
```

No matter where you are, this path always points to the same location.

---

## Relative Path

A **Relative Path** starts from your **current directory**.

Suppose your current directory is:

```text
/home/janvi
```

To open the Documents folder:

```bash
cd Documents
```

Here, `Documents` is a relative path because it depends on your current location.

---

# ☁️ Linux in AWS

When connected to an EC2 instance, navigation commands are used to move through system directories.

Example

```bash
pwd
```

Output

```text
/home/ec2-user
```

List files

```bash
ls -la
```

Move to the configuration directory

```bash
cd /etc
```

Check logs

```bash
cd /var/log
```

These commands are used frequently while managing Linux servers on AWS.

---

# 📝 Key Takeaways

- `pwd` displays the current directory.
- `ls` lists files and directories.
- `ls -l` shows detailed information.
- `ls -la` displays hidden files.
- `cd` changes the current directory.
- Absolute paths begin with `/`.
- Relative paths depend on the current directory.

---

# 🎯 Interview Questions

### 1. What does the `pwd` command do?

It displays the current working directory.

---

### 2. What is the difference between `ls` and `ls -l`?

- `ls` lists file names.
- `ls -l` shows detailed information such as permissions, owner, size, and modification date.

---

### 3. What is the purpose of `cd`?

It changes the current working directory.

---

### 4. What is the difference between an absolute path and a relative path?

- An **Absolute Path** starts from the Root Directory (`/`).
- A **Relative Path** starts from the current working directory.

---

### 5. Which command takes you to the home directory?

```bash
cd ~
```

---

