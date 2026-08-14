# Amazon S3 – Replication

## 1. What is S3 Replication?

**S3 Replication** automatically copies objects from one S3 bucket to another S3 bucket.

The source and destination buckets can be:

* In the same AWS Region
* In different AWS Regions
* In the same AWS account
* In different AWS accounts

Replication is useful for:

* Disaster recovery
* Data redundancy
* Compliance
* Geographic distribution
* Data sharing
* Business continuity

---

## 2. Types of S3 Replication

There are two main types:

### Cross-Region Replication (CRR)

Copies objects from a bucket in one AWS Region to a bucket in another AWS Region.

```text id="7q4m2x"
Region A
Source Bucket
     │
     │ CRR
     ↓
Region B
Destination Bucket
```

Example:

```text id="8x3n6p"
Mumbai
ap-south-1
     │
     │ CRR
     ↓
Singapore
ap-southeast-1
```

---

### Same-Region Replication (SRR)

Copies objects between buckets in the **same AWS Region**.

```text id="5m8r2q"
Region: Mumbai
     │
     ├── Source Bucket
     │       │
     │       │ SRR
     │       ↓
     └── Destination Bucket
```

---

## 3. CRR vs SRR

| Feature            | CRR                       | SRR                         |
| ------------------ | ------------------------- | --------------------------- |
| Full Name          | Cross-Region Replication  | Same-Region Replication     |
| Source Region      | Region A                  | Same Region                 |
| Destination Region | Region B                  | Same Region                 |
| Main Use           | DR, geographic redundancy | Compliance, data separation |
| Example            | Mumbai → Singapore        | Mumbai → Mumbai             |

### Easy Way to Remember

```text id="3y7p2m"
CRR
→ Cross Region

SRR
→ Same Region
```

---

## 4. S3 Replication Architecture

Basic replication flow:

```text id="6k4v8n"
                Source Region
                     │
                     ↓
               Source Bucket
                     │
                     │ Replication
                     ↓
             Replication IAM Role
                     │
                     ↓
             Destination Bucket
                     │
                     ↓
              Destination Region
```

---

## 5. Important Requirement: Versioning

**Versioning must be enabled on both the source and destination buckets for S3 live replication configurations.**

```text id="2p7m4x"
Source Bucket
Versioning Enabled
       │
       ↓
   Replication
       │
       ↓
Destination Bucket
Versioning Enabled
```

This is one of the most important points to remember.

---

## 6. Replication IAM Role

S3 needs permission to replicate objects from the source bucket to the destination bucket.

An **IAM role** is used by S3 for this purpose.

```text id="9n5q3k"
S3
 ↓
Assume IAM Role
 ↓
Read Source Objects
 ↓
Write Destination Objects
```

The role needs appropriate permissions for the replication process.

---

## 7. Replication Flow

Suppose we have:

```text id="x8m4p2"
Source:
ap-south-1
my-source-bucket
```

and:

```text id="q6n3v7"
Destination:
ap-southeast-1
my-destination-bucket
```

When an object is uploaded:

```text id="c5r9k1"
Application
     ↓
Source Bucket
     ↓
Replication Rule
     ↓
S3 Replication
     ↓
Destination Bucket
```

---

## 8. What Can Be Replicated?

S3 replication can replicate objects and associated metadata depending on the configuration.

Replication can include:

* Object data
* Metadata
* Object tags
* Access control information where applicable
* Object versions

The exact behavior depends on the replication configuration and encryption settings.

---

## 9. Replication Rules

Replication is configured using **replication rules**.

A rule can apply to:

* All objects
* Objects with a specific prefix
* Objects with specific tags

Example:

```text id="m7x2q4"
Replication Rule
      │
      ├── Prefix: critical-data/
      │
      └── Destination:
             backup-bucket
```

Only objects matching the rule are replicated.

---

## 10. Prefix-Based Replication

Suppose the bucket contains:

