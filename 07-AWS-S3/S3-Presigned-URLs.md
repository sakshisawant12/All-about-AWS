# Amazon S3 – Presigned URLs

## 1. What is a Presigned URL?

A **presigned URL** is a temporary URL that provides access to a specific S3 object without making the entire bucket public.

It can be used to:

* Download private files
* Upload files
* Share files temporarily
* Give users controlled access to S3 objects

---

## 2. Why Use Presigned URLs?

Suppose we have a private S3 bucket:

```text id="4x8m2q"
Private S3 Bucket
      │
      ├── resume.pdf
      ├── certificate.pdf
      └── private-image.jpg
```

We don't want to make the bucket public.

Instead:

```text id="m7p3x9"
Private Object
      ↓
Presigned URL
      ↓
Temporary Access
      ↓
User
```

The user can access the specific object for a limited period.

---

## 3. Basic Concept

A presigned URL contains temporary authorization information.

```text id="q5n8v2"
Private S3 Object
       ↓
Generate Presigned URL
       ↓
Temporary URL
       ↓
User
       ↓
S3 Object
```

The URL is generated using AWS credentials that have the required permissions.

---

## 4. Presigned URL Does NOT Make the Bucket Public

This is an important point.

### Normal Public Access

```text id="c7m2x5"
Internet
   ↓
Public Bucket
   ↓
Objects
```

### Presigned URL

```text id="r9p4k6"
Internet
   ↓
Specific Presigned URL
   ↓
Specific S3 Object
```

The bucket can remain private.

---

## 5. How Presigned URLs Work

Example:

```text id="x8q3m7"
Application
     ↓
Authenticated User
     ↓
Application generates
Presigned URL
     ↓
User receives URL
     ↓
User accesses S3 Object
```

The application itself does not need to send the entire file through its own server.

---

## 6. GET Presigned URL

A **GET presigned URL** allows temporary access to download an object.

Example:

```text id="p4n7x2"
Private S3 Object
      ↓
GET Presigned URL
      ↓
User
      ↓
Download Object
```

### Common Use Case

A website stores private documents in S3.

When a user clicks:

```text id="1j8k4m"
Download Certificate
```

the application generates a temporary presigned GET URL.

---

## 7. PUT Presigned URL

A **PUT presigned URL** allows a client to upload an object directly to S3.

Example:

```text id="v6q2m8"
User
 ↓
Application
 ↓
Generate PUT Presigned URL
 ↓
User uploads directly
 ↓
S3
```

This can reduce the amount of file data that needs to pass through the application server.

---

## 8. GET vs PUT

| Type              | Purpose         |
| ----------------- | --------------- |
| GET Presigned URL | Download object |
| PUT Presigned URL | Upload object   |

### Easy Way to Remember

```text id="n5x9q3"
GET
→ Get file from S3

PUT
→ Put file into S3
```

---

## 9. Presigned URL Expiration

A presigned URL can have an expiration time.

Example:

```text id="m8q4x1"
Generate URL
     ↓
Valid for 10 minutes
     ↓
10 minutes pass
     ↓
URL expires
```

After expiration, the URL can no longer be used successfully for the authorized operation.

The expiration should be kept as short as practical for the use case.

---

## 10. Example Use Case – Private PDF

Suppose:

```text id="w3p7n9"
S3 Bucket
└── private/
      └── certificate.pdf
```

The bucket is private.

User requests the certificate:

```text id="c6m2x8"
User
 ↓
Application
 ↓
Generate Presigned URL
 ↓
User
 ↓
Download certificate.pdf
```

The bucket remains private.

---

## 11. Example Use Case – Profile Photo Upload

A website allows users to upload profile pictures.

Instead of:

```text id="4q9m2x"
User
 ↓
Application Server
 ↓
S3
```

the application can use:

```text id="n7x3p5"
User
 ↓
Application
 ↓
Presigned PUT URL
 ↓
S3
```

The browser uploads the file directly to S3.

---

## 12. Benefits of Presigned URLs

### 1. Temporary Access

Access can expire automatically.

### 2. Private Bucket

