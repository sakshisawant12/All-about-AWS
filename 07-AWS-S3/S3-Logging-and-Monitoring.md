# Amazon S3 – Logging and Monitoring

## 1. Why Monitor S3?

S3 can contain important or sensitive data, so monitoring helps you understand:

* Who accessed the bucket
* Which objects were accessed
* Who uploaded or deleted objects
* Which API operations were performed
* Whether suspicious activity occurred
* Whether requests are failing

A basic monitoring architecture is:

```text id="q7m3x9"
Users / Applications
        │
        ↓
       S3
        │
        ├── CloudTrail
        ├── CloudWatch
        └── Access Logs
```

---

## 2. Main S3 Monitoring Services

The important AWS services and features are:

* **AWS CloudTrail**
* **Amazon CloudWatch**
* **S3 Server Access Logging**
* **CloudTrail data events**
* **S3 Storage Lens**

Each serves a different purpose.

---

## 3. AWS CloudTrail

**AWS CloudTrail** records API activity in your AWS account.

For S3, CloudTrail can help answer questions such as:

```text id="m5x8q2"
Who accessed the bucket?
Who deleted the object?
Who uploaded the object?
What API operation was performed?
When did it happen?
```

Example:

```text id="n4q7x3"
User
 ↓
S3 API Request
 ↓
CloudTrail
 ↓
Event Record
```

---

## 4. S3 CloudTrail Events

CloudTrail can record S3 API activity.

Examples:

```text id="c8m2p5"
GetObject
PutObject
DeleteObject
ListBucket
```

These events can help with auditing and security investigations.

---

## 5. Management Events vs Data Events

This distinction is important.

### Management Events

These record management/control-plane operations.

Examples can include:

```text id="v6q3m8"
CreateBucket
DeleteBucket
PutBucketPolicy
PutBucketVersioning
```

### Data Events

These record operations on the actual data stored in S3.

Examples:

```text id="x7m4p2"
GetObject
PutObject
DeleteObject
```

### Easy Way to Remember

```text id="p9n3x6"
Management Events
→ Changes to AWS resources/configuration

Data Events
→ Access to actual S3 objects
```

---

## 6. S3 Data Events

S3 object-level activity is generally captured using **CloudTrail data events**.

Example:

```text id="r5m8q2"
User
 ↓
GetObject
 ↓
S3
 ↓
CloudTrail Data Event
```

This can help identify who accessed a particular object.

### Important

S3 data events can generate a large number of events, especially for busy buckets, so configure them according to your monitoring and auditing requirements.

---

## 7. Example CloudTrail Investigation

Suppose someone deleted:

```text id="k4x7m2"
backup/database.zip
```

You can use CloudTrail to investigate:

```text id="q8n3p5"
Object
   ↓
DeleteObject
   ↓
CloudTrail Event
   ↓
Who?
When?
From where?
Which credentials?
```

This is useful for troubleshooting and security investigations.

---

## 8. Amazon CloudWatch

**Amazon CloudWatch** is used to monitor AWS resources and applications.

For S3, CloudWatch can provide useful metrics and monitoring capabilities.

Examples include:

* Bucket request metrics
* Storage-related metrics
* Errors
* Monitoring dashboards
* Alarms

Architecture:

```text id="m3x8q5"
S3
 ↓
CloudWatch
 ↓
Metrics
 ↓
Dashboard / Alarm
```

---

## 9. CloudWatch vs CloudTrail

This is a common point of confusion.

| Feature                | CloudWatch            | CloudTrail                                |
| ---------------------- | --------------------- | ----------------------------------------- |
| Main purpose           | Monitoring            | Auditing/API activity                     |
| Metrics                | Yes                   | No                                        |
| API events             | Not its main purpose  | Yes                                       |
| Alarms                 | Yes                   | Can feed events into monitoring workflows |
| Security investigation | Useful                | Very useful                               |
| Example                | Request/error metrics | Who deleted an object                     |

### Easy Memory Trick

```text id="x7q2m9"
CloudWatch
→ "How is my service behaving?"

CloudTrail
→ "Who did what?"
```

---

## 10. S3 Server Access Logging

**S3 Server Access Logging** provides detailed records about requests made to an S3 bucket.

It can capture information about requests such as:

* Requester
* Bucket
* Object
* Request time
* Operation
* Response status

Example:

```text id="n6m3x8"
User
 ↓
S3 Request
 ↓
S3 Bucket
 ↓
Access Log
```

---

## 11. Server Access Logging Architecture

A common setup is:

```text id="v5q8m2"
                  Source Bucket
                       │
                       │ Requests
                       ↓
                 Access Logs
                       │
                       ↓
                Log Destination
                   Bucket
```

