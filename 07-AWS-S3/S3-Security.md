# Amazon S3 – Security

## 1. Why S3 Security is Important

Amazon S3 can store sensitive data such as:

* Customer documents
* Application files
* Backups
* Logs
* Images
* Database exports

If permissions are configured incorrectly, data can accidentally become publicly accessible.

S3 security mainly involves:

```text
IAM
 ↓
Bucket Policies
 ↓
Block Public Access
 ↓
Object Ownership
 ↓
Encryption
 ↓
Access Control
```

---

## 2. Main S3 Security Controls

Important S3 security mechanisms include:

* IAM policies
* Bucket policies
* Block Public Access
* Object Ownership
* ACLs
* Encryption
* Access Points
* VPC endpoints
* Logging and monitoring

---

## 3. IAM Policy vs Bucket Policy

This is one of the most important S3 concepts.

### IAM Policy

An IAM policy is attached to an **IAM identity**, such as:

* User
* Group
* Role

Example:

```text
IAM User
   ↓
IAM Policy
   ↓
Allow s3:GetObject
```

---

### Bucket Policy

A bucket policy is attached directly to an **S3 bucket**.

It is a resource-based policy.

Example:

```text
S3 Bucket
   ↓
Bucket Policy
   ↓
Allow / Deny access
```

---

## 4. IAM Policy vs Bucket Policy

| Feature         | IAM Policy                     | Bucket Policy                      |
| --------------- | ------------------------------ | ---------------------------------- |
| Type            | Identity-based                 | Resource-based                     |
| Attached to     | User, Group, Role              | S3 Bucket                          |
| Controls access | Yes                            | Yes                                |
| Common use      | Give permissions to identities | Control access to bucket/resources |

### Easy Way to Remember

```text
IAM Policy
→ "What can this identity do?"

Bucket Policy
→ "Who can access this bucket and what can they do?"
```

---

## 5. S3 Block Public Access

**S3 Block Public Access** helps prevent buckets and objects from becoming publicly accessible.

It provides four settings:

1. Block public ACLs
2. Ignore public ACLs
3. Block public bucket policies
4. Restrict public bucket policies

For a private bucket, keeping Block Public Access enabled is generally the safest default.

---

## 6. Why Block Public Access Matters

Imagine a company stores:

```text
customer-data/
├── customer1.pdf
├── customer2.pdf
└── customer3.pdf
```

If a bucket is accidentally made public:

```text
Internet
   ↓
Public S3 Bucket
   ↓
Customer Data
```

Anyone who can access the public resource may be able to retrieve the exposed data.

With appropriate Block Public Access settings:

```text
Internet
   ↓
Blocked
   X
S3 Bucket
```

---

## 7. S3 Access Control Lists (ACLs)

**ACLs** are an older access-control mechanism for S3.

They can define permissions for:

* Bucket
* Object

However, modern S3 configurations generally use **IAM policies and bucket policies** instead of ACLs.

With **Bucket owner enforced** Object Ownership, ACLs are disabled.

### Remember

```text
Modern S3
→ Prefer policies

Legacy / specific use cases
→ ACLs
```

---

## 8. S3 Object Ownership

Object Ownership determines ownership of objects uploaded to a bucket.

The recommended setting for many modern use cases is:

```text
Bucket owner enforced
```

With this setting:

* ACLs are disabled
* Bucket owner automatically owns objects
* Access is managed using policies

Example:

```text
User uploads object
       ↓
S3 Bucket
       ↓
Bucket owner owns object
       ↓
Policies control access
```

---

## 9. S3 Encryption

Encryption protects data stored in S3.

S3 supports **server-side encryption** and client-side encryption.

Common server-side encryption options include:

* SSE-S3
* SSE-KMS
* DSSE-KMS
* SSE-C

---

## 10. SSE-S3

**SSE-S3** means:

> Server-Side Encryption with Amazon S3 managed keys.

AWS manages the encryption keys for you.

Basic flow:

```text
Application
    ↓
Upload Object
    ↓
S3
    ↓
S3 encrypts object
    ↓
Encrypted Object
```

