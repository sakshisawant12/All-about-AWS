# Amazon S3 – AWS CLI Commands

## 1. What is AWS CLI?

**AWS CLI (Command Line Interface)** allows you to interact with AWS services from a terminal.

For S3, the AWS CLI can be used to:

* Create buckets
* List buckets
* Upload objects
* Download objects
* Copy objects
* Synchronize directories
* Delete objects
* Manage versioning
* Check bucket configuration
* Configure lifecycle rules

---

## 2. AWS CLI Prerequisites

Before using S3 commands, make sure AWS CLI is installed.

Check the version:

```bash
aws --version
```

Example:

```text
aws-cli/2.x.x Python/3.x ...
```

---

## 3. Configure AWS CLI

Configure credentials:

```bash
aws configure
```

You will be prompted for:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

Example:

```text
AWS Access Key ID: ********
AWS Secret Access Key: ********
Default region name: ap-south-1
Default output format: json
```

> For AWS workloads such as EC2, prefer IAM roles instead of storing long-term access keys whenever possible.

---

## 4. Check AWS Identity

Before working with S3, verify which AWS identity your CLI is using:

```bash
aws sts get-caller-identity
```

Example output:

```json
{
    "UserId": "AIDAXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/example"
}
```

This is a useful troubleshooting command.

---

# Bucket Commands

## 5. List All S3 Buckets

```bash
aws s3 ls
```

Example:

```text
2026-08-01  my-first-bucket
2026-08-05  my-backup-bucket
```

---

## 6. Create an S3 Bucket

For a Region such as `ap-south-1`:

```bash
aws s3api create-bucket \
  --bucket my-cloud-project-2026 \
  --region ap-south-1 \
  --create-bucket-configuration LocationConstraint=ap-south-1
```

### Important

S3 bucket names must be globally unique.

---

## 7. Check Bucket Location

```bash
aws s3api get-bucket-location \
  --bucket my-cloud-project-2026
```

---

## 8. List Objects in a Bucket

```bash
aws s3 ls s3://my-cloud-project-2026
```

---

## 9. List Objects Recursively

```bash
aws s3 ls s3://my-cloud-project-2026 --recursive
```

This shows objects inside prefixes as well.

Example:

```text
2026-08-14  10:00:00  1000  images/logo.png
2026-08-14  10:01:00  2000  documents/resume.pdf
```

---

## 10. List Objects with Human-Readable Sizes

```bash
aws s3 ls s3://my-cloud-project-2026 --recursive --human-readable --summarize
```

At the end, AWS CLI can show:

```text
Total Objects: 2
Total Size: 3.0 KiB
```

---

# Upload Commands

## 11. Upload a File

```bash
aws s3 cp file.txt s3://my-cloud-project-2026/
```

Flow:

```text
Local Computer
     ↓
file.txt
     ↓
S3 Bucket
```

---

## 12. Upload to a Prefix

```bash
aws s3 cp file.txt s3://my-cloud-project-2026/documents/
```

The object will be stored under:

```text
documents/file.txt
```

---

## 13. Upload Multiple Files

```bash
aws s3 cp ./files/ s3://my-cloud-project-2026/files/ --recursive
```

---

## 14. Upload Only Specific File Types

For example, upload only `.jpg` files:

```bash
aws s3 cp ./images/ s3://my-cloud-project-2026/images/ \
  --recursive \
  --exclude "*" \
  --include "*.jpg"
```

---

# Download Commands

## 15. Download an Object

```bash
aws s3 cp s3://my-cloud-project-2026/file.txt .
```

Here:

```text
.
```

means the current local directory.

---

## 16. Download to a Specific Directory

```bash
aws s3 cp s3://my-cloud-project-2026/file.txt ./downloads/
```

---

## 17. Download an Entire Prefix

```bash
aws s3 cp s3://my-cloud-project-2026/documents/ ./documents/ --recursive
```

---

# Copy Commands

## 18. Copy an Object Inside S3

```bash
aws s3 cp \
  s3://my-cloud-project-2026/file.txt \
  s3://my-cloud-project-2026/backup/file.txt
```

This copies the object without downloading it to your local computer first.

---

## 19. Copy Between Two Buckets

```bash
aws s3 cp \
  s3://source-bucket/file.txt \
  s3://destination-bucket/file.txt
```

Example:

```text
Source Bucket
     │
     │ S3 Copy
     ↓
Destination Bucket
```

---

# Sync Commands

## 20. What is `aws s3 sync`?