The destination bucket stores the access log files.

---

## 12. Why Use Server Access Logging?

It can be useful for:

* Access auditing
* Security analysis
* Troubleshooting
* Compliance requirements
* Understanding request patterns

Example:

```text id="k3m7x9"
Who accessed the bucket?
        ↓
Access Logs
```

---

## 13. CloudTrail vs Server Access Logging

These are not exactly the same.

| Feature                   | CloudTrail            | S3 Server Access Logging |
| ------------------------- | --------------------- | ------------------------ |
| Purpose                   | API auditing          | Request logging          |
| Object-level API activity | Yes, with data events | Yes, request records     |
| AWS API context           | Strong                | More request-focused     |
| Security investigation    | Excellent             | Useful                   |
| Configuration             | CloudTrail            | S3 bucket settings       |

For modern AWS auditing, CloudTrail is particularly important for API-level visibility.

---

## 14. S3 Storage Lens

**Amazon S3 Storage Lens** provides visibility into S3 storage usage and activity across your organization.

It can help analyze:

* Storage usage
* Object counts
* Storage classes
* Activity
* Cost optimization opportunities

Conceptually:

```text id="p4x8m2"
Multiple S3 Buckets
        │
        ↓
   S3 Storage Lens
        │
        ↓
Organization-wide
Visibility
```

---

## 15. Monitoring Storage Usage

Suppose an organization has:

```text id="q7m3x9"
Bucket A → 2 TB
Bucket B → 500 GB
Bucket C → 5 TB
```

Storage Lens can help provide visibility into storage trends and usage patterns.

This can help identify:

* Unused data
* Old objects
* Storage class opportunities
* Unexpected growth

---

## 16. Monitoring S3 Errors

Monitoring can help identify errors such as:

```text id="m8q2x5"
4xx
5xx
AccessDenied
NoSuchKey
```

Example:

```text id="x3n7m9"
Application
     ↓
S3 Request
     ↓
Error
     ↓
CloudWatch / Logs
     ↓
Investigation
```

---

## 17. Common S3 Errors

### AccessDenied

The identity does not have the required permissions or another policy is denying access.

Check:

```text id="v6m2q8"
IAM Policy
Bucket Policy
Access Point Policy
Block Public Access
KMS Permissions
```

---

### NoSuchKey

The requested object key does not exist.

Example:

```text id="p4x8n3"
Requested:
images/logo.png

Actual:
images/logos.png
```

The object key must match exactly.

---

### 403 Forbidden

Often indicates an access or authorization problem.

Check:

```text id="q9m3x7"
IAM
Bucket Policy
Object Permissions
KMS
VPC / Access Point configuration
```

---

## 18. S3 Monitoring for Security

Monitoring is especially important for sensitive buckets.

Example:

```text id="x5m8q2"
Sensitive Bucket
      │
      ↓
CloudTrail
      │
      ↓
Object Access Events
      │
      ↓
Security Investigation
```

You can look for unusual activity such as:

* Unexpected object deletion
* Large numbers of downloads
* Access from unexpected identities
* Policy changes
* Unusual API activity

---

## 19. CloudTrail Example

Suppose an attacker or unauthorized user deletes:

```text id="r7m3x9"
customer-data.csv
```

CloudTrail can help investigate:

```text id="k4q8m2"
DeleteObject
     │
     ├── Identity
     ├── Time
     ├── Source IP
     ├── Region
     └── Request details
```

This can help security teams understand what happened.

---

## 20. Monitoring Bucket Policy Changes

Changes to bucket security configuration are important.

Examples:

```text id="n5x3q8"
PutBucketPolicy
DeleteBucketPolicy
PutBucketPublicAccessBlock
PutBucketVersioning
```

These are management operations that can be logged by CloudTrail.

Example:

```text id="m7q2p4"
Bucket Policy
     ↓
Changed
     ↓
CloudTrail
     ↓
Audit
```

---

## 21. Monitoring Object Deletion

With appropriate CloudTrail data-event configuration:

```text id="x8m4q1"
DeleteObject
     ↓
CloudTrail
     ↓
Event
     ↓
Investigation
```

This is especially useful for important buckets such as:

```text id="p6n3m8"
backups/
customer-data/
financial-data/
```

---

## 22. Monitoring and Versioning

Versioning and monitoring can work together.

Example:

```text id="q5m8x3"
Object Deleted
     ↓
Versioning
     ↓
Previous Version Preserved
     │
     ↓
CloudTrail
     ↓
Who deleted it?
```

This provides both:

* Recovery
* Audit visibility

---

## 23. Monitoring and Lifecycle

Lifecycle rules can automatically transition or delete objects.

Therefore, when investigating object changes, remember that not every deletion is necessarily a manual user action.

