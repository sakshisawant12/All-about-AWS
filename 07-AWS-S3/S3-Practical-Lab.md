# Amazon S3 – Practical Lab

## 1. Lab Overview

In this lab, we will build and configure an S3 bucket from the AWS Management Console and AWS CLI.

### We will practice:

* Creating an S3 bucket
* Configuring security
* Enabling encryption
* Enabling versioning
* Uploading objects
* Downloading objects
* Testing versioning
* Configuring lifecycle management
* Generating a presigned URL
* Hosting a static website
* Verifying the configuration
* Cleaning up resources

---

# 2. Architecture

The lab architecture:

```text id="a6p3x8"
                         AWS Account
                              │
                              ↓
                         Amazon S3
                              │
                     ┌────────┴────────┐
                     ↓                 ↓
                S3 Bucket         S3 Features
                     │                 │
                     │          ┌──────┼──────┐
                     │          ↓      ↓      ↓
                     │     Versioning Encryption Lifecycle
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
       index.html  test.txt  private/
                              │
                              ↓
                         Presigned URL
```

---

# 3. Prerequisites

Before starting, make sure you have:

* AWS account
* AWS Management Console access
* AWS CLI installed
* AWS CLI configured
* Basic IAM knowledge
* Basic S3 knowledge

Verify AWS CLI:

```bash id="q7m3x9"
aws --version
```

Verify your AWS identity:

```bash id="m4x8p2"
aws sts get-caller-identity
```

---

# 4. Create the S3 Bucket

Create a bucket with a globally unique name.

Example:

```text id="x6n3q8"
s3-cloud-engineer-lab-2026
```

> Replace the name with your own unique bucket name.

Choose your preferred AWS Region.

For example:

```text id="p8m4x2"
ap-south-1
```

---

# 5. Configure Bucket Security

For a normal private bucket:

```text id="q3m7x9"
Block Public Access
        ↓
Keep Enabled
```

Do not make the bucket public just for testing normal S3 functionality.

The bucket should remain private for the main lab.

---

# 6. Enable Object Ownership

Use:

```text id="m8x4q2"
Bucket owner enforced
```

This disables ACL-based access control and makes policy-based access management simpler.

---

# 7. Enable Default Encryption

Enable default server-side encryption.

A simple option for this lab:

```text id="v5n8m3"
SSE-S3
```

Conceptually:

```text id="x7q3m9"
Upload Object
     ↓
S3
     ↓
SSE-S3
     ↓
Encrypted Object
```

---

# 8. Enable Versioning

Open:

```text id="p4m8x2"
S3
 ↓
Bucket
 ↓
Properties
 ↓
Bucket Versioning
 ↓
Enable
```

Or use the CLI:

```bash id="n6x3q8"
aws s3api put-bucket-versioning \
  --bucket s3-cloud-engineer-lab-2026 \
  --versioning-configuration Status=Enabled
```

Verify:

```bash id="m7q2x5"
aws s3api get-bucket-versioning \
  --bucket s3-cloud-engineer-lab-2026
```

Expected:

```json id="c8n4x7"
{
    "Status": "Enabled"
}
```

---

# 9. Create a Test File

Create a local file:

```text id="q5m8x3"
test.txt
```

Example content:

```text id="7x3m9p"
This is my Amazon S3 practical lab.
```

---

# 10. Upload the File

Run:

```bash id="m4q8x2"
aws s3 cp test.txt s3://s3-cloud-engineer-lab-2026/
```

Expected flow:

```text id="n7x3m5"
Local Computer
      ↓
AWS CLI
      ↓
S3 Bucket
      ↓
test.txt
```

---

# 11. Verify the Object

List objects:

```bash id="p6m2x8"
aws s3 ls s3://s3-cloud-engineer-lab-2026/
```

You should see:

```text id="x8q4m3"
test.txt
```

---

# 12. Download the Object

Download it to a new file:

