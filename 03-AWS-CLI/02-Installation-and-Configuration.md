# ⚙️ Installation and Configuration

> **Chapter 2** | AWS Command Line Interface (AWS CLI)

---

## 📖 Table of Contents

- Installing AWS CLI
- Verify Installation
- Configure AWS CLI
- Configuration Parameters
- Configuration Files
- Test the Configuration
- Update Configuration
- Best Practices
- Key Takeaways
- Interview Questions

---

# Installing AWS CLI

AWS CLI is available for:

- 🪟 Windows
- 🐧 Linux
- 🍎 macOS

Download the latest version of **AWS CLI Version 2** from the official AWS website.

> [!TIP]
> AWS CLI Version 2 is the recommended version because it includes the latest features, improved security, and better performance.

---

# Verify Installation

After installation, open **Command Prompt**, **PowerShell**, or **Terminal** and run:

```bash
aws --version
```

Example Output:

```bash
aws-cli/2.31.0 Python/3.13 Windows/10 exe/AMD64
```

If the version is displayed, AWS CLI has been installed successfully.

---

# Configure AWS CLI

To connect AWS CLI with your AWS account, run:

```bash
aws configure
```

AWS CLI will ask for four values.

```
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]:
Default output format [None]:
```

---

# Configuration Parameters

## 🔑 AWS Access Key ID

A unique identifier used to authenticate your AWS account.

Example:

```
AKIA****************
```

---

## 🔒 AWS Secret Access Key

A secret key associated with your Access Key ID.

Example:

```
wJalrXUtnFEMI****************
```

> [!WARNING]
> Never share your **Secret Access Key** with anyone.

---

## 🌍 Default Region

The AWS Region where AWS CLI commands will run by default.

Examples:

```
ap-south-1
```

```
us-east-1
```

```
eu-west-1
```

---

## 📄 Default Output Format

Specifies how AWS CLI displays command results.

Supported formats:

- json
- table
- text
- yaml
- yaml-stream

Example:

```
json
```

---

# Configuration Files

AWS CLI stores configuration in two files.

## 📂 Credentials File

**Windows**

```
C:\Users\<username>\.aws\credentials
```

**Linux/macOS**

```
~/.aws/credentials
```

Example:

```ini
[default]
aws_access_key_id = AKIAxxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 📂 Config File

**Windows**

```
C:\Users\<username>\.aws\config
```

**Linux/macOS**

```
~/.aws/config
```

Example:

```ini
[default]
region = ap-south-1
output = json
```

---

# Test the Configuration

Run the following command:

```bash
aws sts get-caller-identity
```

If the configuration is correct, AWS returns details about the authenticated identity.

Example Output:

```json
{
  "UserId": "AIDAXXXXXXXXXXXXX",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/developer-user"
}
```

---

# Update Configuration

To update your AWS CLI configuration, run:

```bash
aws configure
```

Enter the new values when prompted.

---

# Common Configuration Commands

Check the configured region:

```bash
aws configure get region
```

Check the configured output format:

```bash
aws configure get output
```

List all configuration values:

```bash
aws configure list
```

---

# Best Practices

- Use IAM Users instead of the Root User.
- Enable MFA for your AWS account.
- Never hardcode Access Keys in source code.
- Rotate Access Keys regularly.
- Prefer IAM Roles for AWS services.
- Keep AWS CLI updated.

---

# Key Takeaways

- Install AWS CLI Version 2.
- Verify installation using `aws --version`.
- Configure AWS CLI using `aws configure`.
- Store credentials securely.
- Test the setup using `aws sts get-caller-identity`.

---

# Quick Summary

| Command | Purpose |
|----------|---------|
| `aws --version` | Check AWS CLI version |
| `aws configure` | Configure AWS CLI |
| `aws configure list` | View configuration |
| `aws configure get region` | View default region |
| `aws sts get-caller-identity` | Verify configured identity |

---

# Interview Questions

### 1. How do you install AWS CLI?

### 2. Which command checks the installed AWS CLI version?

### 3. Which command configures AWS CLI?

### 4. What information does `aws configure` ask for?

### 5. Where are AWS CLI credentials stored?

### 6. Where is the AWS CLI configuration file stored?

### 7. How do you verify that AWS CLI is configured correctly?

### 8. Why should you never share your Secret Access Key?

### 9. Which output formats are supported by AWS CLI?

### 10. What is the purpose of the default AWS Region?

---
