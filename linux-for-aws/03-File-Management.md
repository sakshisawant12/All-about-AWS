# 📁 Linux File Management

> Learn how to create, copy, move, rename, delete, and view files and directories in Linux. These commands are used daily by Linux Administrators and AWS Cloud Engineers.

---

## 📚 Table of Contents

- [Introduction](#-introduction)
- [mkdir](#-mkdir)
- [touch](#-touch)
- [cp](#-cp)
- [mv](#-mv)
- [rm](#-rm)
- [cat](#-cat)
- [less](#-less)
- [head](#-head)
- [tail](#-tail)
- [Linux in AWS](#-linux-in-aws)
- [Key Takeaways](#-key-takeaways)
- [Interview Questions](#-interview-questions)

---

# 📖 Introduction

Linux provides several commands to manage files and directories efficiently.

These commands help you:

- Create folders
- Create files
- Copy files
- Move or rename files
- Delete files
- Read file contents

Every Linux user and AWS Cloud Engineer should know these commands.

---

# 📂 mkdir

The `mkdir` command creates a new directory.

### Syntax

```bash
mkdir <directory_name>
```

### Example

```bash
mkdir project
```

Output

```text
project/
```

---

## Create Multiple Directories

```bash
mkdir aws linux docker
```

---

## Create Nested Directories

```bash
mkdir -p aws/ec2/linux
```

---

# 📄 touch

The `touch` command creates an empty file.

### Syntax

```bash
touch <file_name>
```

### Example

```bash
touch notes.txt
```

---

## Create Multiple Files

```bash
touch file1.txt file2.txt file3.txt
```

---

# 📋 cp

The `cp` command copies files or directories.

### Syntax

```bash
cp source destination
```

### Example

```bash
cp notes.txt backup.txt
```

Now both files exist.

---

## Copy a Directory

```bash
cp -r project backup_project
```

The `-r` option means **recursive**, which copies the directory along with all its contents.

---

# 🚚 mv

The `mv` command is used to move or rename files.

### Rename a File

```bash
mv notes.txt linux_notes.txt
```

---

### Move a File

```bash
mv notes.txt Documents/
```

---

# ❌ rm

The `rm` command deletes files and directories.

### Delete a File

```bash
rm notes.txt
```

---

### Delete a Directory

```bash
rm -r project
```

---

## Force Delete

```bash
rm -rf project
```

> ⚠️ **Warning:** Be careful with `rm -rf`. It permanently deletes files and folders without asking for confirmation.

---

# 📖 cat

The `cat` command displays the contents of a file.

### Example

```bash
cat notes.txt
```

Output

```text
Linux
AWS
Cloud Computing
```

---

# 📚 less

The `less` command displays large files one page at a time.

### Example

```bash
less notes.txt
```

Useful controls:

- ↑ ↓ → Move line by line
- Space → Next page
- `q` → Quit

---

# 🔝 head

Displays the first 10 lines of a file.

### Example

```bash
head notes.txt
```

Display the first 5 lines:

```bash
head -5 notes.txt
```

---

# 🔚 tail

Displays the last 10 lines of a file.

### Example

```bash
tail notes.txt
```

Display the last 20 lines:

```bash
tail -20 notes.txt
```

---

## Monitor a File in Real Time

```bash
tail -f application.log
```

Press:

```text
Ctrl + C
```

to stop monitoring.

---

# ☁️ Linux in AWS

While working with Amazon EC2, you'll frequently use these commands.

Example:

```bash
mkdir project

cd project

touch app.log

cat app.log

cp app.log backup.log

mv backup.log application.log

tail application.log
```

These commands are commonly used while managing applications, configuration files, and logs on Linux servers.

---

# 📝 Key Takeaways

- `mkdir` creates directories.
- `touch` creates files.
- `cp` copies files and directories.
- `mv` moves or renames files.
- `rm` deletes files and directories.
- `cat` displays file contents.
- `less` views large files page by page.
- `head` shows the beginning of a file.
- `tail` shows the end of a file.

---

# 🎯 Interview Questions

### 1. Which command creates a directory?

```bash
mkdir
```

---

### 2. Which command creates an empty file?

```bash
touch
```

---

### 3. What is the difference between `cp` and `mv`?

- `cp` creates a copy.
- `mv` moves or renames a file.

---

### 4. Why is `rm -rf` dangerous?

It permanently deletes files and directories without confirmation.

---

### 5. What is the difference between `cat`, `head`, and `tail`?

- `cat` displays the entire file.
- `head` displays the beginning of the file.
- `tail` displays the end of the file.

---