```bash id="m3n7x9"
aws s3 cp \
  s3://s3-cloud-engineer-lab-2026/test.txt \
  ./downloaded-test.txt
```

Verify the file:

```bash id="q8m4x2"
cat downloaded-test.txt
```

Expected:

```text id="v5x7n3"
This is my Amazon S3 practical lab.
```

---

# 13. Test Versioning

Modify the local file:

```text id="k4m8x2"
This is Version 2 of my S3 test file.
```

Upload again:

```bash id="x7n3q5"
aws s3 cp test.txt s3://s3-cloud-engineer-lab-2026/
```

Now the bucket contains multiple versions.

---

# 14. List Object Versions

Run:

```bash id="m8q3x7"
aws s3api list-object-versions \
  --bucket s3-cloud-engineer-lab-2026
```

Look for:

```text id="p4x8m2"
Versions
├── Version ID 1
└── Version ID 2
```

This proves that versioning is working.

---

# 15. Test Object Deletion

Delete the current object:

```bash id="q7m3x8"
aws s3 rm s3://s3-cloud-engineer-lab-2026/test.txt
```

Because versioning is enabled, S3 normally creates a **delete marker**.

Check:

```bash id="x5n8m2"
aws s3api list-object-versions \
  --bucket s3-cloud-engineer-lab-2026
```

You should see a delete marker.

---

# 16. Understand the Delete Marker

The structure may look like:

```text id="m3x7q9"
test.txt
│
├── Version 1
├── Version 2
└── Delete Marker ← Current
```

The old versions still exist.

---

# 17. Restore the Object

Identify the delete marker's Version ID.

Then remove the delete marker:

```bash id="p8m4x2"
aws s3api delete-object \
  --bucket s3-cloud-engineer-lab-2026 \
  --key test.txt \
  --version-id DELETE_MARKER_VERSION_ID
```

Replace:

```text id="q3x7m9"
DELETE_MARKER_VERSION_ID
```

with the actual Version ID.

Now the previous object version becomes visible again.

---

# 18. Check Object Metadata

Run:

```bash id="m5x8q2"
aws s3api head-object \
  --bucket s3-cloud-engineer-lab-2026 \
  --key test.txt
```

This can show information such as:

* Content type
* Size
* Last modified
* ETag
* Encryption-related information

---

# 19. Create a Private Folder Structure

Create local directories:

```text id="x7m3q8"
website/
├── index.html
├── error.html
└── style.css
```

Example `index.html`:

```html id="n4q8m2"
<!DOCTYPE html>
<html>
<head>
    <title>S3 Cloud Engineer Lab</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>My S3 Website</h1>
    <p>Hosted using Amazon S3.</p>
</body>
</html>
```

Example `style.css`:

```css id="p6x3m8"
body {
    font-family: Arial, sans-serif;
    text-align: center;
}

h1 {
    margin-top: 100px;
}
```

---

# 20. Upload Website Files

```bash id="m8q4x2"
aws s3 sync ./website/ \
  s3://s3-cloud-engineer-lab-2026/website/
```

Verify:

```bash id="x3n7m5"
aws s3 ls \
  s3://s3-cloud-engineer-lab-2026/website/ \
  --recursive
```

Expected:

```text id="q5m8x3"
website/index.html
website/error.html
website/style.css
```

---

# 21. Configure Lifecycle Management

Create:

```text id="m4x7q2"
lifecycle.json
```

Example:

```json id="n8p3x5"
{
  "Rules": [
    {
      "ID": "ManageTestFiles",
      "Status": "Enabled",
      "Filter": {
        "Prefix": "test/"
      },
      "Expiration": {
        "Days": 30
      }
    }
  ]
}
```

Apply it:

```bash id="q7m3x9"
aws s3api put-bucket-lifecycle-configuration \
  --bucket s3-cloud-engineer-lab-2026 \
  --lifecycle-configuration file://lifecycle.json
```

Check:

