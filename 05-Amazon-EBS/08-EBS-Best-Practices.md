# Amazon EBS Best Practices

Following best practices helps improve the **performance, security, reliability, availability, and cost-efficiency** of Amazon EBS volumes.

These practices are commonly used by Cloud Engineers, System Administrators, and DevOps Engineers in production environments.

---

# 1. Use gp3 for Most Workloads

AWS recommends using **gp3** as the default EBS volume type.

Benefits:

- Better performance
- Lower cost than gp2 for many workloads
- Independent IOPS and Throughput
- Easy to scale

Recommended for:

- Web Servers
- Application Servers
- Development
- Testing
- Boot Volumes

```text
Recommended

gp3

✔ High Performance

✔ Cost Effective

✔ Flexible
```

---

# 2. Enable EBS Encryption

Always encrypt production volumes.

Encryption protects sensitive information if the storage device is compromised.

Use:

- AWS Managed Key
- Customer Managed Key (AWS KMS)

```text
EC2

↓

Encrypted EBS

↓

Secure Data
```

---

# 3. Enable Default EBS Encryption

Instead of manually enabling encryption for every volume,

enable **Default EBS Encryption** for the AWS Region.

Benefits:

- Prevents accidental unencrypted volumes
- Saves time
- Improves security

---

# 4. Take Regular Snapshots

Snapshots protect against:

- Accidental deletion
- Data corruption
- Hardware failure
- Disaster Recovery

Example:

```text
Production Volume

↓

Daily Snapshot

↓

Safe Backup
```

---

# 5. Automate Snapshots

Avoid creating snapshots manually.

Use:

- Amazon Data Lifecycle Manager (DLM)
- AWS Backup

Benefits:

- Automatic backups
- Consistent schedules
- Reduced human error

---

# 6. Delete Unused Snapshots

Snapshots consume storage.

Unused snapshots increase AWS costs.

Review snapshots regularly.

Delete:

- Old
- Duplicate
- Unnecessary

snapshots.

---

# 7. Monitor Volume Performance

Use **Amazon CloudWatch** to monitor:

- VolumeReadOps
- VolumeWriteOps
- Read Latency
- Write Latency
- Burst Balance (for supported volume types)

Monitoring helps detect:

- Slow disks
- Heavy workloads
- Performance bottlenecks

---

# 8. Use the Correct Volume Type

Choose the volume type according to the workload.

| Workload | Recommended Volume |
|----------|--------------------|
| Website | gp3 |
| Application | gp3 |
| Development | gp3 |
| Database | io2 |
| SAP | io2 |
| Analytics | st1 |
| Backup | sc1 |

Using the wrong volume type may increase costs or reduce performance.

---

# 9. Monitor Disk Usage

Regularly check storage usage.

Linux command:

```bash
df -h
```

Check attached block devices:

```bash
lsblk
```

Monitor before the disk becomes full.

---

# 10. Resize Before Storage Becomes Full

Avoid waiting until disk usage reaches 100%.

Example:

```text
Disk Usage

70%

↓

Plan Resize

↓

Avoid Downtime
```

Use CloudWatch alarms to notify administrators before storage becomes critical.

---

# 11. Use Tags

Always tag EBS resources.

Example:

| Key | Value |
|------|--------|
| Name | Production Database |
| Environment | Production |
| Owner | Cloud Team |
| Project | E-Commerce |

Benefits:

- Easy identification
- Cost allocation
- Automation
- Resource management

---

# 12. Unmount Before Detaching

Always unmount a file system before detaching an EBS volume.

```bash
sudo umount /data
```

This helps prevent:

- File corruption
- Data loss
- Incomplete writes

---

# 13. Keep Boot and Data Volumes Separate

Example:

```text
EC2

├── Root Volume

└── Data Volume
```

Benefits:

- Easier backups
- Easier resizing
- Better maintenance
- Faster recovery

---

# 14. Use Multi-Attach Only When Required

