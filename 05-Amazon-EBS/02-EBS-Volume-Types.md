# Amazon EBS Volume Types

Amazon EBS provides different volume types to meet different workload requirements.

Some applications need **high performance**, while others need **low-cost storage**.

AWS divides EBS volumes into two categories:

- SSD (Solid State Drive)
- HDD (Hard Disk Drive)

---

# EBS Volume Categories

```text
                    Amazon EBS

                        │

        ┌───────────────┴───────────────┐

       SSD                             HDD

       │                                │

 ┌─────┴─────┐                  ┌───────┴───────┐

 gp3  gp2   io2              st1            sc1
```

---

# 1. General Purpose SSD (gp3)

The latest and recommended SSD volume type.

Suitable for most applications.

### Best For

- Web Servers
- Application Servers
- Small Databases
- Development
- Testing
- Boot Volumes

---

## Features

- Lowest cost SSD
- High performance
- Independent IOPS and Throughput
- Better than gp2
- Recommended by AWS

---

## Performance

| Feature | Value |
|----------|-------|
| Volume Size | 1 GiB – 64 TiB |
| Baseline IOPS | 3,000 |
| Maximum IOPS | 80,000 |
| Maximum Throughput | 2,000 MB/s |

---

## Example

```text
EC2

↓

Ubuntu Server

↓

gp3 Volume

↓

Website + Database
```

---

# 2. General Purpose SSD (gp2)

Older generation SSD volume.

AWS recommends using **gp3** instead.

---

## Best For

- Existing workloads
- Older EC2 deployments

---

## Features

- Performance depends on volume size
- Cheaper than Provisioned IOPS SSD
- Good balance of cost and performance

---

## Performance

| Feature | Value |
|----------|-------|
| Volume Size | 1 GiB – 16 TiB |
| Maximum IOPS | 16,000 |

---

## Difference Between gp2 and gp3

| gp2 | gp3 |
|------|------|
| Older generation | Latest generation |
| Performance depends on size | Performance independent of size |
| Lower throughput | Higher throughput |
| Less flexible | More flexible |
| Not recommended for new workloads | Recommended by AWS |

---

# 3. Provisioned IOPS SSD (io2)

Designed for applications requiring consistently high performance.

---

## Best For

- Oracle Database
- Microsoft SQL Server
- SAP
- Financial Applications
- Banking Systems
- Enterprise Applications

---

## Features

- Very low latency
- Highest durability
- Highest performance
- Consistent IOPS

---

## Performance

| Feature | Value |
|----------|-------|
| Maximum IOPS | 256,000* |
| Maximum Throughput | 4,000 MB/s* |

> *Performance limits depend on instance type and configuration.

---

## Example

```text
Bank Database

↓

Millions of Transactions

↓

io2 Volume
```

---

# 4. Throughput Optimized HDD (st1)

HDD designed for large sequential workloads.

---

## Best For

- Big Data
- Log Processing
- Streaming
- Data Warehousing

---

## Features

- High throughput
- Low cost
- Not suitable for boot volumes

---

## Example

```text
Application Logs

↓

100 GB Daily

↓

st1 Volume
```

---

# 5. Cold HDD (sc1)

Lowest-cost EBS storage.

Used for data accessed infrequently.

---

## Best For

- Backup Storage
- Archived Files
- Old Data
- Long-term Storage

---

## Features

- Lowest price
- Lowest performance
- Not for databases
- Not for boot volumes

---

## Example

```text
Company Backups

↓

Archive

↓

sc1 Volume
```

---

# SSD vs HDD

| SSD | HDD |
|------|-----|
| Faster | Slower |
| Low latency | High latency |
| High IOPS | Lower IOPS |
| Databases | Backup |
| Boot Volumes | Archive |

---

# Understanding IOPS

## What is IOPS?

IOPS stands for:

**Input/Output Operations Per Second**

It measures how many read or write operations a storage device can perform every second.

### Example

Imagine a library.

A librarian can issue books to students.

```text
100 Books Issued

↓

1 Second

↓

100 IOPS
```

Higher IOPS means faster storage performance.

---

# Understanding Throughput

Throughput measures how much data can be transferred every second.

Usually measured in:

- MB/s
- GB/s

---

## Example

```text
Road

↓

Cars Passing

↓

Throughput
```

A wider road allows more cars to pass.

Similarly, higher throughput allows more data transfer.

---

# IOPS vs Throughput

| IOPS | Throughput |
|------|------------|
| Number of operations | Amount of data transferred |
| Good for small files | Good for large files |
| Measured in Operations/sec | Measured in MB/s |

---

# Which Volume Should You Choose?

| Workload | Recommended Volume |
|----------|--------------------|
| EC2 Boot Volume | gp3 |
| Website | gp3 |
| Application Server | gp3 |
| Development | gp3 |
| SQL Database | io2 |
| Oracle Database | io2 |
| SAP | io2 |
| Log Processing | st1 |
| Data Warehouse | st1 |
| Backup | sc1 |
| Archive | sc1 |

---

# AWS Recommendation

For almost all new workloads, AWS recommends using **gp3** because it provides:

- Better performance
- Better flexibility
- Lower cost than gp2
- Independent IOPS and Throughput

---

# Interview Questions

## 1. Which EBS volume is recommended for most workloads?

**Answer:** gp3

---

## 2. Which EBS volume is best for databases?

**Answer:** io2

---

## 3. Which volume is used for backups?

**Answer:** sc1

---

## 4. Which volume provides the highest performance?

**Answer:** io2

---

## 5. Which volume is best for web servers?

**Answer:** gp3

---

## 6. What is IOPS?

Input/Output Operations Per Second.

---

## 7. What is Throughput?

The amount of data transferred per second.

---

# Quick Revision

✅ gp3 → General purpose SSD (Recommended)

✅ gp2 → Older SSD generation

✅ io2 → High-performance databases

✅ st1 → Big data & sequential workloads

✅ sc1 → Lowest-cost archive storage

✅ SSD → Fast

✅ HDD → Cheap

✅ IOPS → Number of operations per second

✅ Throughput → Data transferred per second
