# Amazon EBS Snapshots

An Amazon EBS Snapshot is a **backup of an EBS volume**.

Snapshots help you recover data if a volume is accidentally deleted, corrupted, or damaged.

AWS stores snapshots automatically in **Amazon S3 (managed by AWS)**, but you do **not** access or manage the S3 bucket directly.

---

# Why Do We Need Snapshots?

Imagine your EC2 instance stores a production database.

```text
EC2
 │
 ▼
EBS Volume
 │
 ▼
Database
```

If the EBS volume fails or is accidentally deleted, all your data could be lost.

A snapshot allows you to restore the data by creating a new EBS volume.

---

# What Does a Snapshot Store?

A snapshot stores all the data present on an EBS volume at the time it is created.

It includes:

- Operating System
- Applications
- Configuration Files
- Databases
- User Files
- Logs

---

# How Snapshots Work

```text
          EC2 Instance
                │
                ▼
        Amazon EBS Volume
                │
      Create Snapshot
                │
                ▼
      Snapshot Stored in
      Amazon S3 (Managed)
                │
                ▼
      Create New EBS Volume
                │
                ▼
          Attach to EC2
```

---

# Snapshot Workflow

```text
Create EBS Volume
        │
        ▼
Store Data
        │
        ▼
Create Snapshot
        │
        ▼
Stored in AWS Managed S3
        │
        ▼
Create New Volume
        │
        ▼
Attach to EC2
```

---

# Creating a Snapshot

## Step 1

Open **AWS Management Console**

Go to:

```text
EC2

↓

Elastic Block Store

↓

Volumes
```

---

## Step 2

Select the EBS Volume.

Click

```text
Actions

↓

Create Snapshot
```

---

## Step 3

Provide

- Snapshot Name (optional)
- Description
- Tags (optional)

Click

```text
Create Snapshot
```

AWS starts creating the snapshot.

---

# Viewing Snapshots

Navigate to

```text
EC2

↓

Snapshots
```

You'll see

- Snapshot ID
- Volume ID
- Status
- Size
- Creation Time

---

# Restore from Snapshot

A snapshot cannot be directly attached to an EC2 instance.

Instead:

```text
Snapshot

↓

Create Volume

↓

Attach Volume

↓

Access Data
```

---

# Steps to Restore

## Step 1

Select Snapshot

---

## Step 2

Click

```text
Actions

↓

Create Volume
```

---

## Step 3

Choose

- Volume Type
- Size
- Availability Zone

---

## Step 4

Create Volume

---

## Step 5

Attach it to EC2.

---

# Incremental Snapshots

This is one of the most important interview topics.

The first snapshot copies the **entire EBS volume**.

```text
Volume

↓

Snapshot 1

Copies Everything
```

---

Suppose only one file changes.

The second snapshot stores **only the changed blocks**.

```text
Snapshot 1

↓

100 GB

↓

Snapshot 2

↓

Only Changed Blocks
```

AWS calls this an **Incremental Snapshot**.

---

# Example

Suppose your EBS volume size is

```text
100 GB
```

### First Snapshot

```text
100 GB copied
```

---

You modify only

```text
5 GB
```

Second Snapshot

```text
Only 5 GB stored
```

This saves storage space and reduces backup time.

---

# Snapshot Benefits

- Backup
- Disaster Recovery
- Migration
- Restore Data
- Clone Volumes
- Save Storage (Incremental)
- Easy Recovery

---

# Cross-Region Copy

AWS allows copying snapshots to another AWS Region.

Example

```text
Mumbai

↓

Snapshot

↓

Copy

↓

Singapore
```

Useful for

- Disaster Recovery
- Business Continuity
- Regional Migration

---

# Copy Snapshot Across Accounts

AWS also allows sharing snapshots with another AWS account (subject to permissions and encryption settings).

Example

```text
Account A

↓

Snapshot

↓

Shared

↓

Account B
```

Useful for

- Team Collaboration
- Migration
- Multi-account Environments

---

# Encrypted Snapshots

If the EBS volume is encrypted,

its snapshot is also encrypted.

```text
Encrypted Volume

↓

Encrypted Snapshot
```

AWS uses **AWS Key Management Service (KMS)** for encryption.

---

# Delete Snapshot

Deleting a snapshot:

```text
Snapshot Deleted

↓

EBS Volume Remains
```

Deleting a snapshot does **not** delete the original EBS volume.

---

# Snapshot Lifecycle

```text
Create Volume

↓

Store Data

↓

Create Snapshot

↓

Snapshot Stored

↓

Delete Volume

↓

Snapshot Still Exists

↓

Restore Anytime
```

---

# Snapshot Pricing

AWS charges for the amount of snapshot data stored.

Because snapshots are incremental,

you pay only for the changed blocks that are stored.

---

# Snapshot vs Backup

| Snapshot | Traditional Backup |
|------------|-------------------|
| Faster | Slower |
| Incremental | Often Full Copy |
| AWS Managed | Manual |
| Easy Restore | More Steps |
| Stored by AWS | Depends on Solution |

---

# Snapshot vs AMI

| Snapshot | AMI |
|------------|-----|
| Backup of a Volume | Template for EC2 |
| Stores Disk Data | Stores OS + Configuration + Snapshots |
| Used to Restore Storage | Used to Launch New EC2 Instances |

---

# Real-World Example

A company hosts an e-commerce website.

Every night:

```text
EC2

↓

EBS Volume

↓

Automatic Snapshot

↓

Stored Safely
```

If the server fails:

```text
Snapshot

↓

New Volume

↓

Attach

↓

Website Restored
```

---

# Best Practices

- Take snapshots before making major system changes.
- Use tags to identify snapshots.
- Delete unnecessary snapshots to reduce costs.
- Automate backups using **Amazon Data Lifecycle Manager (DLM)** or **AWS Backup**.
- Copy important snapshots to another Region for disaster recovery.

---

# Interview Questions

## 1. What is an EBS Snapshot?

A point-in-time backup of an Amazon EBS volume.

---

## 2. Where are snapshots stored?

In Amazon S3 managed by AWS.

---

## 3. Are snapshots incremental?

Yes.

Only changed blocks after the first snapshot are stored.

---

## 4. Can we restore an EBS volume from a snapshot?

Yes.

Create a new EBS volume from the snapshot.

---

## 5. Can snapshots be copied to another Region?

Yes.

---

## 6. Does deleting a snapshot delete the original EBS volume?

No.

---

## 7. What AWS service can automate snapshot creation?

- Amazon Data Lifecycle Manager (DLM)
- AWS Backup

---

# Quick Revision

✅ Snapshot = Backup of an EBS Volume

✅ Stored in AWS-managed Amazon S3

✅ First Snapshot = Full Backup

✅ Later Snapshots = Incremental

✅ Can Restore New EBS Volumes

✅ Can Copy Across Regions

✅ Supports Encryption

✅ Used for Backup & Disaster Recovery

✅ Delete Snapshot ≠ Delete Volume

✅ Automate with DLM or AWS Backup
