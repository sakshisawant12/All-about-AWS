# Amazon Elastic Block Store (EBS)

## What is Amazon EBS?

Amazon Elastic Block Store (EBS) is a **persistent block storage service** provided by AWS for Amazon EC2 instances.

It acts as the **virtual hard disk** attached to an EC2 instance.

Without storage, an EC2 instance cannot save:

- Operating System
- Applications
- Database
- User Files
- Logs

---

## Real-Life Analogy

Imagine buying a laptop.

```text
Laptop
│
├── CPU
├── RAM
└── SSD/HDD
```

Now compare it with AWS.

```text
EC2 Instance
│
├── CPU
├── Memory
└── Amazon EBS
```

Just like a laptop stores data inside its hard disk, EC2 stores its data inside an EBS volume.

---

## Definition

Amazon Elastic Block Store (EBS) is a **network-attached block storage service** that provides durable and persistent storage for Amazon EC2 instances.

---

## Why Do We Need EBS?

Without EBS:

- No Operating System
- No Website Files
- No Database
- No Application Storage
- No Permanent Data

With EBS:

- Persistent Storage
- High Performance
- Snapshots
- Encryption
- Resize Anytime
- Backup Support

---

## Key Features

- Persistent Storage
- Block-Level Storage
- SSD & HDD Volume Types
- Snapshot Support
- Encryption
- High Availability
- Elastic Resize
- High Performance

---

## How EBS Works

```text
               AWS Cloud

          +------------------+
          |   EC2 Instance   |
          +--------+---------+
                   |
          Attached EBS Volume
                   |
          +--------▼---------+
          |   Amazon EBS     |
          +------------------+
                   |
      -------------------------
      |          |            |
 Operating   Applications   Database
 System         Files        Data
```

---

## Availability Zone Rule

An EBS volume exists inside **one Availability Zone (AZ)**.

Example:

```text
Mumbai Region

├── ap-south-1a
│      EC2
│       │
│      EBS
│
└── ap-south-1b
```

An EBS volume created in one Availability Zone **cannot be directly attached** to an EC2 instance in another Availability Zone.

To move data between AZs:

1. Create Snapshot
2. Create New Volume
3. Restore in another AZ

---

## Maximum Volume Size

Depending on the volume type, EBS supports sizes from:

- Minimum: **1 GiB**
- Maximum: **64 TiB**

---

## Common Use Cases

- Linux Root Volume
- Windows Root Volume
- Database Storage
- Website Hosting
- Log Storage
- File Storage
- Application Storage

---

## Persistence

### Stop Instance

```text
EC2 Stopped

↓

EBS Still Exists

↓

Data Remains Safe
```

---

### Reboot Instance

```text
Restart EC2

↓

EBS Remains Attached

↓

No Data Loss
```

---

### Terminate Instance

Default:

```text
Terminate EC2

↓

Root EBS Deleted
```

Optional:

```text
Delete on Termination = Disabled

↓

EBS Remains

↓

Can Attach to Another EC2
```

---

## EBS vs Physical Hard Disk

| Physical Hard Disk | Amazon EBS |
|--------------------|------------|
| Installed inside computer | Attached to EC2 |
| Physical device | Virtual storage |
| Manual replacement | Managed by AWS |
| Manual backup | Snapshot |
| Physical failure possible | Highly durable within an AZ |

---

## Advantages

- Persistent Storage
- High Availability
- Easy Backup
- Encryption
- High Performance
- Resize Anytime
- Reliable
- Secure

---

## Limitations

- Works only with EC2
- Limited to one Availability Zone
- Charges apply even if volume is unused
- Cannot directly attach across AZs

---

## Quick Revision

✔ EBS is Block Storage

✔ Works with EC2

✔ Persistent Storage

✔ Stores Operating System

✔ Supports Snapshots

✔ Supports Encryption

✔ Supports Resize

✔ Exists inside one Availability Zone

---

# Interview Questions

### 1. What is Amazon EBS?

Amazon EBS is a persistent block storage service for Amazon EC2.

---

### 2. Is EBS Object Storage?

No.

It is Block Storage.

---

### 3. Does EBS store data permanently?

Yes.

Data remains after stop and reboot.

---

### 4. Can we increase EBS volume size?

Yes.

Using Elastic Volumes.

---

### 5. Can one EBS volume be attached to multiple EC2 instances?

Normally, no.

One EBS volume is attached to one EC2 instance at a time (except specific Multi-Attach supported configurations).

---

### 6. Is EBS regional?

No.

An EBS volume belongs to one Availability Zone.

---

## Summary

- Amazon EBS is persistent block storage for EC2.
- It stores the operating system, applications, databases, and files.
- It supports snapshots, encryption, resizing, and multiple volume types.
- Every EBS volume belongs to a single Availability Zone.
- It is one of the most commonly used AWS storage services.
