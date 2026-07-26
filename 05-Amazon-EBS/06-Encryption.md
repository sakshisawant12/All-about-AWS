# Amazon EBS Encryption

Amazon EBS Encryption protects your data by encrypting it while it is stored on an EBS volume.

Even if someone gains physical access to the storage device, they cannot read the data without the correct encryption key.

AWS uses **AWS Key Management Service (AWS KMS)** to manage encryption keys.

---

# What is Encryption?

Encryption is the process of converting readable data (Plain Text) into an unreadable format (Cipher Text).

Only someone with the correct **encryption key** can decrypt and read the data.

Example:

```text
Original Data

Hello AWS

        │

Encrypt

        │

▼

X#7@P!9Lm$2

        │

Decrypt using Key

        │

▼

Hello AWS
```

---

# Why Do We Need Encryption?

Without Encryption

```text
EBS Volume

↓

Someone Accesses Storage

↓

Reads Data
```

---

With Encryption

```text
EBS Volume

↓

Encrypted Data

↓

Without Key

↓

Cannot Read
```

---

# What Gets Encrypted?

When EBS encryption is enabled, AWS encrypts:

- Data stored on the EBS volume
- Snapshots
- Data moving between the EC2 instance and the EBS volume
- Volumes created from encrypted snapshots

---

# How EBS Encryption Works

```text
          EC2 Instance
                │
                ▼
        Write Data to EBS
                │
                ▼
      AWS KMS Encryption Key
                │
                ▼
      Encrypt Data Automatically
                │
                ▼
        Amazon EBS Volume
```

When the EC2 instance reads data:

```text
Amazon EBS

↓

Decrypt using KMS

↓

Send Plain Text

↓

EC2 Instance
```

The encryption and decryption process is automatic.

---

# AWS Key Management Service (AWS KMS)

AWS KMS is the service that creates, stores, rotates, and manages encryption keys.

Think of KMS as a secure key locker.

```text
                AWS KMS

           Stores Encryption Keys

                 │

                 ▼

      Amazon EBS Encryption
```

---

# Types of KMS Keys

AWS provides two main types of keys.

## 1. AWS Managed Key

- Created automatically by AWS
- Free to use
- Easy to manage
- Best for most workloads

Example:

```text
alias/aws/ebs
```

---

## 2. Customer Managed Key (CMK)

Created and managed by you.

You can:

- Rotate keys
- Control permissions
- Enable or disable keys
- Audit usage
- Share with other AWS services (where supported)

Best for production environments with strict security requirements.

---

# Creating an Encrypted EBS Volume

1. Open AWS Console.
2. Go to **EC2**.
3. Click **Volumes**.
4. Select **Create Volume**.
5. Choose:

- Volume Type
- Size
- Availability Zone

6. Enable:

```text
☑ Encrypt this volume
```

7. Choose a KMS Key.

Example:

```text
alias/aws/ebs
```

8. Click **Create Volume**.

AWS encrypts the volume automatically.

---

# Encrypted Snapshot

```text
Encrypted Volume

↓

Create Snapshot

↓

Encrypted Snapshot
```

Encryption is preserved.

---

# Restore from an Encrypted Snapshot

```text
Encrypted Snapshot

↓

Create Volume

↓

Encrypted Volume
```

The restored volume remains encrypted unless you intentionally choose a different supported encryption option.

---

# Encrypt an Existing Unencrypted Volume

An existing unencrypted EBS volume cannot be encrypted directly.

Instead:

```text
Unencrypted Volume

↓

Create Snapshot

↓

Copy Snapshot

↓

Enable Encryption

↓

Create New Encrypted Volume

↓

Attach to EC2
```

---

# Default EBS Encryption

AWS allows you to enable **Default EBS Encryption** for an AWS Region.

When enabled:

```text
New EBS Volume

↓

Automatically Encrypted
```

No need to manually enable encryption each time.

---

# Enable Default Encryption

Go to:

```text
EC2

↓

Settings

↓

EBS Encryption

↓

Enable
```

Select the default KMS key.

---

# Advantages of EBS Encryption

- Protects sensitive data
- Automatic encryption
- Automatic decryption
- Secure backups
- Integrated with AWS KMS
- Meets many compliance requirements

---

# Encryption at Rest

Encryption at Rest means:

```text
Data Stored

↓

Encrypted
```

Example:

```text
EBS Volume

↓

Encrypted Data
```

---

# Encryption in Transit

Encryption in Transit means:

```text
EC2

↓

Encrypted Data Transfer

↓

EBS
```

AWS encrypts data moving between supported EC2 instances and encrypted EBS volumes.

---

# Real-World Example

A bank stores customer information.

```text
Customer Data

↓

Amazon EC2

↓

Encrypted EBS

↓

Protected using AWS KMS
```

Even if the storage device is stolen,

the attacker cannot read the data without the encryption key.

---

# Best Practices

- Enable Default EBS Encryption.
- Use Customer Managed Keys for production environments that require additional control.
- Rotate customer-managed keys according to your organization's security policy.
- Use IAM policies to control KMS key access.
- Take encrypted snapshots for sensitive data.
- Never disable or delete KMS keys without understanding the impact on encrypted resources.

---

# Common Mistakes

### Forgetting to Enable Encryption

Always encrypt production volumes.

---

### Deleting KMS Keys

If a required KMS key is deleted or becomes unavailable, data encrypted with that key may become inaccessible.

---

### Sharing Keys Unnecessarily

Follow the principle of least privilege.

Only authorized users should access encryption keys.

---

# Interview Questions

## 1. What service manages EBS encryption?

**Answer:**

AWS Key Management Service (AWS KMS)

---

## 2. Does encryption affect snapshots?

**Answer:**

Yes.

Encrypted volumes create encrypted snapshots.

---

## 3. Can an existing unencrypted volume be encrypted directly?

**Answer:**

No.

Create a snapshot, copy it with encryption enabled, then create a new encrypted volume.

---

## 4. What is the default AWS-managed EBS key?

**Answer:**

```text
alias/aws/ebs
```

---

## 5. What is the difference between AWS Managed Keys and Customer Managed Keys?

| AWS Managed Key | Customer Managed Key |
|-----------------|----------------------|
| Created by AWS | Created by You |
| Less control | Full control |
| Automatic management | You manage permissions and lifecycle |
| Good for general workloads | Best for strict security requirements |

---

## 6. What is Encryption at Rest?

Data stored on disk is encrypted.

---

## 7. What is Encryption in Transit?

Data moving between EC2 and EBS is encrypted for supported configurations.

---

# Quick Revision

✅ EBS supports encryption

✅ AWS KMS manages encryption keys

✅ Encrypts volumes automatically

✅ Snapshots remain encrypted

✅ Restored volumes remain encrypted

✅ Enable Default EBS Encryption

✅ Encryption at Rest protects stored data

✅ Encryption in Transit protects moving data

✅ AWS Managed Key → `alias/aws/ebs`

✅ Customer Managed Key → Full control
