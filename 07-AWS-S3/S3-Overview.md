# Amazon S3 – Overview

## 1. What is Amazon S3?

**Amazon S3 (Simple Storage Service)** is an AWS **object storage service** used to store and retrieve large amounts of data from anywhere over the internet.

S3 is commonly used for:

* Storing files, images, videos, documents, backups, and logs
* Application data storage
* Backup and disaster recovery
* Static website hosting
* Data lakes
* Storing application artifacts
* Sharing files using presigned URLs

---

## 2. Key Characteristics of S3

* **Object storage** service
* Highly scalable
* High durability
* Accessible over the internet
* Supports different storage classes
* Provides security through IAM, bucket policies, and encryption
* Supports versioning
* Supports lifecycle management
* Supports replication
* Can host static websites

---

## 3. S3 Basic Architecture

The basic structure of S3 is:

```text
AWS Account
     │
     └── S3
          │
          ├── Bucket
          │    │
          │    ├── Object
          │    ├── Object
          │    └── Object
          │
          └── Bucket
               │
               ├── Object
               └── Object
```

---

## 4. Important S3 Terminology

### Bucket

A **bucket** is a container used to store objects in S3.

Example:

```text
my-cloud-project-bucket
```

A bucket has:

* A globally unique name
* An AWS Region
* Security settings
* Storage objects

> Bucket names must be globally unique across AWS.

---

### Object

An **object** is the actual data stored inside an S3 bucket.

Examples:

```text
resume.pdf
photo.jpg
website.html
backup.zip
server.log
```

An object consists mainly of:

* Object data
* Object key
* Metadata

---

### Object Key

The **key** is the name used to identify an object inside a bucket.

Example:

```text
images/profile.jpg
```

Here:

```text
Bucket → my-bucket
Key    → images/profile.jpg
```

S3 does not actually use traditional folders. What looks like a folder is generally a **prefix in the object key**.

Example:

```text
images/profile.jpg
images/logo.png
documents/resume.pdf
```

Here:

```text
images/
documents/
```

are prefixes.

---

## 5. Bucket vs Object

| Feature  | Bucket                   | Object                             |
| -------- | ------------------------ | ---------------------------------- |
| Purpose  | Container                | Actual data                        |
| Example  | `my-bucket`              | `photo.jpg`                        |
| Contains | Objects                  | Data + metadata                    |
| Region   | Yes                      | Stored within a bucket's Region    |
| Security | Bucket policies/settings | Object-level permissions can apply |

Simple way to remember:

> **Bucket = Container**
> **Object = File/Data**

---

## 6. S3 Storage Model

S3 uses **object storage**.

Unlike traditional block storage such as EBS, S3 stores data as objects.

```text
Object
├── Data
├── Key
└── Metadata
```

### Object Storage vs Block Storage

| Feature         | S3                    | EBS                  |
| --------------- | --------------------- | -------------------- |
| Storage type    | Object                | Block                |
| Attached to EC2 | No                    | Yes                  |
| Access          | API/HTTP              | OS filesystem        |
| Common use      | Files, backups, media | OS/application disks |
| Scalability     | Very high             | Volume-based         |

---

## 7. S3 Region

When creating an S3 bucket, you select an AWS Region.

Example:

```text
Bucket
   ↓
ap-south-1
   ↓
Mumbai Region
```

The objects stored in that bucket are stored in that Region according to the selected S3 storage configuration.

Choosing an appropriate Region can help with:

* Lower latency
* Compliance requirements
* Data residency
* Cost optimization

---

## 8. S3 Durability vs Availability

These two terms are important.

### Durability

**Durability** refers to how reliably AWS protects your data from loss.

S3 is designed for extremely high durability.

### Availability

**Availability** refers to how often the service is accessible and usable.

Remember:

```text
Durability → Data should not be lost
Availability → Service should be accessible
```

---

## 9. Common S3 Use Cases

### Backup

Store:

```text
Database backups
Server backups
Application backups
```

### Static Website Hosting

S3 can host static websites containing:

```text
HTML
CSS
JavaScript
Images
```

### Media Storage

Store:

```text
Images
Videos
Audio
Documents
```

### Data Lakes

S3 can be used as a central storage layer for large amounts of structured and unstructured data.