Amazon EBS Multi-Attach allows specific **io1/io2** volumes to be attached to multiple EC2 instances in supported configurations.

Use only when your application and file system support it.

Example:

```text
        io2 Volume

      ┌────┴────┐

    EC2-1    EC2-2
```

---

# 15. Keep Volumes in the Same Availability Zone

An EBS volume can only be attached to an EC2 instance in the **same Availability Zone**.

Example:

```text
Mumbai

├── ap-south-1a

│      EC2

│       │

│      EBS

└── ap-south-1b
```

To use the data in another Availability Zone:

```text
Create Snapshot

↓

Create New Volume

↓

Attach to EC2
```

---

# 16. Take a Snapshot Before Major Changes

Before:

- Resizing
- Partitioning
- Formatting
- Operating System Upgrade
- Database Upgrade

Create a snapshot.

```text
Snapshot

↓

Modify

↓

Recover if Needed
```

---

# 17. Follow the Principle of Least Privilege

Grant only the permissions users require.

Use IAM policies to restrict access to:

- Create Volume
- Delete Volume
- Create Snapshot
- Delete Snapshot
- Modify Volume

This improves security.

---

# 18. Clean Up Unused Volumes

Sometimes an EC2 instance is deleted but the EBS volume remains.

These are called **orphaned volumes**.

Example:

```text
EC2 Deleted

↓

EBS Still Exists

↓

Storage Charges Continue
```

Review and remove unused volumes after confirming they are no longer needed.

---

# 19. Use CloudWatch Alarms

Create alarms for:

- Low Disk Space
- High Read Latency
- High Write Latency
- High IOPS Utilization

Example:

```text
Disk Usage

90%

↓

CloudWatch Alarm

↓

Email Notification

↓

Administrator
```

---

# 20. Follow a Backup Strategy

Example:

```text
Daily Snapshot

↓

Weekly Verification

↓

Monthly Disaster Recovery Test
```

A backup is useful only if it can be restored successfully.

Regularly test snapshot restoration.

---

# Common Mistakes

❌ Forgetting to create snapshots

❌ Using gp2 instead of gp3 for new workloads

❌ Forgetting `/etc/fstab`

❌ Leaving unused volumes running

❌ Not monitoring disk usage

❌ Using databases on sc1 volumes

❌ Detaching volumes without unmounting

❌ Forgetting to enable encryption

---

# Production Checklist

| Task | Status |
|--------|--------|
| Use gp3 | ✅ |
| Enable Encryption | ✅ |
| Enable Default Encryption | ✅ |
| Take Snapshots | ✅ |
| Monitor CloudWatch | ✅ |
| Use Tags | ✅ |
| Resize Before Full | ✅ |
| Use Correct Volume Type | ✅ |
| Remove Unused Volumes | ✅ |
| Backup Regularly | ✅ |

---

# Interview Questions

## 1. Which EBS volume does AWS recommend?

**Answer:**

gp3

---

## 2. Why should snapshots be automated?

To ensure regular backups and reduce manual work.

---

## 3. Which AWS service monitors EBS performance?

Amazon CloudWatch.

---

## 4. Why should EBS volumes be tagged?

For easier identification, cost tracking, and automation.

---

## 5. Why should a file system be unmounted before detaching a volume?

To help prevent data corruption and incomplete writes.

---

## 6. What is an orphaned EBS volume?

An EBS volume that still exists after its EC2 instance has been deleted.

---

## 7. Which command checks disk usage?

```bash
df -h
```

---

## 8. Which command lists block devices?

```bash
lsblk
```

---

# Quick Revision

✅ Use gp3 for new workloads

✅ Enable Encryption

✅ Enable Default Encryption

✅ Take Regular Snapshots

✅ Automate Backups

✅ Monitor with CloudWatch

✅ Tag Resources

✅ Resize Before Disk Is Full

✅ Keep Root and Data Volumes Separate

✅ Clean Up Unused Volumes

✅ Test Backup Restoration

✅ Follow Least Privilege
