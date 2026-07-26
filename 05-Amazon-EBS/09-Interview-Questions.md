# Amazon EBS Interview Questions

This document contains frequently asked Amazon EBS interview questions for:

- Cloud Support Engineer
- AWS Cloud Engineer
- System Administrator
- DevOps Engineer
- Technical Support Engineer

---

# Beginner Level

## 1. What is Amazon EBS?

Amazon Elastic Block Store (EBS) is a **persistent block storage** service for Amazon EC2 instances.

---

## 2. What type of storage is Amazon EBS?

Amazon EBS provides **Block Storage**.

---

## 3. Which AWS service mainly uses EBS?

Amazon EC2.

---

## 4. Is EBS persistent?

Yes.

Data remains after an EC2 instance is stopped or rebooted.

---

## 5. Can EBS exist without EC2?

Yes.

An EBS volume can exist independently even if it is not attached to an EC2 instance.

---

## 6. What is the maximum size of an EBS volume?

Up to **64 TiB** (depending on the volume type).

---

## 7. Can one EBS volume be attached to multiple EC2 instances?

Normally **No**.

Only specific **io1/io2 Multi-Attach** volumes support attachment to multiple EC2 instances in supported configurations.

---

## 8. Which EBS volume type does AWS recommend?

**gp3**

---

## 9. What are the different EBS volume types?

- gp3
- gp2
- io2
- st1
- sc1

---

## 10. What is the difference between SSD and HDD EBS volumes?

SSD volumes are optimized for low latency and random I/O.

HDD volumes are optimized for large sequential workloads.

---

# Intermediate Level

## 11. What is an EBS Snapshot?

A point-in-time backup of an EBS volume.

---

## 12. Where are snapshots stored?

In **Amazon S3**, managed internally by AWS.

---

## 13. Are snapshots incremental?

Yes.

After the first full snapshot, only changed blocks are stored.

---

## 14. Can we restore an EBS volume from a snapshot?

Yes.

Create a new EBS volume from the snapshot.

---

## 15. Can snapshots be copied to another AWS Region?

Yes.

---

## 16. What is Elastic Volumes?

A feature that allows modification of an EBS volume without creating a new one.

---

## 17. What can be modified using Elastic Volumes?

- Volume Size
- Volume Type
- IOPS
- Throughput

---

## 18. Can we reduce the size of an EBS volume?

No.

Amazon EBS only supports increasing the volume size.

---

## 19. Can EBS volumes be encrypted?

Yes.

Using **AWS Key Management Service (AWS KMS)**.

---

## 20. Which service manages EBS encryption keys?

AWS KMS.

---

## 21. Can an unencrypted EBS volume be encrypted directly?

No.

Create a snapshot, copy it with encryption enabled, then create a new encrypted volume.

---

## 22. Which command lists block devices in Linux?

```bash
lsblk
```

---

## 23. Which command checks disk usage?

```bash
df -h
```

---

## 24. Which command displays a volume UUID?

```bash
blkid
```

---

## 25. Which file controls automatic mounting?

```text
/etc/fstab
```

---

# Advanced Level

## 26. Why is gp3 preferred over gp2?

Because gp3 provides:

- Better performance
- Lower cost for many workloads
- Independent IOPS and Throughput
- More flexibility

---

## 27. What is IOPS?

Input/Output Operations Per Second.

It measures the number of read/write operations a storage device can perform every second.

---

## 28. What is Throughput?

The amount of data transferred per second.

Usually measured in MB/s.

---

## 29. Which volume type is best for databases?

io2

---

## 30. Which volume type is best for backups?

sc1

---

## 31. Which volume type is best for analytics and large sequential workloads?

st1

---

## 32. What happens if an encrypted snapshot is restored?

The new volume remains encrypted unless you choose a different supported encryption option during creation.

---

## 33. What happens if an EC2 instance is stopped?

The attached EBS volume remains and the data is preserved.

---

## 34. What happens if an EC2 instance is terminated?

The root EBS volume is deleted by default unless **Delete on Termination** is disabled.

Additional data volumes are not automatically deleted unless configured to do so.

---

## 35. Can an EBS volume be attached across Availability Zones?

No.

It must be attached to an EC2 instance in the same Availability Zone.

---

# Scenario-Based Questions

## 36. Your EC2 disk is 100% full. What would you do?

1. Create a snapshot.
2. Modify the EBS volume.
3. Increase the volume size.
4. Extend the file system.
5. Verify the new storage using `df -h`.

---

## 37. A user accidentally deleted important files. How would you recover them?

Restore a new EBS volume from a previously created snapshot and attach it to an EC2 instance.

---

## 38. The company wants encrypted storage. What would you do?

- Enable EBS Encryption.
- Use AWS KMS.
- Enable Default EBS Encryption for the Region.

---

## 39. You need to migrate data to another Availability Zone. How would you do it?

1. Create a snapshot.
2. Create a new volume in the target Availability Zone.
3. Attach the new volume to an EC2 instance.

---

## 40. Your manager asks you to reduce storage costs. What would you recommend?

- Delete unused volumes.
- Delete unnecessary snapshots.
- Use gp3 for general workloads.
- Use sc1 for archive storage.
- Monitor storage usage regularly.

---

# Practical Commands

## List Block Devices

```bash
lsblk
```

---

## Show Mounted File Systems

```bash
df -h
```

---

## Show UUID

```bash
blkid
```

---

## Format a Volume

```bash
mkfs.ext4 /dev/xvdf
```

---

## Mount a Volume

```bash
mount /dev/xvdf /data
```

---

## Unmount a Volume

```bash
umount /data
```

---

## Extend ext4 File System

```bash
resize2fs /dev/xvdf1
```

---

## Extend XFS File System

```bash
xfs_growfs /data
```

---

# One-Line Revision

| Topic | Key Point |
|--------|-----------|
| EBS | Persistent Block Storage |
| EC2 | Uses EBS for storage |
| Snapshot | Point-in-time Backup |
| gp3 | Recommended SSD |
| io2 | High-performance Database Storage |
| st1 | Throughput Optimized HDD |
| sc1 | Cold Archive HDD |
| Encryption | AWS KMS |
| Resize | Elastic Volumes |
| Backup | Snapshot |
| Monitor | CloudWatch |
| Auto Mount | /etc/fstab |
| Disk Check | df -h |
| Block Devices | lsblk |
| UUID | blkid |

---

# Common Interview Mistakes

❌ Saying EBS is Object Storage

❌ Saying snapshots are full backups every time

❌ Forgetting that EBS is Availability Zone specific

❌ Confusing IOPS with Throughput

❌ Forgetting to extend the file system after increasing volume size

❌ Forgetting to unmount before detaching a volume

❌ Saying EBS volume size can be decreased

---

# Final Revision

✅ EBS = Persistent Block Storage

✅ EC2 uses EBS for long-term storage

✅ gp3 is the recommended general-purpose volume

✅ io2 is best for high-performance databases

✅ st1 is for throughput-intensive workloads

✅ sc1 is for archive and infrequently accessed data

✅ Snapshots are incremental after the first backup

✅ Snapshots are stored in AWS-managed Amazon S3

✅ AWS KMS manages EBS encryption

✅ Elastic Volumes allow live modification

✅ Use `lsblk`, `df -h`, and `blkid` regularly

✅ Configure `/etc/fstab` for automatic mounting

✅ Take snapshots before major changes

✅ Use CloudWatch for monitoring

✅ Enable Default EBS Encryption

✅ Keep boot and data volumes separate