```bash id="x4m8p2"
aws s3api get-bucket-lifecycle-configuration \
  --bucket s3-cloud-engineer-lab-2026
```

---

# 22. Generate a Presigned URL

For the private `test.txt` object:

```bash id="m6q3x8"
aws s3 presign \
  s3://s3-cloud-engineer-lab-2026/test.txt \
  --expires-in 600
```

Here:

```text id="p8x4m2"
600 seconds = 10 minutes
```

Open the generated URL in your browser.

The bucket remains private.

---

# 23. Test URL Expiration

After the configured expiration period:

```text id="x7m3q9"
Presigned URL
      ↓
Expiration
      ↓
Access denied
```

Generate a new URL if you need access again.

---

# 24. Check Public Access Configuration

Run:

```bash id="m5q8x3"
aws s3api get-public-access-block \
  --bucket s3-cloud-engineer-lab-2026
```

For a private bucket, you should see the Block Public Access settings enabled.

---

# 25. Check Encryption

Run:

```bash id="q3m7x8"
aws s3api get-bucket-encryption \
  --bucket s3-cloud-engineer-lab-2026
```

Verify that default encryption is configured.

---

# 26. Check Bucket Versioning

Run:

```bash id="x8m4p2"
aws s3api get-bucket-versioning \
  --bucket s3-cloud-engineer-lab-2026
```

Expected:

```json id="n6q3m8"
{
    "Status": "Enabled"
}
```

---

# 27. Check Lifecycle

Run:

```bash id="p4x7m2"
aws s3api get-bucket-lifecycle-configuration \
  --bucket s3-cloud-engineer-lab-2026
```

Verify that the lifecycle rule exists.

---

# 28. Optional – CloudTrail Verification

If CloudTrail data-event logging has been configured for the bucket, perform an S3 operation:

```bash id="m8q3x5"
aws s3 cp test.txt s3://s3-cloud-engineer-lab-2026/
```

Then investigate the corresponding CloudTrail event.

Conceptually:

```text id="q7m4x2"
AWS CLI
  ↓
S3 API
  ↓
PutObject
  ↓
CloudTrail
  ↓
Audit Event
```

---

# 29. Optional – S3 Static Website

For learning purposes, you can explore S3 static website hosting using a **separate test bucket**.

Why?

The main lab bucket should remain private.

A traditional S3 website endpoint may require public access to website objects.

For production architectures, prefer:

```text id="x5m8q3"
User
 ↓
CloudFront
 ↓
Private S3 Bucket
```

instead of exposing the S3 bucket directly.

---

# 30. Verify the Complete Lab

Run these commands:

### Check identity

```bash id="m4x7q2"
aws sts get-caller-identity
```

### Check bucket

```bash id="p8n3x5"
aws s3 ls
```

### Check objects

```bash id="q6m4x8"
aws s3 ls s3://s3-cloud-engineer-lab-2026/ --recursive
```

### Check versioning

```bash id="x3q7m9"
aws s3api get-bucket-versioning \
  --bucket s3-cloud-engineer-lab-2026
```

### Check encryption

```bash id="m7x4p2"
aws s3api get-bucket-encryption \
  --bucket s3-cloud-engineer-lab-2026
```

### Check public access block

```bash id="q8m3x5"
aws s3api get-public-access-block \
  --bucket s3-cloud-engineer-lab-2026
```

### Check lifecycle

```bash id="x4n7m2"
aws s3api get-bucket-lifecycle-configuration \
  --bucket s3-cloud-engineer-lab-2026
```

---

# 31. Lab Architecture – Final

```text id="m8q3x7"
                         AWS Account
                              │
                              ↓
                        Amazon S3
                              │
                     ┌────────┴────────┐
                     │                 │
                     ↓                 ↓
                S3 Bucket          Security
                     │                 │
         ┌───────────┼───────────┐     ├── Block Public Access
         │           │           │     ├── Encryption
         ↓           ↓           ↓     └── IAM
      Objects    Versions    Website
         │           │
         │           ↓
         │      Version Recovery
         │
         ├── Lifecycle
         │
         └── Presigned URL
```