```text id="p4n8c2"
my-source-bucket/
│
├── critical/
│   ├── database.zip
│   └── backup.zip
│
├── images/
│   ├── logo.png
│   └── photo.jpg
│
└── temporary/
    └── file.txt
```

A replication rule can target:

```text id="h3m7x9"
critical/
```

Then:

```text id="w6q2p5"
critical/database.zip
critical/backup.zip
```

can be replicated while other prefixes are excluded.

---

## 11. Tag-Based Replication

Replication rules can also use object tags.

Example:

```text id="a8r4m6"
Tag:
Environment = production
```

A replication rule can be configured to replicate objects matching that tag.

This is useful when objects are organized by metadata rather than prefixes.

---

## 12. CRR for Disaster Recovery

CRR is commonly used for disaster recovery.

Example:

```text id="j5k9x2"
              Primary Region
              Mumbai
                  │
                  ↓
             Source Bucket
                  │
                  │ CRR
                  ↓
             Backup Region
              Singapore
                  │
                  ↓
          Destination Bucket
```

If the primary Region experiences a major issue, replicated data exists in another Region.

> Replication improves data availability and recovery options, but a complete disaster-recovery strategy also requires planning for applications, databases, DNS, security, and recovery procedures.

---

## 13. SRR for Data Separation

Same-Region Replication can be useful when data needs to be copied between buckets in the same Region.

Example:

```text id="k4m8p1"
Mumbai Region
     │
     ├── Production Bucket
     │
     │ SRR
     ↓
     └── Backup / Compliance Bucket
```

---

## 14. Cross-Account Replication

S3 replication can also copy objects between buckets owned by different AWS accounts.

Example:

```text id="r7x3m5"
AWS Account A
Source Bucket
      │
      │ CRR / SRR
      ↓
AWS Account B
Destination Bucket
```

This can provide additional isolation between production and backup environments.

---

## 15. Replication and Encryption

If objects use encryption, replication must be configured appropriately for the encryption type.

For example, when using **SSE-KMS**, the replication configuration needs the required permissions to use the relevant KMS keys.

Conceptually:

```text id="n2q8v4"
Source Object
     ↓
SSE-KMS
     ↓
Replication IAM Role
     ↓
Destination KMS Key
     ↓
Destination Object
```

The required permissions must be correctly configured.

---

## 16. Replication Status

S3 can provide replication status information.

Objects can have statuses such as:

```text id="z5m1x8"
PENDING
COMPLETED
FAILED
```

This helps determine whether replication has completed successfully.

---

## 17. Replication Time Control (RTC)

**S3 Replication Time Control (RTC)** is an optional feature designed for workloads that need predictable replication timing.

It is useful when organizations have strict requirements around how quickly objects should be replicated.

Example:

```text id="v8q4m2"
Source Object
     ↓
Replication
     ↓
Destination
     ↓
Predictable replication target
```

RTC is a paid feature and should be considered when replication timing requirements justify the additional cost.

---

## 18. Delete Marker Replication

S3 replication can be configured to replicate certain delete operations.

For example:

```text id="x6n3p9"
Source Bucket
     │
     ├── Object
     │
     └── Delete Marker
              │
              ↓
       Destination Bucket
```

Delete replication behavior depends on the replication configuration.

It should be planned carefully for disaster-recovery environments because accidentally deleting data in the source should not automatically undermine your recovery strategy.

---

## 19. Existing Objects

Replication is primarily designed to replicate objects according to configured replication rules.

When setting up replication, consider whether you also need to replicate **existing objects** that were already present before the rule was created.

AWS provides mechanisms such as **S3 Batch Replication** for replicating existing objects.

```text id="m9k4x7"
Existing Objects
      ↓
S3 Batch Replication
      ↓
Destination Bucket
```

---

## 20. Replication vs Backup

Replication and backup are related but not identical.

### Replication

```text id="c8v2m6"
Source
  ↓
Automatic Copy
  ↓
Destination
```

### Backup

A backup strategy may involve:

* Point-in-time recovery
* Retention policies
* Separate storage
* Separate accounts
* Recovery testing

### Important