Example:

```text id="v3m7x9"
Lifecycle Rule
     ↓
Expiration
     ↓
Object Removed
```

A Cloud Engineer should check lifecycle configuration when troubleshooting unexpected object transitions or deletions.

---

## 24. Monitoring Architecture

A practical monitoring architecture:

```text id="m8q4x2"
                    S3
                    │
       ┌────────────┼────────────┐
       │            │            │
       ↓            ↓            ↓
  CloudTrail    CloudWatch   Access Logs
       │            │            │
       ↓            ↓            ↓
   API Audit     Metrics      Requests
       │            │            │
       └────────────┼────────────┘
                    ↓
              Investigation
```

---

## 25. Practical Lab – CloudTrail

### Step 1

Open:

```text id="q4m8x2"
AWS CloudTrail
```

### Step 2

Create or use an appropriate trail/event data-store configuration.

### Step 3

Configure S3 data events according to the required scope.

### Step 4

Perform an S3 operation:

```bash id="x7n3m5"
aws s3 cp test.txt s3://my-bucket/
```

### Step 5

Check CloudTrail events.

You should be able to investigate the corresponding API activity when the configured event collection includes it.

---

## 26. Practical Lab – S3 Access Logging

Basic flow:

```text id="m5q8x3"
Create Log Destination Bucket
        ↓
Configure Server Access Logging
        ↓
Select Source Bucket
        ↓
Generate Requests
        ↓
Check Log Objects
```

Use a dedicated destination bucket when designing a logging architecture.

---

## 27. Best Practices

### 1. Enable appropriate CloudTrail auditing

Especially for sensitive S3 data.

### 2. Monitor important buckets

Prioritize:

```text id="n4m7x2"
Production
Backups
Customer Data
Financial Data
```

### 3. Use CloudWatch for operational monitoring

Set appropriate alarms for important metrics and errors.

### 4. Protect log data

Logs can contain sensitive information and should be protected with appropriate access controls.

### 5. Use separate log storage where appropriate

Keep audit logs separate from application data.

### 6. Monitor configuration changes

Track changes to:

* Bucket policies
* Versioning
* Public access settings
* Encryption settings

---

## 28. CloudWatch vs CloudTrail vs Access Logging

```text id="x8m3q5"
CloudWatch
    ↓
Metrics + Monitoring + Alarms

CloudTrail
    ↓
API Activity + Auditing

S3 Access Logging
    ↓
Detailed S3 Request Logs

S3 Storage Lens
    ↓
Storage Usage + Organization-wide Visibility
```

---

## 29. Real-World Cloud Engineer Scenario

A company stores customer documents in S3.

Requirements:

* Track who accesses objects
* Detect suspicious activity
* Monitor bucket usage
* Investigate accidental deletion

Architecture:

```text id="p7n4x2"
                    S3 Bucket
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
    CloudTrail     CloudWatch     Access Logs
        │              │              │
        ↓              ↓              ↓
   API Auditing     Metrics        Requests
        │              │              │
        └──────────────┼──────────────┘
                       ↓
                 Security Team
```

Versioning can also be enabled for recovery.

---

## 30. Troubleshooting Flow

When an S3 operation fails:

```text id="m6q3x8"
Request Failed
      ↓
Check Error Message
      ↓
Check IAM Permissions
      ↓
Check Bucket Policy
      ↓
Check Access Point Policy
      ↓
Check Block Public Access
      ↓
Check KMS Permissions
      ↓
Check VPC / Endpoint Configuration
      ↓
Check CloudTrail
      ↓
Identify Cause
```

---

## 31. Quick Revision

```text id="q8m4x2"
CloudWatch
→ Metrics, monitoring, alarms

CloudTrail
→ API activity and auditing

S3 Data Events
→ Object-level API activity

S3 Server Access Logging
→ S3 request logs

S3 Storage Lens
→ Storage usage and activity visibility

AccessDenied
→ Permission / policy problem

NoSuchKey
→ Object key not found
```

---

## 32. Key Takeaways

> **CloudTrail tells you what API activity happened, CloudWatch helps monitor service behavior, and S3 Access Logging records S3 requests.**

Remember:

```text id="x5n8m3"
CloudWatch
→ "How is S3 behaving?"

CloudTrail
→ "Who did what?"

Access Logs
→ "What requests reached S3?"

Storage Lens
→ "How are we using S3?"
```

### Cloud Engineer Mindset

When troubleshooting S3, don't immediately change permissions.

First:

```text id="v7m3q9"
Understand the error
       ↓
Check permissions
       ↓
Check policies
       ↓
Check encryption
       ↓
Check network path
       ↓
Check logs / CloudTrail
       ↓
Fix the actual cause
```

---