---

# 32. What You Practiced

By completing this lab, you practiced:

* [x] Creating an S3 bucket
* [x] Choosing an AWS Region
* [x] Block Public Access
* [x] Object Ownership
* [x] Server-side encryption
* [x] Versioning
* [x] Uploading objects
* [x] Downloading objects
* [x] Listing objects
* [x] Deleting objects
* [x] Understanding delete markers
* [x] Restoring versioned objects
* [x] Lifecycle configuration
* [x] Presigned URLs
* [x] AWS CLI
* [x] S3 troubleshooting

---

# 33. GitHub Screenshots to Add

For your GitHub repository, capture screenshots of:

```text id="q5m8x3"
1. S3 Bucket created
2. Bucket Properties
3. Versioning Enabled
4. Default Encryption
5. Block Public Access
6. Uploaded Objects
7. Multiple Object Versions
8. Delete Marker
9. Lifecycle Rule
10. Presigned URL working
11. AWS CLI commands
12. Final S3 bucket structure
```

Store them in:

```text id="x7m3q9"
07-AWS-S3/
└── Screenshots/
```

Use meaningful names:

```text id="m4x8p2"
01-bucket-created.png
02-versioning-enabled.png
03-encryption.png
04-block-public-access.png
05-uploaded-objects.png
06-object-versions.png
07-delete-marker.png
08-lifecycle-rule.png
09-presigned-url.png
10-cli-commands.png
```

---

# 34. What to Write in Your Project README

The practical project demonstrates:

> **Secure Amazon S3 object storage with encryption, versioning, lifecycle management, private access, presigned URLs, and AWS CLI operations.**

Main technologies:

```text id="p8q3m7"
Amazon S3
AWS CLI
IAM
S3 Versioning
S3 Lifecycle
S3 Encryption
Presigned URLs
```

---

# 35. Important Learning Outcomes

After completing this lab, you should be able to explain:

### S3

> S3 is AWS object storage used to store and retrieve objects.

### Bucket

> A bucket is a container for S3 objects.

### Versioning

> Versioning preserves multiple versions of objects.

### Lifecycle

> Lifecycle rules automatically transition or expire objects.

### Encryption

> Encryption protects stored S3 data.

### Presigned URL

> A presigned URL provides temporary access to a private object.

### Block Public Access

> Block Public Access helps prevent unintended public exposure.

---

# 36. Cloud Engineer Scenario

### Requirement

A company wants to store application backups.

Requirements:

* Private storage
* Encryption
* Protection against accidental deletion
* Automatic lifecycle management
* Temporary download access
* CLI administration

### Solution

```text id="x6m3q8"
                Application
                    │
                    ↓
               Private S3
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
   Encryption   Versioning   Lifecycle
        │           │           │
        └───────────┼───────────┘
                    ↓
              Presigned URL
                    ↓
              Authorized User
```

This combines the main S3 concepts you've learned.

---

# 37. Final Revision

```text id="m7x4q2"
S3
│
├── Bucket
│
├── Objects
│
├── Storage Classes
│
├── Versioning
│
├── Lifecycle
│
├── Replication
│
├── Security
│   ├── IAM
│   ├── Bucket Policies
│   ├── Block Public Access
│   └── Encryption
│
├── Presigned URLs
│
├── Access Points
│
├── Logging & Monitoring
│
├── Cost Optimization
│
└── AWS CLI
```

---

# 38. Key Takeaways

> **S3 is more than just file storage. A Cloud Engineer needs to understand storage, security, access control, lifecycle, recovery, monitoring, and cost optimization together.**

The complete S3 workflow is:

```text id="q8m3x5"
Store
 ↓
Secure
 ↓
Version
 ↓
Manage
 ↓
Monitor
 ↓
Optimize
 ↓
Recover
```

---