### Best For

* Simple encryption
* General-purpose data protection
* Applications that don't need direct KMS key management

---

## 11. SSE-KMS

**SSE-KMS** uses **AWS Key Management Service (KMS)** keys to encrypt S3 objects.

Flow:

```text
Application
      ↓
S3
      ↓
AWS KMS
      ↓
Encryption Key
      ↓
Encrypted Object
```

### Advantages

* Centralized key management
* Fine-grained permissions
* Key policies
* AWS CloudTrail integration
* Control over key usage

---

## 12. SSE-C

**SSE-C** means:

> Server-Side Encryption with Customer-Provided Keys.

The customer provides the encryption key with the request.

```text
Customer
   ↓
Customer-Provided Key
   ↓
S3
   ↓
Encrypted Object
```

AWS performs the encryption operation, but the customer is responsible for providing the key.

---

## 13. Encryption Comparison

| Encryption | Key Managed By | Use Case                         |
| ---------- | -------------- | -------------------------------- |
| SSE-S3     | Amazon S3      | Simple server-side encryption    |
| SSE-KMS    | AWS KMS        | More control and auditing        |
| SSE-C      | Customer       | Customer provides encryption key |

### Easy Memory Trick

```text
SSE-S3
→ S3 manages keys

SSE-KMS
→ KMS manages keys

SSE-C
→ Customer provides keys
```

---

## 14. Encryption at Rest vs Encryption in Transit

### Encryption at Rest

Protects data while it is stored in S3.

```text
Data
 ↓
S3
 ↓
Encrypted Storage
```

### Encryption in Transit

Protects data while moving between systems.

HTTPS/TLS is commonly used.

```text
Application
     │
     │ HTTPS / TLS
     ↓
    S3
```

### Remember

```text
At Rest
→ Stored data

In Transit
→ Moving data
```

---

## 15. HTTPS and S3

When accessing S3 through HTTPS, data is protected while traveling over the network.

Example:

```text
https://...
```

A bucket policy can also be designed to deny requests that are not using secure transport.

Conceptually:

```text
HTTP Request
    ↓
   DENY

HTTPS Request
    ↓
   ALLOW
```

---

## 16. IAM Permissions for S3

IAM policies can grant permissions for S3 actions.

Common S3 actions include:

```text
s3:ListBucket
s3:GetObject
s3:PutObject
s3:DeleteObject
```

### Example

A role might have permission to download objects:

```text
Allow
  ↓
s3:GetObject
  ↓
arn:aws:s3:::my-bucket/*
```

---

## 17. Bucket ARN vs Object ARN

This distinction is important when writing policies.

### Bucket ARN

```text
arn:aws:s3:::my-bucket
```

Used for bucket-level actions such as:

```text
s3:ListBucket
```

### Object ARN

```text
arn:aws:s3:::my-bucket/*
```

Used for object-level actions such as:

```text
s3:GetObject
s3:PutObject
s3:DeleteObject
```

### Easy Rule

```text
Bucket-level action
→ Bucket ARN

Object-level action
→ Object ARN
```

---

## 18. Public vs Private S3 Bucket

### Private Bucket

```text
User / Application
       ↓
IAM / Bucket Policy
       ↓
S3 Bucket
```

Access is explicitly controlled.

### Public Bucket

```text
Internet
    ↓
S3 Bucket
    ↓
Public Objects
```

Public access should only be enabled when there is a specific requirement, such as certain public website content, and should be configured carefully.

---

## 19. Presigned URLs

A **presigned URL** provides temporary access to an S3 object without making the bucket public.

Example:

```text
Private S3 Object
       ↓
Presigned URL
       ↓
Temporary Access
       ↓
User
```

### Example Use Case

A user needs to download a private PDF.

Instead of making the bucket public:

```text
Private Object
      ↓
Generate Presigned URL
      ↓
User downloads file
```

The URL can have an expiration time.

---

## 20. S3 Access Points

**S3 Access Points** provide dedicated access points for an S3 bucket.