### Log Storage

Applications and AWS services can store logs in S3 for long-term retention.

---

## 10. Important S3 Features

### Versioning

Keeps multiple versions of an object.

Example:

```text
file.txt
   ↓
Version 1
   ↓
Version 2
   ↓
Version 3
```

Useful for recovering accidentally deleted or overwritten objects.

---

### Lifecycle Management

Automatically moves or deletes objects based on rules.

Example:

```text
S3 Standard
     ↓
Standard-IA
     ↓
Glacier
     ↓
Delete
```

This can help reduce storage costs.

---

### Replication

S3 can automatically replicate objects between buckets.

Two important types:

* **CRR – Cross-Region Replication**
* **SRR – Same-Region Replication**

---

### Encryption

S3 supports encryption to protect stored data.

Common options include:

* SSE-S3
* SSE-KMS
* SSE-C
* Client-side encryption

---

### Access Control

S3 access can be controlled using:

* IAM policies
* Bucket policies
* Block Public Access
* Access Points
* Object Ownership

---

## 11. S3 Data Flow

A simple upload flow:

```text
User / Application
        │
        │ Upload Object
        ↓
      S3 API
        │
        ↓
      Bucket
        │
        ↓
      Object
```

A download flow:

```text
User / Application
        │
        │ Request Object
        ↓
      S3 API
        │
        ↓
      Bucket
        │
        ↓
      Object
        │
        ↓
      User / Application
```

---

## 12. S3 vs EBS vs EFS

| Feature            | S3                  | EBS                                         | EFS                      |
| ------------------ | ------------------- | ------------------------------------------- | ------------------------ |
| Type               | Object storage      | Block storage                               | File storage             |
| Mainly used for    | Files/data/backups  | EC2 disks                                   | Shared filesystem        |
| Attached to EC2    | No                  | Yes                                         | Through network          |
| Shared between EC2 | Yes, through S3 API | Generally one EC2 at a time for typical use | Yes                      |
| Scalability        | Very high           | Volume-based                                | Elastic                  |
| Example            | Backup files        | OS disk                                     | Shared application files |

### Easy way to remember

```text
S3  → Objects
EBS → Blocks
EFS → Files
```

---

## 13. Important S3 Concepts to Learn

For AWS Cloud Engineer preparation, understand these concepts well:

* Buckets
* Objects
* Object Keys
* Prefixes
* Regions
* Storage Classes
* Versioning
* Lifecycle Rules
* Bucket Policies
* IAM Permissions
* Block Public Access
* Encryption
* Replication
* Static Website Hosting
* Presigned URLs

---

## 14. Quick Revision

```text
S3 = Object Storage

Bucket = Container

Object = Actual Data

Key = Object Identifier

Prefix = Folder-like path

Versioning = Keeps object versions

Lifecycle = Automatically transitions/deletes objects

Replication = Copies objects to another bucket

Encryption = Protects stored data

Bucket Policy = Controls bucket access

Presigned URL = Temporary access to an object
```

---

## 15. Real-World Example

Suppose we have an application where users upload profile pictures.

```text
User
  │
  │ Upload Profile Picture
  ↓
Application
  │
  ↓
Amazon S3
  │
  └── my-profile-bucket
         │
         ├── users/
         │    ├── user1.jpg
         │    ├── user2.jpg
         │    └── user3.jpg
         │
         └── logos/
              └── company-logo.png
```

Instead of storing these files directly on an EC2 server, the application can store them in S3.

This provides scalable and durable object storage.

---

## 16. Key Takeaways

> **Amazon S3 is AWS's object storage service.**

Remember these four things first:

```text
S3
│
├── Bucket → Container
├── Object → Data
├── Key → Object Identifier
└── Prefix → Folder-like organization
```

Then learn the major S3 features:

```text
Storage Classes
      ↓
Versioning
      ↓
Lifecycle
      ↓
Security
      ↓
Encryption
      ↓
Replication
      ↓
Static Website
      ↓
Presigned URLs
```

---

## Next File

```text
Notes/S3-Storage-Classes.md
```

This will cover **S3 Standard, Intelligent-Tiering, IA, Glacier classes, use cases, differences, and when to choose each one.**
