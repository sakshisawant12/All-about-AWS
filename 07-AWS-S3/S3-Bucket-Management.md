# Amazon S3 – Bucket Management

## 1. What is an S3 Bucket?

An **S3 bucket** is a container used to store objects in Amazon S3.

```text
AWS Account
    │
    └── Amazon S3
          │
          └── Bucket
               │
               ├── Object
               ├── Object
               └── Object
```

A bucket can contain a very large number of objects.

---

## 2. Creating an S3 Bucket

When creating a bucket, important settings include:

* Bucket name
* AWS Region
* Object Ownership
* Block Public Access
* Bucket Versioning
* Default Encryption

### Basic Flow

```text
S3 Console
   ↓
Create Bucket
   ↓
Choose Bucket Name
   ↓
Choose Region
   ↓
Configure Settings
   ↓
Create Bucket
```

---

## 3. Bucket Naming Rules

S3 bucket names must be **globally unique** within the AWS partition.

Example:

```text
my-cloud-project-2026
```

Good naming practices:

* Use lowercase letters
* Use numbers when required
* Use hyphens for readability
* Keep names meaningful
* Avoid sensitive information in bucket names

Example:

```text
aws-cloud-project-backup
company-static-website
application-log-storage
```

### Important

This will usually fail if another AWS customer already owns the name:

```text
mybucket
```

Instead, use a more unique name:

```text
mybucket-2026-cloud
```

---

## 4. Bucket Region

Every S3 bucket is created in a specific AWS Region.

Example:

```text
Bucket
   ↓
ap-south-1
   ↓
Mumbai
```

Choosing a Region can affect:

* Network latency
* Data residency
* Compliance
* Service integration
* Data transfer costs

---

## 5. Objects in a Bucket

An **object** is the actual data stored inside the bucket.

Examples:

```text
resume.pdf
image.jpg
backup.zip
index.html
server.log
```

Example:

```text
my-bucket
│
├── resume.pdf
├── image.jpg
├── backup.zip
└── index.html
```

---

## 6. Object Key

Each object has a unique **key** within its bucket.

Example:

```text
my-bucket
└── documents/resume.pdf
```

Here:

```text
Bucket = my-bucket

Key = documents/resume.pdf
```

The key identifies the object.

---

## 7. Prefixes and Folders

S3 uses a **flat object storage structure**.

It does not require a traditional filesystem hierarchy.

However, prefixes can make objects appear organized into folders.

Example:

```text
documents/resume.pdf
documents/certificate.pdf
images/profile.jpg
images/logo.png
backups/database.zip
```

The prefixes are:

```text
documents/
images/
backups/
```

### Important

> S3 does not use traditional folders like a Linux filesystem. What looks like a folder is based on object key prefixes.

---

## 8. Uploading an Object

Objects can be uploaded through:

* AWS Management Console
* AWS CLI
* AWS SDK
* AWS APIs

### Console Flow

```text
S3
 ↓
Select Bucket
 ↓
Upload
 ↓
Add Files
 ↓
Choose File
 ↓
Upload
```

---

## 9. Downloading an Object

To download an object:

```text
S3
 ↓
Bucket
 ↓
Select Object
 ↓
Download
```

The object is downloaded to your local system.

---

## 10. Deleting an Object

An object can be deleted from a bucket.

Example:

```text
Bucket
│
├── image.jpg
├── resume.pdf
└── backup.zip

Delete resume.pdf

Bucket
│
├── image.jpg
└── backup.zip
```

### Important

If **Versioning is enabled**, deleting an object does not necessarily permanently remove all versions.

A delete marker may be created instead.

Versioning is covered separately in:

```text
S3-Versioning.md
```

---

## 11. Object Properties

An S3 object can have several properties.

Important ones include:

* Key
* Size
* Last modified date
* Storage class
* Encryption
* Metadata
* Version ID when versioning is enabled

Example:

```text
Object
│
├── Key: images/photo.jpg
├── Size: 2 MB
├── Storage Class: S3 Standard
├── Encryption: SSE-S3
└── Last Modified: Date/Time
```

---

## 12. Object Metadata

**Metadata** is information about an object.

Examples:

```text
Content-Type
Content-Length
Last-Modified
Encryption information
Custom metadata
```

For example, an image might have:

```text
Content-Type: image/jpeg
```

A PDF might have:

```text
Content-Type: application/pdf
```

---

## 13. Bucket-Level Settings

Important bucket settings include:

### Versioning

Keeps multiple versions of objects.

```text
Version 1
   ↓
Version 2
   ↓
Version 3
```

---

### Encryption

Protects objects stored in the bucket.

Common options include:

* SSE-S3
* SSE-KMS
* DSSE-KMS
* SSE-C

---

### Block Public Access

Helps prevent unintended public access to S3 data.

For most private buckets:

```text
Block Public Access
        ↓
Enabled
```

---

### Lifecycle Rules

Automatically perform actions based on object age or other conditions.

Example:

```text
30 days
   ↓
Move to Standard-IA

90 days
   ↓
Move to Glacier

365 days
   ↓
Delete
```