The bucket does not need to be public.

### 3. Direct Upload

Users can upload directly to S3.

### 4. Direct Download

Users can download directly from S3.

### 5. Reduced Application Server Load

Large files don't necessarily need to pass through the application server.

---

## 13. Security Considerations

Presigned URLs should be treated like **temporary credentials**.

Anyone who possesses a valid presigned URL may be able to use it according to the permissions and conditions associated with that URL until it expires.

Therefore:

* Keep expiration short
* Don't expose URLs unnecessarily
* Use HTTPS
* Generate URLs only for authorized users
* Restrict IAM permissions
* Use appropriate object keys
* Avoid putting sensitive information in object names

---

## 14. IAM Permissions

The identity generating the presigned URL must have the required S3 permission.

For a download:

```text id="a5n8q2"
s3:GetObject
```

For an upload:

```text id="j3m7x9"
s3:PutObject
```

Example:

```text id="w6p2k4"
Application IAM Role
       ↓
s3:GetObject
       ↓
Private S3 Object
```

---

## 15. AWS CLI – Generate Presigned GET URL

A simple example:

```bash id="z8m4q1"
aws s3 presign s3://my-bucket/private/file.pdf --expires-in 600
```

Here:

```text id="c4x7n2"
600 seconds = 10 minutes
```

The command generates a temporary presigned URL.

---

## 16. AWS CLI Example – Download

Suppose:

```text id="p7m3x5"
Bucket:
my-private-bucket

Object:
documents/resume.pdf
```

Generate a URL:

```bash id="r2n8q6"
aws s3 presign \
  s3://my-private-bucket/documents/resume.pdf \
  --expires-in 600
```

The resulting URL can be given to an authorized user.

---

## 17. Python Example

Presigned URLs can also be generated using the AWS SDK for Python, **Boto3**.

Example:

```python id="7q3m9x"
import boto3

s3 = boto3.client("s3")

url = s3.generate_presigned_url(
    "get_object",
    Params={
        "Bucket": "my-private-bucket",
        "Key": "documents/resume.pdf"
    },
    ExpiresIn=600
)

print(url)
```

This generates a URL valid for the configured expiration period.

---

## 18. Presigned PUT URL with Python

Example:

```python id="m4x8p2"
import boto3

s3 = boto3.client("s3")

url = s3.generate_presigned_url(
    "put_object",
    Params={
        "Bucket": "my-private-bucket",
        "Key": "uploads/photo.jpg"
    },
    ExpiresIn=600
)

print(url)
```

The client can use the URL to upload the object directly to S3.

---

## 19. Presigned URL Architecture

### Download

```text id="x5n8q3"
                    User
                      │
                      │ Request File
                      ↓
                Application
                      │
                      │ Generate
                      ↓
              Presigned GET URL
                      │
                      ↓
                 Amazon S3
                      │
                      ↓
                  S3 Object
                      │
                      ↓
                    User
```

### Upload

```text id="p6m2x9"
                    User
                      │
                      ↓
                Application
                      │
                      │ Generate
                      ↓
              Presigned PUT URL
                      │
                      ↓
                    User
                      │
                      │ Upload Directly
                      ↓
                 Amazon S3
```

---

## 20. Presigned URL vs Public URL

| Feature    | Presigned URL      | Public URL                           |
| ---------- | ------------------ | ------------------------------------ |
| Bucket     | Can remain private | Resource must be publicly accessible |
| Access     | Temporary          | Generally public                     |
| Expiration | Yes                | Not inherently temporary             |
| Security   | More controlled    | Less restrictive                     |
| Use Case   | Private files      | Public content                       |

---

## 21. Presigned URL vs IAM User Access

A user does not need AWS credentials simply to use a valid presigned URL.

Example:

```text id="k9x4m7"
AWS Credentials
      ↓
Application
      ↓
Generate Presigned URL
      ↓
Normal User
      ↓
S3
```

The application signs the request.

The end user only needs the URL.

---

## 22. Important Security Scenario

### Bad Approach

Make the entire bucket public:

