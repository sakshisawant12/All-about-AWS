# Resize and Modify an Amazon EBS Volume

As your applications grow, you may need more storage or better performance.

Amazon EBS allows you to modify a volume **without creating a new one** using a feature called **Elastic Volumes**.

With Elastic Volumes, you can:

- Increase storage size
- Change the volume type
- Increase IOPS
- Increase Throughput

In most cases, these changes can be made without stopping the EC2 instance.

---

# What is Elastic Volumes?

Elastic Volumes is an Amazon EBS feature that lets you modify an existing volume while it is attached to a running EC2 instance.

Example:

```text
Before

gp2
100 GiB

↓

Modify Volume

↓

gp3
200 GiB

↓

No New Volume Required
```

---

# Why Resize an EBS Volume?

Common reasons include:

- Disk space is almost full.
- Database size has increased.
- Website traffic has grown.
- Better performance is required.
- Upgrade from gp2 to gp3.

---

# What Can Be Modified?

You can modify:

- Volume Size
- Volume Type
- IOPS
- Throughput (supported volume types)

---

# Example

Before

```text
Volume Type : gp2

Size : 100 GiB

IOPS : Default
```

After

```text
Volume Type : gp3

Size : 200 GiB

IOPS : 6000

Throughput : 250 MB/s
```

---

# Resize Workflow

```text
Current Volume

↓

Modify Volume

↓

AWS Updates Volume

↓

Extend File System

↓

More Storage Available
```

---

# Step 1: Open the EC2 Console

Navigate to:

```text
EC2

↓

Volumes
```

Select the volume you want to modify.

---

# Step 2: Modify the Volume

Click:

```text
Actions

↓

Modify Volume
```

You can now change:

- Size
- Volume Type
- IOPS
- Throughput

Click:

```text
Modify
```

AWS starts updating the volume.

---

# Step 3: Verify the Modification

Go to:

```text
Volumes

↓

Modification State
```

Possible states:

- Modifying
- Optimizing
- Completed

Even during optimization, the volume is generally available for use.

---

# Step 4: Check the New Disk Size

Connect to the EC2 instance.

Run:

```bash
lsblk
```

Example:

Before

```text
xvdf

100G
```

After

```text
xvdf

200G
```

The disk is larger, but the file system still uses the old size.

---

# Step 5: Extend the Partition

If the volume contains a partition, extend it.

Example:

```bash
sudo growpart /dev/xvdf 1
```

Where:

- `/dev/xvdf` → Disk
- `1` → Partition Number

If the volume does not use partitions, you can skip this step.

---

# Step 6: Extend the File System

## For ext4

```bash
sudo resize2fs /dev/xvdf1
```

---

## For XFS

```bash
sudo xfs_growfs /data
```

Replace `/data` with your mount point.

---

# Step 7: Verify the New Size

Run:

```bash
df -h
```

Example:

Before

```text
Filesystem

100G
```

After

```text
Filesystem

200G
```

Storage expansion is complete.

---

# Change Volume Type

Example:

```text
gp2

↓

gp3
```

Benefits:

- Better performance
- Lower cost for many workloads
- Independent IOPS and Throughput

AWS recommends gp3 for new workloads.

---

# Increase IOPS

Before

```text
3000 IOPS
```

After

```text
6000 IOPS
```

Useful for:

- Databases
- High-performance applications
- Transaction-heavy workloads

---

# Increase Throughput

Before

```text
125 MB/s
```

After

```text
500 MB/s
```

Useful for:

- Large file transfers
- Data analytics
- Video processing
- Big data workloads

---

# Can You Reduce Volume Size?

No.

Amazon EBS allows you to increase volume size but **does not support reducing** the size of an existing volume.

To reduce storage:

```text
Create Snapshot

↓

Create Smaller Volume

↓

Copy Data

↓

Attach New Volume
```

---

# Real-World Example

A company has:

```text
EC2

↓

100 GiB gp2

↓

Disk Almost Full
```

Administrator:

```text
Modify Volume

↓

gp3

↓

200 GiB

↓

Resize File System

↓

Problem Solved
```

No new EC2 instance is required.

---

# Best Practices

- Take a snapshot before modifying a volume.
- Monitor disk usage using Amazon CloudWatch.
- Use gp3 for most new workloads.
- Extend the file system after increasing the volume size.
- Verify the file system after resizing.

---

# Common Mistakes

### Forgetting to Resize the File System

Increasing the EBS volume size alone does not make the additional space usable.

You must also extend the file system.

---

### Forgetting to Take a Snapshot

Always create a snapshot before major storage changes.

---

### Trying to Reduce Volume Size

Amazon EBS does not support shrinking an existing volume.

---

### Choosing the Wrong File System Command

| File System | Command |
|-------------|----------|
| ext4 | `resize2fs` |
| XFS | `xfs_growfs` |

---

# Interview Questions

## 1. Can an EBS volume be resized?

Yes.

You can increase the size using Elastic Volumes.

---

## 2. Can you decrease an EBS volume size?

No.

You must create a smaller volume and migrate the data.

---

## 3. Which AWS feature allows live volume modification?

Elastic Volumes.

---

## 4. Which command checks the new disk size?

```bash
lsblk
```

---

## 5. Which command checks file system usage?

```bash
df -h
```

---

## 6. Which command resizes an ext4 file system?

```bash
resize2fs
```

---

## 7. Which command resizes an XFS file system?

```bash
xfs_growfs
```

---

# Quick Revision

✅ Elastic Volumes allow live modification

✅ Increase Size

✅ Change Volume Type

✅ Increase IOPS

✅ Increase Throughput

✅ Use `lsblk` to verify disk size

✅ Use `df -h` to verify file system size

✅ Extend the file system after resizing

✅ Cannot reduce EBS volume size

✅ Take a snapshot before modifying
