# Amazon S3 – Versioning

## 1. What is S3 Versioning?

**S3 Versioning** is a feature that keeps multiple versions of an object in the same S3 bucket.

It helps protect data from:

* Accidental deletion
* Accidental overwriting
* Unwanted changes

Example:

```text id="xj1wq6"
document.txt
     │
     ├── Version 1
     ├── Version 2
     └── Version 3
```

Instead of permanently replacing the old object, S3 can preserve previous versions.

---

## 2. Why Use Versioning?

Without versioning:

```text id="6k8x3e"
Version 1
   ↓
Upload Version 2
   ↓
Version 1 is replaced
```

With versioning:

```text id="7q3m8a"
Version 1
   ↓
Upload Version 2
   ↓
Version 1 is preserved
   ↓
Version 2 becomes current
```

This makes it possible to recover from accidental changes.

---

## 3. Versioning States

An S3 bucket can have three versioning states:

* **Unversioned**
* **Enabled**
* **Suspended**

### Unversioned

Versioning has not been enabled.

```text id="j9q2xk"
Object
  ↓
Single current version
```

### Enabled

New objects and changes are versioned.

```text id="d8k2qp"
Object
  ├── Version 1
  ├── Version 2
  └── Version 3
```

### Suspended

Versioning is stopped for new versions, but existing versions are retained.

> Suspending versioning does **not** delete previously stored versions.

---

## 4. Enabling Versioning

### AWS Console

Basic flow:

```text id="j7c5a9"
S3
 ↓
Select Bucket
 ↓
Properties
 ↓
Bucket Versioning
 ↓
Enable
```

---

## 5. AWS CLI – Enable Versioning

Use:

```bash id="0f4q8e"
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled
```

Check the current status:

```bash id="5a1w7k"
aws s3api get-bucket-versioning \
  --bucket my-bucket
```

Example output:

```json id="w8d2pl"
{
    "Status": "Enabled"
}
```

---

## 6. How Versioning Works

Suppose we upload:

```text id="h3j6y9"
report.txt
```

Initially:

```text id="x1p7va"
report.txt
└── Version 1
```

We modify and upload it again:

```text id="f8r4kc"
report.txt
├── Version 1
└── Version 2
```

Upload it again:

```text id="s7n2md"
report.txt
├── Version 1
├── Version 2
└── Version 3
```

The newest version becomes the **current version**.

---

## 7. Version ID

Every version of an object receives a unique **Version ID** when versioning is enabled.

Example:

```text id="0g3m5z"
report.txt
│
├── Version ID: abc123
├── Version ID: xyz456
└── Version ID: pqr789
```

The Version ID allows S3 to distinguish between different versions of the same object.

---

## 8. Current Version

The **current version** is the version that S3 treats as the latest version of the object.

Example:

```text id="k9x3as"
report.txt
├── Version 1
├── Version 2
└── Version 3 ← Current Version
```

If Version 4 is uploaded:

```text id="4r6jpd"
report.txt
├── Version 1
├── Version 2
├── Version 3
└── Version 4 ← Current Version
```

---

## 9. Overwriting an Object

Suppose the bucket contains:

```text id="s8k4qp"
website.html
└── Version 1
```

You upload a new `website.html`.

With versioning enabled:

```text id="q3m8vz"
website.html
├── Version 1
└── Version 2 ← Current
```

The previous version remains available.

---

## 10. Deleting an Object

This is one of the most important S3 Versioning concepts.

When versioning is enabled and you normally delete an object:

```text id="9x3w7p"
aws s3 rm s3://my-bucket/file.txt
```

S3 generally creates a **delete marker** instead of permanently deleting the existing version.

---

## 11. Delete Marker

A **delete marker** is a special version that tells S3 that the object has been deleted.

Example:

```text id="1a8r4m"
file.txt
├── Version 1
├── Version 2
└── Delete Marker ← Current
```

When a normal request is made for:

```text id="8r2c6v"
file.txt
```

S3 treats the object as deleted because the delete marker is the current version.

However, the previous versions still exist.

---

## 12. Recovering a Deleted Object

Because the previous versions are retained, you can recover an object by removing the delete marker.

Conceptually:

```text id="5v7k2q"
Version 1
Version 2
Delete Marker
     ↓
Remove Delete Marker
     ↓
Version 2 becomes current
```

The object becomes accessible again.

---

## 13. Permanent Deletion

To permanently delete a specific version, you must specify its **Version ID**.

Example:

```bash id="1c6m8q"
aws s3api delete-object \
  --bucket my-bucket \
  --key file.txt \
  --version-id VERSION_ID
```

This permanently removes that specific version.

> Be careful when permanently deleting versioned data because it cannot be recovered through S3 versioning after deletion.

---

## 14. Listing Object Versions

You can list versions using:

```bash id="p7k3x1"
aws s3api list-object-versions \
  --bucket my-bucket
```

This can show:

* Object versions
* Version IDs
* Delete markers
* Last modified timestamps

---

## 15. Versioning and Storage Costs

Versioning can increase storage costs.