`sync` synchronizes files between a local directory and S3.

Example:

```bash
aws s3 sync ./website/ s3://my-cloud-project-2026/
```

Useful for:

* Website deployments
* Backups
* Repeated file synchronization

---

## 21. Sync S3 to Local

```bash
aws s3 sync s3://my-cloud-project-2026/ ./backup/
```

Flow:

```text
S3
 ↓
Local Directory
```

---

## 22. Sync Between Two S3 Buckets

```bash
aws s3 sync \
  s3://source-bucket/ \
  s3://destination-bucket/
```

---

## 23. Sync with Delete

```bash
aws s3 sync ./website/ s3://my-cloud-project-2026/ --delete
```

`--delete` removes destination files that don't exist in the source.

### Be Careful

```text
Source
 ↓
Sync
 ↓
Destination
 ↓
Extra destination files may be deleted
```

Always understand the source and destination before using `--delete`.

---

# Delete Commands

## 24. Delete an Object

```bash
aws s3 rm s3://my-cloud-project-2026/file.txt
```

---

## 25. Delete All Objects Under a Prefix

```bash
aws s3 rm s3://my-cloud-project-2026/temp/ --recursive
```

> Be careful with `--recursive`.

---

## 26. Delete All Objects in a Bucket

```bash
aws s3 rm s3://my-cloud-project-2026/ --recursive
```

This removes objects but does not necessarily delete the bucket itself.

---

## 27. Delete the Bucket

After the bucket is empty:

```bash
aws s3 rb s3://my-cloud-project-2026
```

---

## 28. Force Delete a Non-Versioned Bucket

For a non-versioned bucket:

```bash
aws s3 rb s3://my-cloud-project-2026 --force
```

> Use destructive commands carefully, especially in production.

---

# Versioning Commands

## 29. Enable Versioning

```bash
aws s3api put-bucket-versioning \
  --bucket my-cloud-project-2026 \
  --versioning-configuration Status=Enabled
```

---

## 30. Check Versioning Status

```bash
aws s3api get-bucket-versioning \
  --bucket my-cloud-project-2026
```

Example:

```json
{
    "Status": "Enabled"
}
```

---

## 31. Suspend Versioning

```bash
aws s3api put-bucket-versioning \
  --bucket my-cloud-project-2026 \
  --versioning-configuration Status=Suspended
```

Remember:

> Suspending versioning does not delete existing object versions.

---

## 32. List Object Versions

```bash
aws s3api list-object-versions \
  --bucket my-cloud-project-2026
```

This can show:

* Version IDs
* Object keys
* Delete markers
* Last modified times

---

# Presigned URL Commands

## 33. Generate a Presigned URL

```bash
aws s3 presign \
  s3://my-cloud-project-2026/private/file.pdf \
  --expires-in 600
```

Here:

```text
600 seconds = 10 minutes
```

The URL provides temporary access to the object.

---

# Bucket Security Commands

## 34. Check Public Access Block Configuration

```bash
aws s3api get-public-access-block \
  --bucket my-cloud-project-2026
```

---

## 35. Configure Public Access Block

Example:

```bash
aws s3api put-public-access-block \
  --bucket my-cloud-project-2026 \
  --public-access-block-configuration \
  BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

This is commonly appropriate for private buckets.

---

## 36. Get Bucket Policy

```bash
aws s3api get-bucket-policy \
  --bucket my-cloud-project-2026
```

---

## 37. Delete Bucket Policy

```bash
aws s3api delete-bucket-policy \
  --bucket my-cloud-project-2026
```

Use carefully because this changes access control.

---

# Encryption Commands

## 38. Check Default Encryption

```bash
aws s3api get-bucket-encryption \
  --bucket my-cloud-project-2026
```

---

## 39. Enable Default SSE-S3 Encryption

Example:

```bash
aws s3api put-bucket-encryption \
  --bucket my-cloud-project-2026 \
  --server-side-encryption-configuration '{
    "Rules": [
      {
        "ApplyServerSideEncryptionByDefault": {
          "SSEAlgorithm": "AES256"
        }
      }
    ]
  }'
```

Here:

```text
AES256
 ↓
SSE-S3
```

---

## 40. SSE-KMS

A bucket can also use SSE-KMS.

Conceptually:

```text
S3
 ↓
AWS KMS
 ↓
KMS Key
 ↓