> Replication alone should not automatically be treated as a complete backup strategy.

For example, if a malicious or accidental deletion is replicated, the destination may also be affected depending on the configuration.

---

## 21. Replication vs Versioning

These features have different purposes.

| Feature     | Purpose                             |
| ----------- | ----------------------------------- |
| Versioning  | Keeps multiple versions in a bucket |
| Replication | Copies objects to another bucket    |

They often work together:

```text id="y7m3q8"
Source Bucket
     │
     ├── Versioning
     │
     └── Replication
             ↓
      Destination Bucket
```

---

## 22. Replication vs Lifecycle

| Feature     | Purpose             |
| ----------- | ------------------- |
| Replication | Copy data           |
| Lifecycle   | Move or delete data |

Example:

```text id="p3x8n5"
Replication
→ Source → Destination

Lifecycle
→ Standard → IA → Glacier → Delete
```

---

## 23. Real-World Example

A company stores application backups in Mumbai.

Requirement:

* Primary backup storage in Mumbai
* Disaster-recovery copy in another Region
* Objects encrypted
* Old data moved to cheaper storage
* Data protected against accidental deletion

Architecture:

```text id="n6r2x9"
                    AWS Account
                         │
                         ↓
                  Mumbai Region
                         │
                         ↓
                Source S3 Bucket
                  │       │
                  │       └── Versioning
                  │
                  │ CRR
                  ↓
             Singapore Region
                  │
                  ↓
           Destination Bucket
                  │
                  └── Versioning
```

Additional controls:

```text id="a4m8q1"
Encryption
Lifecycle Rules
IAM
Bucket Policies
Block Public Access
Monitoring
```

---

## 24. Practical Configuration Flow

```text id="x7p3m5"
Create Source Bucket
        ↓
Enable Versioning
        ↓
Create Destination Bucket
        ↓
Enable Versioning
        ↓
Configure IAM Replication Role
        ↓
Create Replication Rule
        ↓
Select Destination Bucket
        ↓
Configure Encryption if Required
        ↓
Save Rule
        ↓
Upload Test Object
        ↓
Verify Destination
```

---

## 25. Things to Check When Replication Fails

If an object is not replicated, check:

### 1. Versioning

```text id="m5x9q2"
Source → Versioning Enabled?
Destination → Versioning Enabled?
```

### 2. IAM Role

Does the replication role have the required permissions?

### 3. KMS Permissions

If SSE-KMS is used, verify KMS permissions.

### 4. Replication Rule

Check:

* Prefix
* Tags
* Destination
* Rule status

### 5. Object Eligibility

Check whether the object matches the replication rule.

### 6. Replication Status

Check whether the object is:

```text id="r8n3v6"
PENDING
COMPLETED
FAILED
```

---

## 26. Quick Revision

```text id="k2m7x4"
S3 Replication
→ Automatically copies objects between buckets

CRR
→ Cross-Region Replication

SRR
→ Same-Region Replication

Versioning
→ Required for S3 replication configurations

Replication Role
→ IAM role used by S3 for replication

Replication Rule
→ Defines which objects are replicated

CRR
→ Useful for disaster recovery and geographic redundancy

SRR
→ Useful for same-Region separation and compliance

S3 Batch Replication
→ Helps replicate existing objects

RTC
→ Helps provide predictable replication timing
```

---

## 27. Key Takeaways

> **S3 Replication automatically copies objects from a source bucket to a destination bucket.**

Remember:

```text id="b6q9m3"
CRR
→ Region A → Region B

SRR
→ Region A → Same Region

Versioning
→ Source + Destination

IAM Role
→ Allows S3 to replicate

Replication Rule
→ Decides what gets replicated
```

### Cloud Engineer Mindset

When designing S3 replication, always ask:

```text id="q4x8m2"
What data?
     ↓
Where should it go?
     ↓
Same Region or another Region?
     ↓
What permissions are required?
     ↓
Is encryption required?
     ↓
What happens if data is deleted?
     ↓
How will recovery be performed?
```

---

