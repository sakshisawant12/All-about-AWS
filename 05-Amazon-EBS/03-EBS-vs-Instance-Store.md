# EBS vs Instance Store

When launching an Amazon EC2 instance, AWS provides two types of storage:

1. Amazon EBS (Elastic Block Store)
2. Instance Store

Choosing the right storage depends on whether your data needs to be **persistent** or **temporary**.

---

# What is Amazon EBS?

Amazon EBS is a **persistent block storage** service for EC2 instances.

Data stored in an EBS volume remains even after the EC2 instance is stopped or restarted.

Example:

```text
EC2 Instance
      │
      ▼
+----------------+
|   EBS Volume   |
+----------------+
      │
      ▼
 Operating System
 Applications
 Database
 User Files
```

---

# What is Instance Store?

Instance Store is **temporary storage** physically attached to the host machine where your EC2 instance runs.

It provides very fast storage but **does not preserve data** when the instance is stopped, terminated, or the underlying host fails.

Example:

```text
EC2 Instance
      │
      ▼
+----------------------+
|  Instance Store SSD  |
+----------------------+
      │
      ▼
 Temporary Data
 Cache
 Scratch Files
```

---

# Persistence

## Amazon EBS

```text
Stop EC2

↓

Data Remains

↓

Start EC2

↓

Everything is Still Available
```

---

## Instance Store

```text
Stop EC2

↓

Storage Removed

↓

Data Lost
```

---

# Storage Location

## Amazon EBS

```text
EC2

↓

Network

↓

Amazon EBS
```

EBS is **network-attached storage**.

---

## Instance Store

```text
EC2

↓

Local Disk

↓

Physical Server
```

Instance Store is directly attached to the physical host machine.

---

# Performance

| Storage | Performance |
|----------|-------------|
| EBS | High |
| Instance Store | Very High |

Because Instance Store is physically attached to the host, it generally offers lower latency than network-attached EBS.

---

# Data Durability

## Amazon EBS

AWS automatically replicates EBS data within the same Availability Zone.

This protects against hardware failures.

---

## Instance Store

No automatic replication.

If the physical host fails, the data is lost.

---

# Cost

| Amazon EBS | Instance Store |
|------------|----------------|
| Charged separately | Included with supported instance types |

You pay for the size and type of EBS volume you provision.

Instance Store storage is included in the cost of eligible EC2 instance types.

---

# Backup Support

## Amazon EBS

Supports:

- Snapshots
- Backup
- Restore

Example:

```text
EBS Volume

↓

Snapshot

↓

Amazon S3 (Managed by AWS)
```

---

## Instance Store

Does not support snapshots.

If data is important, you must copy it elsewhere before it is lost.

---

# Encryption

| Storage | Encryption |
|----------|------------|
| Amazon EBS | Supported |
| Instance Store | Not supported through EBS encryption features |

EBS supports encryption using AWS Key Management Service (AWS KMS).

---

# Resize Support

## Amazon EBS

Supports online resizing.

You can:

- Increase storage size
- Change volume type
- Increase IOPS
- Increase throughput

---

## Instance Store

Cannot be resized independently.

Its size depends on the EC2 instance type.

---

# Availability Zone

## Amazon EBS

One volume belongs to one Availability Zone.

It can only be attached to EC2 instances in the same AZ.

---

## Instance Store

Always attached to the physical host where the EC2 instance is running.

---

# Real-World Use Cases

## Amazon EBS

- Operating System
- Database
- Website Files
- Application Storage
- User Data
- Logs

---

## Instance Store

- Cache
- Temporary Files
- Scratch Space
- Buffer Storage
- High-speed Processing

---

# Example 1

Website Hosting

```text
Internet

↓

EC2

↓

Amazon EBS

↓

Website Files
```

Reason:

Website data must not disappear.

---

# Example 2

Video Processing

```text
Upload Video

↓

EC2

↓

Instance Store

↓

Process Video

↓

Upload Output to S3
```

Reason:

Temporary files are deleted after processing.

---

# Comparison Table

| Feature | Amazon EBS | Instance Store |
|---------|------------|----------------|
| Storage Type | Block Storage | Local Storage |
| Persistent | ✅ Yes | ❌ No |
| Data After Stop | Remains | Lost |
| Data After Termination | Optional (depends on Delete on Termination setting) | Lost |
| Network Attached | ✅ Yes | ❌ No |
| Local Disk | ❌ No | ✅ Yes |
| Snapshots | ✅ Yes | ❌ No |
| Encryption | ✅ Yes | Not through EBS encryption |
| Resize | ✅ Yes | ❌ No |
| Best For | OS, Databases, Applications | Cache, Temporary Files |
| Cost | Separate charge | Included with supported instances |

---

# Which One Should You Choose?

Choose **Amazon EBS** if:

- Data is important
- You need backups
- You need persistence
- You run databases
- You host websites

Choose **Instance Store** if:

- Data is temporary
- Maximum performance is required
- Files can be recreated
- You need high-speed caching

---

# Interview Questions

## 1. What is the main difference between EBS and Instance Store?

**Answer:**

EBS provides persistent storage, while Instance Store provides temporary storage.

---

## 2. Which storage survives an EC2 stop?

**Answer:**

Amazon EBS.

---

## 3. Which storage provides the highest performance?

**Answer:**

Instance Store.

---

## 4. Which storage supports snapshots?

**Answer:**

Amazon EBS.

---

## 5. Which storage should be used for databases?

**Answer:**

Amazon EBS.

---

## 6. Which storage should be used for caching?

**Answer:**

Instance Store.

---

## 7. Can Instance Store be resized?

**Answer:**

No. Its size depends on the EC2 instance type.

---

# Quick Revision

✅ EBS = Persistent Storage

✅ Instance Store = Temporary Storage

✅ EBS supports Snapshots

✅ Instance Store does not support Snapshots

✅ EBS supports Encryption

✅ Instance Store is physically attached to the host

✅ Databases → EBS

✅ Cache & Scratch Files → Instance Store

✅ EBS is network-attached block storage

✅ Instance Store provides very high local storage performance