---

### Replication

Automatically copies objects to another bucket.

Types include:

* Cross-Region Replication
* Same-Region Replication

---

## 14. Bucket Policy

A **bucket policy** is a resource-based policy attached to an S3 bucket.

It controls who can perform actions on the bucket and its objects.

Example actions:

```text
s3:GetObject
s3:PutObject
s3:DeleteObject
s3:ListBucket
```

Example concept:

```text
User/Application
       │
       ↓
Bucket Policy
       │
       ↓
Allow / Deny
       │
       ↓
S3 Bucket
```

Bucket policies will be covered in more detail in:

```text
S3-Security.md
```

---

## 15. Object Ownership

S3 provides **Object Ownership** settings that help determine who owns objects uploaded to a bucket.

The commonly used setting is:

```text
Bucket owner enforced
```

With Bucket owner enforced, ACLs are disabled and the bucket owner owns the objects in the bucket.

This simplifies access management.

---

## 16. S3 Bucket ARN

Every S3 bucket has an **ARN (Amazon Resource Name)**.

Example:

```text
arn:aws:s3:::my-cloud-project
```

For an object:

```text
arn:aws:s3:::my-cloud-project/images/photo.jpg
```

### Structure

Bucket:

```text
arn:aws:s3:::bucket-name
```

Object:

```text
arn:aws:s3:::bucket-name/object-key
```

ARNs are commonly used in IAM and bucket policies.

---

## 17. S3 URI

An S3 URI can be used to identify an S3 location.

Example:

```text
s3://my-cloud-project/images/photo.jpg
```

Structure:

```text
s3://bucket-name/object-key
```

Example:

```text
s3://my-cloud-project/documents/resume.pdf
```

---

## 18. S3 Website URL vs S3 URI

These are different.

### S3 URI

Used to identify an S3 object:

```text
s3://my-bucket/index.html
```

### HTTP/HTTPS URL

Used to access an object through HTTP when the required access is configured:

```text
https://...
```

Do not confuse:

```text
s3://
```

with:

```text
https://
```

---

## 19. Basic AWS CLI Commands

### List Buckets

```bash
aws s3 ls
```

---

### List Objects in a Bucket

```bash
aws s3 ls s3://my-bucket
```

---

### Create a Bucket

For a Region-specific bucket, use the appropriate AWS CLI options for that Region.

Example for `ap-south-1`:

```bash
aws s3api create-bucket \
  --bucket my-cloud-project-2026 \
  --region ap-south-1 \
  --create-bucket-configuration LocationConstraint=ap-south-1
```

---

### Upload a File

```bash
aws s3 cp file.txt s3://my-bucket/
```

---

### Download a File

```bash
aws s3 cp s3://my-bucket/file.txt .
```

---

### Upload a Directory

```bash
aws s3 cp my-folder/ s3://my-bucket/my-folder/ --recursive
```

---

### Delete an Object

```bash
aws s3 rm s3://my-bucket/file.txt
```

---

### Delete All Objects in a Prefix

```bash
aws s3 rm s3://my-bucket/my-folder/ --recursive
```

> Be careful with `--recursive` because it can affect multiple objects.

---

## 20. `aws s3` vs `aws s3api`

The AWS CLI provides two common ways to work with S3.

### High-Level Commands

```bash
aws s3
```

Examples:

```bash
aws s3 ls
aws s3 cp
aws s3 sync
aws s3 rm
```

These are easier for common operations.

### Low-Level API Commands

```bash
aws s3api
```

Examples:

```bash
aws s3api create-bucket
aws s3api get-bucket-versioning
aws s3api put-bucket-versioning
```

These provide more detailed control over S3 APIs and configuration.

---

## 21. S3 Bucket Management Flow

```text
Create Bucket
      ↓
Choose Region
      ↓
Configure Security
      ↓
Configure Encryption
      ↓
Enable Versioning if Required
      ↓
Upload Objects
      ↓
Manage Objects
      ↓
Configure Lifecycle / Replication
```

---

## 22. Real-World Example

Suppose a company has an application that stores customer documents.

```text
Application
     │
     │ Upload
     ↓
S3 Bucket
     │
     ├── customers/
     │     ├── customer1/
     │     │     ├── id.pdf
     │     │     └── document.pdf
     │     │
     │     └── customer2/
     │           ├── id.pdf
     │           └── document.pdf
     │
     └── backups/
```

The company can then use:

* IAM for user/application permissions
* Bucket policies for resource-level access
* Encryption for data protection
* Versioning for recovery
* Lifecycle rules for cost optimization

---

## 23. Key Takeaways

```text
Bucket
→ Container for objects

Object
→ Actual stored data

Key
→ Unique identifier of an object

Prefix
→ Folder-like organization

Bucket Policy
→ Resource-based access control

Versioning
→ Keeps multiple object versions

Lifecycle
→ Automates storage transitions/deletion

Encryption
→ Protects stored data

ARN
→ AWS resource identifier

S3 URI
→ s3://bucket/object
```

---
