# Amazon S3 – Lifecycle Management

## 1. What is S3 Lifecycle Management?

**S3 Lifecycle Management** allows you to automatically manage objects throughout their lifecycle.

You can create rules to:

* Move objects to cheaper storage classes
* Delete objects after a certain period
* Manage old object versions
* Remove expired delete markers

The main goal is **cost optimization and automated data management**.

---

## 2. Why Use Lifecycle Rules?

Suppose an application stores logs in S3.

Initially, logs are frequently accessed:

```text id="k8v3mz"
New Logs
   ↓
S3 Standard
```

After some time, they are rarely accessed:

```text id="p2x7nc"
30 Days
   ↓
S3 Standard-IA
```

Later, they become archival data:

```text id="w5m1qr"
90 Days
   ↓
S3 Glacier
```

Eventually, the company no longer needs them:

```text id="t9c4va"
365 Days
   ↓
Delete
```

A lifecycle rule can automate this entire process.

---

## 3. Lifecycle Rule

A **Lifecycle Rule** defines what S3 should do with objects based on conditions such as:

* Object age
* Object prefix
* Object tags
* Object version status

Example:

```text id="x4n8kp"
Objects
   ↓
30 Days
   ↓
Move to Standard-IA
   ↓
90 Days
   ↓
Move to Glacier
   ↓
365 Days
   ↓
Delete
```

---

## 4. Lifecycle Actions

The major lifecycle actions are:

### Transition

Move objects to another storage class.

Example:

```text id="m6r2vz"
S3 Standard
     ↓
S3 Standard-IA
     ↓
S3 Glacier
```

### Expiration

Delete objects after a specified period.

Example:

```text id="q7w3bn"
Object
  ↓
365 Days
  ↓
Expiration
  ↓
Delete
```

### Noncurrent Version Management

When versioning is enabled, lifecycle rules can manage older versions.

Example:

```text id="n5x8jc"
Current Version
      ↓
Becomes Noncurrent
      ↓
30 Days
      ↓
Transition / Expire
```

---

## 5. Lifecycle Rule Components

A lifecycle rule generally contains:

```text id="a9m4kp"
Lifecycle Rule
│
├── Scope
│    ├── Entire Bucket
│    ├── Prefix
│    └── Object Tags
│
└── Actions
     ├── Transition
     └── Expiration
```

---

## 6. Lifecycle Rule Scope

A lifecycle rule can apply to:

### Entire Bucket

The rule applies to all objects.

```text id="q2v6nm"
Bucket
│
├── file1
├── file2
├── file3
└── file4

Lifecycle Rule
      ↓
All Objects
```

---

### Prefix

The rule can target objects with a particular prefix.

Example:

```text id="h8p3wr"
logs/
```

Objects:

```text id="u5m7xc"
logs/app.log
logs/server.log
logs/database.log
images/photo.jpg
```

The lifecycle rule can target:

```text id="z4k9nb"
logs/
```

Only objects under that prefix are affected.

---

### Object Tags

Lifecycle rules can also target objects using tags.

Example:

```text id="v3n6qp"
Environment = archive
```

Only objects with the matching tag can be selected by the rule.

---

## 7. Transition Actions

A transition moves objects to a different storage class.

Example:

```text id="r8m2vx"
Day 0
  ↓
S3 Standard
  ↓
Day 30
  ↓
S3 Standard-IA
  ↓
Day 90
  ↓
S3 Glacier Flexible Retrieval
```

This can reduce storage costs when objects become less frequently accessed.

---

## 8. Expiration Actions

Expiration automatically removes objects after a defined period.

Example:

```text id="c6q4yz"
Application Logs
       ↓
180 Days
       ↓
Expiration
       ↓
Object Deleted
```

This is useful when data only needs to be retained for a certain period.

---

## 9. Current vs Noncurrent Versions

When versioning is enabled, an object can have:

```text id="w7n3kx"
Current Version
       │
       └── Latest version

Noncurrent Versions
       │
       ├── Old Version 1
       ├── Old Version 2
       └── Old Version 3
```

Lifecycle rules can separately manage noncurrent versions.

---

## 10. Noncurrent Version Expiration

Suppose:

```text id="m2c8vp"
Version 1
Version 2
Version 3 ← Current
```

After Version 4 is uploaded:

```text id="q9x4nd"
Version 1 → Noncurrent
Version 2 → Noncurrent
Version 3 → Noncurrent
Version 4 → Current
```

A lifecycle rule can automatically expire old noncurrent versions.

Example:

```text id="f6k2yr"
Noncurrent Version
       ↓
90 Days
       ↓
Expire
```

This helps prevent old versions from accumulating storage costs.

---

## 11. Delete Markers

With versioning enabled, deleting an object normally creates a **delete marker**.

Lifecycle rules can help manage expired delete markers.

Example:

```text id="b8m5qw"
Object
  ↓
Delete
  ↓
Delete Marker
  ↓
Lifecycle Rule
  ↓
Remove expired delete marker
```

---

## 12. Example – Log Management

Suppose an application stores logs in:

```text id="r4n7mx"
logs/
├── application/
├── server/
└── database/
```

The company needs:

* Recent logs → Frequently accessed
* Older logs → Cheaper storage
* Very old logs → Archive
* Logs older than one year → Delete

Lifecycle rule:

```text id="y6c2pk"
Day 0
 ↓
S3 Standard
 ↓
30 Days
 ↓
S3 Standard-IA
 ↓
90 Days
 ↓
S3 Glacier Flexible Retrieval
 ↓
365 Days
 ↓
Delete
```

This reduces manual management and storage costs.

---

## 13. Example – Backup Management

Suppose backups are stored in:

```text id="p5v8xq"
backups/
```

Lifecycle policy:

```text id="s3m7kn"
Day 0
 ↓
S3 Standard
 ↓
30 Days
 ↓
Standard-IA
 ↓
90 Days
 ↓
Glacier
 ↓
1 Year
 ↓
Delete
```

This is useful when old backups are rarely accessed.

---

## 14. Lifecycle Rules and Storage Classes

Lifecycle management works together with S3 storage classes.

```text id="z8n4vc"
S3 Standard
     ↓
Lifecycle Rule
     ↓
Standard-IA
     ↓
Lifecycle Rule
     ↓
Glacier
     ↓
Lifecycle Rule
     ↓
Expiration
```

This allows storage to match the changing value and access frequency of data.

---

## 15. Important Storage Transition Consideration

Do not blindly move every object to a cheaper storage class.

Before creating a lifecycle rule, consider:

* Access frequency
* Minimum storage duration requirements
* Retrieval charges
* Transition costs
* Data retention requirements
* Recovery requirements

The cheapest storage class is not always the cheapest **overall** option.

---

## 16. Lifecycle Rule Example

A practical rule might be:

```text id="k3x7vp"
Rule Name:
Archive Application Logs

Filter:
logs/

Actions:

After 30 days
→ Transition to Standard-IA

After 90 days
→ Transition to Glacier Flexible Retrieval

After 365 days
→ Expire objects
```

---

## 17. Creating a Lifecycle Rule

### AWS Console Flow

```text id="n6q2zr"
S3
 ↓
Select Bucket
 ↓
Management
 ↓
Lifecycle Rules
 ↓
Create Lifecycle Rule
 ↓
Choose Scope
 ↓
Choose Transitions
 ↓
Configure Expiration
 ↓
Create Rule
```

---

## 18. AWS CLI

Lifecycle configuration can also be managed using the AWS CLI.

Example command:

```bash id="c8m4yx"
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-bucket \
  --lifecycle-configuration file://lifecycle.json
```

Example `lifecycle.json`:

```json id="w4q9mz"
{
  "Rules": [
    {
      "ID": "ArchiveLogs",
      "Status": "Enabled",
      "Filter": {
        "Prefix": "logs/"
      },
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        },
        {
          "Days": 90,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```

> Always verify current AWS-supported transition and expiration constraints before applying a production lifecycle configuration.

---

## 19. Viewing Lifecycle Configuration

You can retrieve a bucket's lifecycle configuration using:

```bash id="v7p2kn"
aws s3api get-bucket-lifecycle-configuration \
  --bucket my-bucket
```

---

## 20. Removing Lifecycle Configuration

To remove a lifecycle configuration:

```bash id="x5m8qr"
aws s3api delete-bucket-lifecycle \
  --bucket my-bucket
```

Use this carefully because removing a lifecycle configuration changes the bucket's automated object-management behavior.

---

## 21. Lifecycle Management Architecture

```text id="n4v7cx"
                  S3 Bucket
                     │
                     ↓
              Lifecycle Rule
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
      Transition  Expiration  Versions
          │          │          │
          ↓          ↓          ↓
   Cheaper Class   Delete   Manage Old
                            Versions
```

---

## 22. Lifecycle vs Versioning

These two features are different.

| Feature    | Purpose                            |
| ---------- | ---------------------------------- |
| Versioning | Keeps multiple versions            |
| Lifecycle  | Automates transitions and deletion |

They can work together:

```text id="q8m3vx"
Versioning
    ↓
Keeps Old Versions
    ↓
Lifecycle Rule
    ↓
Manages Old Versions
```

---

## 23. Lifecycle vs Replication

| Feature     | Purpose                              |
| ----------- | ------------------------------------ |
| Lifecycle   | Manages object storage and retention |
| Replication | Copies objects to another bucket     |

Example:

```text id="y2n6kc"
Lifecycle
→ Move / Delete

Replication
→ Copy
```

---

## 24. Real-World Cloud Engineer Scenario

A company runs an application on EC2.

The application generates logs:

```text id="b5r8qm"
EC2
 ↓
Application Logs
 ↓
S3
```

The company wants:

* Recent logs available quickly
* Older logs stored cheaply
* Very old logs archived
* Logs automatically deleted after retention period

Solution:

```text id="h7x3np"
EC2
 ↓
S3
 ↓
Lifecycle Rule
 │
 ├── 30 Days → Standard-IA
 │
 ├── 90 Days → Glacier
 │
 └── 365 Days → Delete
```

This is a common cloud cost-optimization pattern.

---

## 25. Best Practices

### 1. Define a retention policy

Know how long data needs to be stored before creating expiration rules.

### 2. Use lifecycle rules for old data

Avoid manually moving thousands of objects.

### 3. Combine lifecycle with versioning

Manage old versions to prevent unnecessary storage growth.

### 4. Use prefixes or tags

Apply different rules to different types of data.

Example:

```text id="x6c9vk"
logs/
backups/
temporary/
archive/
```

### 5. Consider retrieval costs

Moving data to cheaper storage can increase retrieval costs.

### 6. Test rules carefully

A poorly designed expiration rule can delete data that is still required.

---

## 26. Quick Revision

```text id="p3x7mq"
Lifecycle Management
→ Automates object management

Transition
→ Moves object to another storage class

Expiration
→ Deletes object after a defined period

Prefix
→ Applies rule to matching object keys

Tags
→ Can be used to filter objects

Noncurrent Versions
→ Old versions of versioned objects

Delete Marker
→ Marker created by a delete in a versioned bucket
```

---

## 27. Key Takeaways

> **S3 Lifecycle Management automatically moves and deletes objects based on defined rules.**

Remember:

```text id="m8q2vx"
New Data
   ↓
S3 Standard
   ↓
Less Frequently Accessed
   ↓
Standard-IA
   ↓
Archive
   ↓
Glacier
   ↓
Retention Period Ends
   ↓
Delete
```