Example:

```text id="6m4q9a"
file.txt – 10 MB
      ↓
Updated
      ↓
10 MB old version + 10 MB new version
      ↓
20 MB total storage
```

If the object is updated many times, many versions can accumulate.

Therefore, version management should be combined with **S3 Lifecycle rules** when appropriate.

---

## 16. Versioning + Lifecycle Management

Lifecycle rules can help manage older object versions.

Example:

```text id="4y7n2k"
Current Version
      ↓
Older Version
      ↓
30 days
      ↓
Transition / Expire
```

You can configure lifecycle rules to manage:

* Current object versions
* Noncurrent object versions
* Delete markers

This helps control storage costs.

---

## 17. Versioning for Backup Protection

Versioning can provide an additional layer of protection against accidental changes.

Example:

```text id="w3r8x5"
Application
    ↓
S3 Bucket
    ↓
Versioning Enabled
    ↓
Old versions retained
```

If an application accidentally overwrites a file, an older version can still be recovered.

---

## 18. Versioning Does NOT Replace Backups

Versioning is useful, but it should not automatically be considered a complete backup strategy.

Why?

Because versioned objects are still stored within the same S3 bucket.

For stronger protection, organizations may also use:

* S3 Replication
* Cross-account backup strategies
* Separate backup storage
* AWS Backup where appropriate

---

## 19. Versioning and Replication

Versioning is commonly required for S3 replication configurations.

Example:

```text id="9w5r2x"
Source Bucket
     │
     │ Versioning
     ↓
S3 Replication
     │
     ↓
Destination Bucket
     │
     │ Versioning
     ↓
Replicated Objects
```

This can provide additional protection and geographic separation.

---

## 20. MFA Delete

**MFA Delete** is a feature that can require multi-factor authentication for certain version-management operations.

It can provide additional protection against accidental or unauthorized deletion of object versions.

Important:

* It is related to S3 Versioning.
* It is not the same as simply enabling MFA on an IAM user.
* It has specific configuration and operational requirements.

For most basic S3 learning, remember the concept rather than focusing on implementation details.

---

## 21. Versioning Example

Imagine an important configuration file:

```text id="q8n4mx"
config.json
```

### Day 1

```text id="4h7y2p"
Version 1
```

### Day 2

Someone updates the file:

```text id="9m3k6a"
Version 1
Version 2 ← Current
```

### Day 3

Someone accidentally deletes it:

```text id="0v6p1s"
Version 1
Version 2
Delete Marker ← Current
```

The application sees the object as deleted.

But the old versions are still stored.

Remove the delete marker:

```text id="x7c2m9"
Version 1
Version 2 ← Current
```

The object is restored.

---

## 22. Versioning Flow

```text id="g8w4p2"
Create Bucket
      ↓
Enable Versioning
      ↓
Upload Object
      ↓
Version 1
      ↓
Modify Object
      ↓
Version 2
      ↓
Delete Object
      ↓
Delete Marker
      ↓
Remove Delete Marker
      ↓
Previous Version Restored
```

---

## 23. Important CLI Commands

### Enable Versioning

```bash id="r6m1x4"
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled
```

### Check Versioning

```bash id="z2q8k5"
aws s3api get-bucket-versioning \
  --bucket my-bucket
```

### List Versions

```bash id="b7n3p9"
aws s3api list-object-versions \
  --bucket my-bucket
```

### Delete a Specific Version

```bash id="c5w9m2"
aws s3api delete-object \
  --bucket my-bucket \
  --key file.txt \
  --version-id VERSION_ID
```

---

## 24. Common Interview Questions

### Q1. What is S3 Versioning?

S3 Versioning keeps multiple versions of objects in a bucket to protect against accidental deletion and overwriting.

### Q2. What happens when you delete a versioned object?

A delete marker is normally created instead of immediately deleting the existing object version.

### Q3. Can you recover a deleted versioned object?

Yes. If the previous version still exists, removing the delete marker can restore the object.

### Q4. Does suspending versioning delete existing versions?

No. Existing versions remain in the bucket.

### Q5. Does versioning increase storage costs?

Yes. Multiple versions of objects consume storage.

### Q6. Does versioning replace backups?

No. Versioning is a recovery feature, not a complete backup strategy.

---

## 25. Quick Revision

```text id="y4m8q2"
Versioning
→ Keeps multiple object versions

Version ID
→ Unique identifier for an object version

Current Version
→ Latest version

Delete Marker
→ Marks the object as deleted

Suspended
→ Stops creating new versions but keeps existing versions

Versioning + Lifecycle
→ Helps manage old versions and storage costs

Versioning + Replication
→ Provides stronger data protection
```

---

## 26. Key Takeaways

> **S3 Versioning protects objects from accidental overwrites and deletions by preserving previous versions.**

Remember:

```text id="8r6x2n"
Upload
  ↓
Version 1
  ↓
Update
  ↓
Version 2
  ↓
Delete
  ↓
Delete Marker
  ↓
Remove Delete Marker
  ↓
Object Restored
```

---