They can simplify managing access for different applications or teams.

Example:

```text
                 S3 Bucket
                /    |    \
               /     |     \
        Access Point Access Point
             ↓           ↓
        Application A  Application B
```

Each access point can have its own policy.

---

## 21. S3 VPC Endpoint

Applications running inside a VPC can access S3 using an **S3 VPC endpoint**.

This can allow private connectivity to S3 without requiring traffic to travel through the public internet.

Example:

```text
Private EC2
    ↓
Private Subnet
    ↓
S3 VPC Endpoint
    ↓
Amazon S3
```

This is especially useful when private workloads need S3 access.

---

## 22. S3 Security Best Practices

### 1. Keep Block Public Access enabled

Unless public access is specifically required.

### 2. Use IAM roles

For AWS workloads such as EC2, prefer IAM roles instead of storing long-term access keys.

### 3. Enable encryption

Use appropriate S3 server-side encryption.

### 4. Use least privilege

Only provide the permissions required.

Example:

```text
Bad:
s3:*

Better:
s3:GetObject
```

### 5. Enable Versioning when appropriate

Helps protect against accidental deletion or overwriting.

### 6. Use lifecycle policies

Automatically transition or delete objects when appropriate.

### 7. Use HTTPS

Protect data while it is transferred.

### 8. Monitor access

Use AWS services such as CloudTrail and CloudWatch where appropriate.

---

## 23. Common S3 Security Mistakes

### Mistake 1: Making the entire bucket public

```text
Internet
   ↓
Public Bucket
   ↓
Sensitive Data
```

Avoid this unless there is a legitimate public-access requirement.

---

### Mistake 2: Giving `s3:*`

Giving an identity unrestricted S3 permissions violates the principle of least privilege in many scenarios.

Prefer only the required actions.

---

### Mistake 3: Using IAM user access keys on EC2

For an EC2 application, prefer:

```text
EC2
 ↓
IAM Role
 ↓
S3
```

instead of storing long-term IAM user access keys on the server.

---

### Mistake 4: Confusing bucket and object permissions

Remember:

```text
s3:ListBucket
→ Bucket-level

s3:GetObject
→ Object-level
```

---

## 24. S3 Security Architecture

```text
                  AWS Account
                       │
                       ↓
                  IAM Policies
                       │
                       ↓
                 ┌─────────────┐
                 │ S3 Bucket   │
                 └─────────────┘
                   │    │    │
          ┌────────┘    │    └─────────┐
          ↓             ↓              ↓
    Bucket Policy   Encryption    Block Public
                                    Access
          │             │              │
          └─────────────┼──────────────┘
                        ↓
                   S3 Objects
```

---

## 25. Real-World Example

Suppose an application running on EC2 needs to upload user documents to S3.

A secure design would be:

```text
                    EC2
                     │
                     │ IAM Role
                     ↓
              S3 Permissions
                     │
                     ↓
                S3 Bucket
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
     Encryption            Versioning
          │                     │
          └──────────┬──────────┘
                     ↓
                 S3 Objects
```

The bucket can remain private while the EC2 application accesses it through its IAM role.

---

## 26. Quick Revision

```text
IAM Policy
→ Permissions attached to identities

Bucket Policy
→ Resource-based permissions on S3

Block Public Access
→ Helps prevent accidental public access

Object Ownership
→ Controls object ownership and ACL behavior

SSE-S3
→ S3-managed encryption keys

SSE-KMS
→ KMS-managed encryption keys

SSE-C
→ Customer-provided encryption keys

Presigned URL
→ Temporary access to private objects

Access Point
→ Dedicated access point for a bucket

VPC Endpoint
→ Private access to S3 from a VPC
```

---

## 27. Key Takeaways

> **Keep S3 private by default and grant only the access that is required.**

The most important security concepts are:

```text
Private by Default
       ↓
Block Public Access
       ↓
Least Privilege
       ↓
IAM + Bucket Policies
       ↓
Encryption
       ↓
HTTPS
       ↓
Monitoring
```

---