Encrypted Object
```

The exact CLI configuration depends on the KMS key ARN and required permissions.

---

# Bucket Information Commands

## 41. Get Bucket Versioning

```bash
aws s3api get-bucket-versioning \
  --bucket my-cloud-project-2026
```

---

## 42. Get Bucket Lifecycle Configuration

```bash
aws s3api get-bucket-lifecycle-configuration \
  --bucket my-cloud-project-2026
```

---

## 43. Get Bucket Tagging

```bash
aws s3api get-bucket-tagging \
  --bucket my-cloud-project-2026
```

---

## 44. Get Bucket Ownership Controls

```bash
aws s3api get-bucket-ownership-controls \
  --bucket my-cloud-project-2026
```

---

## 45. Get Bucket Policy Status

```bash
aws s3api get-bucket-policy-status \
  --bucket my-cloud-project-2026
```

This can help determine whether a bucket is considered public based on its policy.

---

# Object Commands

## 46. Check Object Metadata

```bash
aws s3api head-object \
  --bucket my-cloud-project-2026 \
  --key file.txt
```

This returns metadata without downloading the object.

---

## 47. Copy an Object with a Different Storage Class

Example:

```bash
aws s3 cp \
  s3://my-cloud-project-2026/file.txt \
  s3://my-cloud-project-2026/archive/file.txt \
  --storage-class STANDARD_IA
```

This creates a copy using the specified storage class.

---

## 48. Upload Directly with a Storage Class

```bash
aws s3 cp file.txt \
  s3://my-cloud-project-2026/file.txt \
  --storage-class STANDARD_IA
```

---

# Lifecycle Commands

## 49. Apply Lifecycle Configuration

Create a file:

```text
lifecycle.json
```

Example:

```json
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
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```

Apply it:

```bash
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-cloud-project-2026 \
  --lifecycle-configuration file://lifecycle.json
```

---

## 50. Delete Lifecycle Configuration

```bash
aws s3api delete-bucket-lifecycle \
  --bucket my-cloud-project-2026
```

---

# Website Hosting Commands

## 51. Configure Static Website Hosting

Example:

```bash
aws s3api put-bucket-website \
  --bucket my-cloud-project-2026 \
  --website-configuration '{
    "IndexDocument": {
      "Suffix": "index.html"
    },
    "ErrorDocument": {
      "Key": "error.html"
    }
  }'
```

---

## 52. Check Website Configuration

```bash
aws s3api get-bucket-website \
  --bucket my-cloud-project-2026
```

---

## 53. Upload Website

```bash
aws s3 sync ./website/ s3://my-cloud-project-2026/
```

Example:

```text
website/
├── index.html
├── error.html
├── style.css
└── script.js
```

---

# Useful Filtering Commands

## 54. Exclude Files

```bash
aws s3 cp ./website/ s3://my-cloud-project-2026/ \
  --recursive \
  --exclude "*.log"
```

---

## 55. Include Only Specific Files

```bash
aws s3 cp ./website/ s3://my-cloud-project-2026/ \
  --recursive \
  --exclude "*" \
  --include "*.html"
```

---

# Practical S3 Workflow

## 56. Complete Basic Workflow

```text
1. Configure AWS CLI
        ↓
2. Verify Identity
        ↓
3. Create Bucket
        ↓
4. Check Bucket
        ↓
5. Enable Versioning
        ↓
6. Configure Encryption
        ↓
7. Upload Object
        ↓
8. List Object
        ↓
9. Download Object
        ↓
10. Test Permissions
        ↓
11. Configure Lifecycle
        ↓
12. Monitor
```

---

# Practical Lab

## 57. Create an S3 Bucket

```bash
aws s3api create-bucket \
  --bucket my-s3-lab-2026-unique \
  --region ap-south-1 \
  --create-bucket-configuration LocationConstraint=ap-south-1
```

---

## 58. Enable Versioning

```bash
aws s3api put-bucket-versioning \
  --bucket my-s3-lab-2026-unique \
  --versioning-configuration Status=Enabled
```

---

## 59. Upload a Test File

Create:

```text
test.txt
```

Then:

```bash
aws s3 cp test.txt s3://my-s3-lab-2026-unique/
```

---

## 60. Verify the Object

```bash
aws s3 ls s3://my-s3-lab-2026-unique/
```

---

## 61. Download the Object

```bash
aws s3 cp \
  s3://my-s3-lab-2026-unique/test.txt \
  ./downloaded-test.txt