```text id="q2m7x5"
Public Internet
       ↓
Public Bucket
       ↓
Private Documents
```

### Better Approach

Keep bucket private:

```text id="v8n3m6"
Private Bucket
      ↓
Authorized User
      ↓
Presigned URL
      ↓
Temporary Access
```

---

## 23. Presigned URLs and CloudFront

Presigned URLs are specifically an **S3 access mechanism**.

For applications using CloudFront, you may instead use **CloudFront signed URLs or signed cookies** when you need controlled access through CloudFront.

Example:

```text id="f5m8q2"
User
 ↓
CloudFront
 ↓
Private S3
```

The access-control mechanism should match the architecture.

---

## 24. Common Mistakes

### Mistake 1 – Very Long Expiration

Giving a URL an unnecessarily long lifetime increases the risk if it is leaked.

### Mistake 2 – Making the Bucket Public

Presigned URLs are designed to provide temporary access without requiring the bucket to be public.

### Mistake 3 – Excessive IAM Permissions

The application generating the URL should have only the permissions it needs.

### Mistake 4 – Sharing Presigned URLs Publicly

Anyone who obtains a valid URL may be able to use it before expiration.

### Mistake 5 – Confusing GET and PUT

```text id="m6x2q8"
GET → Download
PUT → Upload
```

---

## 25. Real-World Cloud Engineer Example

Suppose an HR application stores employee certificates.

```text id="r3n7m5"
HR Application
      │
      ↓
Private S3 Bucket
      │
      └── certificates/
            ├── employee1.pdf
            ├── employee2.pdf
            └── employee3.pdf
```

When an employee requests their certificate:

```text id="x8q4m2"
Employee
   ↓
HR Application
   ↓
Authentication
   ↓
Generate Presigned GET URL
   ↓
Employee
   ↓
Download from S3
```

The S3 bucket stays private.

---

## 26. Practical Lab

### Step 1 – Create Private Bucket

Create:

```text id="k5m9x3"
my-private-files-2026
```

Keep public access blocked.

---

### Step 2 – Upload File

```bash id="v4q8n2"
aws s3 cp certificate.pdf s3://my-private-files-2026/
```

---

### Step 3 – Generate Presigned URL

```bash id="c7m3x5"
aws s3 presign \
  s3://my-private-files-2026/certificate.pdf \
  --expires-in 600
```

---

### Step 4 – Test the URL

Open the generated URL in a browser.

The object should be accessible while the presigned URL is valid.

---

### Step 5 – Wait for Expiration

After the URL expires:

```text id="n8x2m6"
Presigned URL
     ↓
Expired
     ↓
Access denied
```

The S3 bucket remains private.

---

## 27. Troubleshooting

### Access Denied

Check:

```text id="m5q9x3"
IAM permissions
Bucket policy
Object key
Expiration
Credentials
```

### URL Expired

Generate a new presigned URL.

### Upload Fails

For PUT URLs, check:

```text id="r7n4k2"
s3:PutObject permission
Correct HTTP method
Object key
Encryption requirements
```

### Download Fails

For GET URLs, check:

```text id="p3x8m6"
s3:GetObject permission
Correct bucket
Correct object key
URL expiration
```

---

## 28. Quick Revision

```text id="w6m2q8"
Presigned URL
→ Temporary access to S3

GET
→ Download

PUT
→ Upload

Expiration
→ Controls how long the URL is valid

Private Bucket
→ Can remain private

IAM
→ Controls the permissions of the identity generating the URL

Security
→ Treat the URL like temporary credentials
```

---

## 29. Key Takeaways

> **A presigned URL provides temporary access to a private S3 object without making the bucket public.**

Remember:

```text id="x4n8m2"
Private S3 Object
       ↓
Application
       ↓
Generate Presigned URL
       ↓
Temporary Access
       ↓
User
```

### Most Important Points

```text id="q7m3v9"
GET Presigned URL
→ Download

PUT Presigned URL
→ Upload

Short Expiration
→ Better security

Private Bucket
→ Recommended for sensitive data

Least Privilege
→ Required permissions only
```

---

