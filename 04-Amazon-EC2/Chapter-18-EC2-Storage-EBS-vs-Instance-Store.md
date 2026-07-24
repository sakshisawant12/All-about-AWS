# 📘 Chapter 18 - EC2 Storage: Amazon EBS vs Instance Store

> **"Amazon EBS provides persistent block storage, while Instance Store provides temporary high-speed local storage attached to the physical host."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is Amazon EBS?
- What is Instance Store?
- Persistent vs Ephemeral Storage
- EBS vs Instance Store
- Performance Comparison
- Data Persistence
- Billing Differences
- Real-world Use Cases
- Best Practices
- Interview Questions

---

# 📖 Why Does EC2 Need Storage?

Every EC2 instance needs storage to keep:

- Operating System
- Applications
- Configuration Files
- Logs
- User Data
- Databases

AWS provides two main storage options:

- Amazon Elastic Block Store (EBS)
- Instance Store

---

# 💡 Simple Definition

Imagine a desktop computer.

It has:

- **SSD/HDD** → Stores data permanently.
- **RAM Disk (Temporary Storage)** → Very fast but loses data when power is removed.

Similarly:

- **Amazon EBS = Permanent Storage**
- **Instance Store = Temporary Local Storage**

---

# 🏗️ Storage Architecture

```
                  EC2 Instance
                       │
        ┌──────────────┴──────────────┐
        ▼                             ▼
   Amazon EBS                  Instance Store
(Persistent Storage)      (Temporary Storage)
```

---

# 📦 What is Amazon EBS?

**Amazon Elastic Block Store (EBS)** is persistent block storage that can be attached to an EC2 instance.

Characteristics:

- Data persists after Stop/Start
- Can create Snapshots
- Supports Encryption
- Can be resized
- Can be detached and attached to another EC2 instance (within the same Availability Zone)

---

### Example

```
EC2

↓

EBS Volume

↓

Ubuntu Installed
```

Even if the EC2 instance stops, the data on the EBS volume remains.

---

# ⚡ What is Instance Store?

An **Instance Store** is temporary storage physically attached to the host server running the EC2 instance.

Characteristics:

- Extremely fast
- Local to the host
- No snapshots
- Cannot be detached
- Data is lost if the instance stops, hibernates, terminates, or the underlying host fails

---

### Example

```
EC2

↓

Instance Store

↓

Temporary Cache
```

---

# 🌍 Real-Life Example

Imagine working on a document.

### Amazon EBS

```
Save File

↓

Hard Disk

↓

File Still Exists Tomorrow
```

---

### Instance Store

```
Write on Whiteboard

↓

Power Goes Off

↓

Everything Erased
```

---

# 📊 EBS vs Instance Store

| Feature | Amazon EBS | Instance Store |
|----------|------------|----------------|
| Storage Type | Network-attached Block Storage | Local Physical Storage |
| Data Persistence | Persistent | Temporary (Ephemeral) |
| Stop Instance | Data Preserved | Data Lost |
| Start Instance | Data Available | Empty Storage |
| Snapshot Support | Yes | No |
| Encryption | Yes | Supported at hardware level on many instance types, but no EBS-style encryption management |
| Detach & Attach | Yes | No |
| Resize Volume | Yes | No |
| Availability Zone | Same AZ | Same Physical Host |
| Best Use | OS, Databases, Applications | Cache, Scratch Space, Temporary Files |

---

# 🔄 Data Persistence

## Amazon EBS

```
Launch EC2

↓

Create Files

↓

Stop Instance

↓

Start Instance

↓

Files Still Available
```

---

## Instance Store

```
Launch EC2

↓

Create Files

↓

Stop Instance

↓

Start Instance

↓

Files Lost
```

---

# 🚀 Performance Comparison

### Amazon EBS

- High performance
- Consistent throughput
- Suitable for production workloads
- Multiple volume types (gp3, io2, st1, sc1)

---

### Instance Store

- Very low latency
- Very high IOPS
- Excellent for temporary workloads
- Performance depends on the EC2 instance type

---

# 💰 Billing Comparison

## Amazon EBS

You pay for:

- Allocated storage (GB)
- Provisioned performance (for some volume types)
- Snapshots
- Data transfer where applicable

---

## Instance Store

Included in the price of supported EC2 instance types.

No separate storage charge.

---

# 🌍 Real-World Examples

## Amazon EBS

```
Production Database

↓

Amazon EBS

↓

Persistent Data
```

---

## Instance Store

```
Video Rendering

↓

Temporary Files

↓

Instance Store

↓

Delete After Job Completes
```

---

# ⭐ When Should You Use Amazon EBS?

Use Amazon EBS for:

- Operating Systems
- Production Applications
- Databases
- File Systems
- Long-term Storage
- Boot Volumes

---

# ⭐ When Should You Use Instance Store?

Use Instance Store for:

- Cache
- Temporary Files
- Scratch Data
- Buffer Storage
- High-speed Processing
- Intermediate Computation Results

---

# ⭐ Best Practices

- Use Amazon EBS for important data.
- Use EBS Snapshots for backups.
- Encrypt sensitive EBS volumes.
- Never store critical data only on Instance Store.
- Use Instance Store only for temporary data that can be recreated.

---

# ❌ Common Mistakes

- Storing production databases on Instance Store.
- Assuming Instance Store survives a Stop or Terminate action.
- Forgetting to back up EBS volumes with Snapshots.
- Confusing EBS with Amazon S3.
- Assuming every EC2 instance type includes Instance Store volumes.

---

# 📊 Storage Summary

| Feature | Amazon EBS | Instance Store |
|----------|------------|----------------|
| Persistent | ✅ Yes | ❌ No |
| Boot Volume | ✅ Yes | ❌ No |
| Snapshot Support | ✅ Yes | ❌ No |
| Encryption | ✅ Yes | Limited |
| Detachable | ✅ Yes | ❌ No |
| Production Workloads | ✅ Yes | ❌ No |
| Temporary Workloads | ❌ No | ✅ Yes |

---

# 📝 Quick Revision

```
Amazon EBS

↓

Persistent Storage

↓

Databases

↓

Operating System

------------------------

Instance Store

↓

Temporary Storage

↓

Cache

↓

Scratch Data
```

---

# 🎤 Interview Questions

### 1. What is the difference between Amazon EBS and Instance Store?

Amazon EBS is persistent block storage, while Instance Store is temporary local storage attached to the physical host.

---

### 2. Which storage type survives a Stop → Start operation?

**Amazon EBS**

---

### 3. Can you take a Snapshot of an Instance Store volume?

No.

Only Amazon EBS supports snapshots.

---

### 4. Which storage is best for production databases?

**Amazon EBS**

---

### 5. Which storage provides the lowest latency?

**Instance Store**, because it is physically attached to the host server.

---

### 6. Can an EBS volume be detached and attached to another EC2 instance?

Yes.

An EBS volume can be detached and attached to another compatible EC2 instance in the **same Availability Zone**.

---

# 🎯 Chapter Summary

Amazon EBS and Instance Store serve different purposes in AWS. **Amazon EBS** is persistent, network-attached block storage designed for operating systems, databases, and production workloads. **Instance Store** is temporary, high-performance local storage that is ideal for cache, scratch space, and other data that can be recreated. Choosing the correct storage option improves both performance and reliability.