```

---

## 62. Modify and Re-upload

Change `test.txt`, then:

```bash
aws s3 cp test.txt s3://my-s3-lab-2026-unique/
```

Because versioning is enabled, the previous version is retained.

---

## 63. Check Versions

```bash
aws s3api list-object-versions \
  --bucket my-s3-lab-2026-unique
```

---

## 64. Generate a Presigned URL

```bash
aws s3 presign \
  s3://my-s3-lab-2026-unique/test.txt \
  --expires-in 600
```

---

## 65. Clean Up

Delete objects:

```bash
aws s3 rm \
  s3://my-s3-lab-2026-unique/ \
  --recursive
```

### Important for Versioned Buckets

A normal recursive delete does not necessarily permanently remove every historical object version.

For a versioned bucket, inspect versions before attempting complete cleanup.

---

# `aws s3` vs `aws s3api`

## 66. High-Level Commands

Use:

```bash
aws s3
```

for common file operations.

Examples:

```bash
aws s3 ls
aws s3 cp
aws s3 sync
aws s3 rm
aws s3 mv
```

Think:

```text
aws s3
→ Easy / high-level operations
```

---

## 67. Low-Level API Commands

Use:

```bash
aws s3api
```

for detailed S3 API operations.

Examples:

```bash
aws s3api create-bucket
aws s3api get-bucket-versioning
aws s3api put-bucket-versioning
aws s3api get-bucket-encryption
```

Think:

```text
aws s3api
→ Detailed API-level configuration
```

---

# Important Commands Cheat Sheet

## 68. Most Important Commands

```bash
# Check AWS identity
aws sts get-caller-identity

# List buckets
aws s3 ls

# Create bucket
aws s3api create-bucket ...

# List objects
aws s3 ls s3://bucket-name/

# Upload
aws s3 cp file.txt s3://bucket-name/

# Download
aws s3 cp s3://bucket-name/file.txt .

# Sync
aws s3 sync ./folder/ s3://bucket-name/

# Delete object
aws s3 rm s3://bucket-name/file.txt

# Enable versioning
aws s3api put-bucket-versioning ...

# Check versioning
aws s3api get-bucket-versioning ...

# List versions
aws s3api list-object-versions ...

# Generate presigned URL
aws s3 presign s3://bucket-name/file.txt --expires-in 600

# Check encryption
aws s3api get-bucket-encryption ...

# Check lifecycle
aws s3api get-bucket-lifecycle-configuration ...

# Check public access block
aws s3api get-public-access-block ...
```

---

# Common Troubleshooting

## 69. AccessDenied

Check:

```text
IAM Policy
Bucket Policy
Access Point Policy
Block Public Access
KMS Permissions
```

---

## 70. NoSuchBucket

Check:

```text
Bucket Name
AWS Account
AWS Region
```

---

## 71. NoSuchKey

Check:

```text
Object Key
Prefix
File Name
Case Sensitivity
```

For example:

```text
images/Logo.png
```

is different from:

```text
images/logo.png
```

---

## 72. InvalidAccessKeyId

Check:

```text
AWS Credentials
AWS Profile
Access Key
```

You can verify the active identity:

```bash
aws sts get-caller-identity
```

---

## 73. Wrong Region

Check the bucket's Region:

```bash
aws s3api get-bucket-location \
  --bucket my-cloud-project-2026
```

---

# 74. Cloud Engineer Practical Flow

When working with S3 from the CLI:

```text
AWS CLI
   ↓
Authentication
   ↓
IAM Permissions
   ↓
S3 API
   ↓
Bucket
   ↓
Object
```

If something fails:

```text
Request
  ↓
Error
  ↓
Check Identity
  ↓
Check IAM
  ↓
Check Bucket Policy
  ↓
Check Encryption
  ↓
Check Region
  ↓
Check Object Key
  ↓
Check CloudTrail
```

---

# 75. Key Takeaways

> **AWS CLI allows you to manage S3 buckets and objects without using the AWS Management Console.**

Remember the most important commands:

```text
aws s3 ls
→ List

aws s3 cp
→ Copy / Upload / Download

aws s3 sync
→ Synchronize

aws s3 rm
→ Remove

aws s3api
→ Detailed S3 configuration

aws s3 presign
→ Temporary access

aws s3api list-object-versions
→ View versions
```

The commands you should be comfortable using as a Cloud Engineer are:

```text
Create
 ↓
List
 ↓
Upload
 ↓
Download
 ↓
Copy
 ↓
Sync
 ↓
Version
 ↓
Secure
 ↓
Monitor
 ↓
Clean Up
```

---

