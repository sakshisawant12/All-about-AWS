# Amazon S3 – Static Website Hosting

## 1. What is Static Website Hosting?

Amazon S3 can host a **static website** directly from an S3 bucket.

A static website contains files such as:

* HTML
* CSS
* JavaScript
* Images
* Fonts

Example:

```text
index.html
style.css
script.js
images/
   ├── logo.png
   └── banner.jpg
```

S3 serves these files to users over the web.

---

## 2. What is a Static Website?

A static website delivers pre-created files to users.

Example:

```text
User
  ↓
Web Browser
  ↓
S3
  ↓
index.html
  ↓
Website
```

Unlike a traditional server-side application, S3 does not execute backend code such as:

```text
Python
PHP
Java
Node.js backend
```

S3 is suitable for the **static frontend** of a website.

---

## 3. Static vs Dynamic Website

| Feature        | Static Website           | Dynamic Website           |
| -------------- | ------------------------ | ------------------------- |
| Content        | Pre-built files          | Generated dynamically     |
| Backend server | Not required for hosting | Usually required          |
| Database       | Not required             | Often required            |
| Example        | HTML/CSS/JS              | Django, Node.js, PHP      |
| S3 suitable    | Yes                      | Not for backend execution |

Example:

```text
Static:
HTML + CSS + JavaScript
        ↓
       S3
```

Dynamic:

```text
Browser
   ↓
Application Server
   ↓
Backend
   ↓
Database
```

---

## 4. S3 Static Website Architecture

Basic architecture:

```text
              User
                │
                ↓
           Web Browser
                │
                ↓
          Amazon S3
                │
                ↓
          S3 Bucket
          │    │    │
          ↓    ↓    ↓
       HTML  CSS   JS
```

---

## 5. Example Website Structure

Suppose we have:

```text
my-website/
│
├── index.html
├── error.html
├── style.css
├── script.js
└── images/
    ├── logo.png
    └── banner.jpg
```

These files can be uploaded to an S3 bucket.

Example:

```text
my-website-bucket
│
├── index.html
├── error.html
├── style.css
├── script.js
└── images/
```

---

## 6. Important Website Files

### `index.html`

The **index document** is the default page served when a user visits the website.

Example:

```text
index.html
```

It usually contains the homepage.

---

### `error.html`

The **error document** is displayed when the requested page cannot be found or an error occurs.

Example:

```text
error.html
```

---

## 7. Enabling Static Website Hosting

Basic AWS Console flow:

```text
S3
 ↓
Select Bucket
 ↓
Properties
 ↓
Static Website Hosting
 ↓
Enable
 ↓
Index Document
 ↓
index.html
 ↓
Error Document
 ↓
error.html
```

---

## 8. Upload Website Files

After enabling website hosting, upload the website files.

Example:

```text
S3 Bucket
│
├── index.html
├── error.html
├── style.css
├── script.js
└── images/
```

---

## 9. Public Access Requirement

A traditional S3 website endpoint requires the website content to be accessible to website visitors.

Historically, this was commonly achieved using:

```text
S3 Bucket
   ↓
Public Read Access
   ↓
Website
```

However, **making an S3 bucket public should be avoided unless it is specifically required and carefully configured**.

For modern production architectures, CloudFront is commonly placed in front of S3 so the bucket itself can remain private.

---

## 10. Block Public Access

S3 provides **Block Public Access** settings.

For a normal private bucket:

```text
Block Public Access
        ↓
Enabled
```

For a traditional public S3 website:

```text
Block Public Access
        ↓
Configuration must allow the required public access
```

This should only be changed deliberately.

> Never disable Block Public Access just because a tutorial says to do it. Understand why public access is required first.

---

## 11. Bucket Policy for Website Access

If using the traditional S3 website endpoint, a bucket policy can grant public read access to website objects.

Conceptually:

```text
Internet User
      ↓
Bucket Policy
      ↓
s3:GetObject
      ↓
Website Objects
```

A typical permission involved is:

```text
s3:GetObject
```

The exact bucket policy should be carefully scoped to the website objects.

---

## 12. Example Public Read Policy

A simplified example is:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-website-bucket/*"
    }
  ]
}
```

### Important

```text
Principal: "*"
```

means anyone can potentially access the objects covered by the policy.

Therefore, do **not** use a public bucket policy for sensitive or private data.

---

## 13. Website Endpoint

When static website hosting is enabled, S3 provides a **website endpoint** for the bucket.

Conceptually:

```text
Browser
   ↓
S3 Website Endpoint
   ↓
S3 Bucket
   ↓
index.html
```

The exact endpoint format depends on the AWS Region and S3 website configuration.

---

## 14. Website Endpoint vs S3 API Endpoint

These are different.

### S3 API Endpoint

Used for normal S3 API operations.

```text
AWS CLI
   ↓
S3 API
   ↓
Bucket
```

### S3 Website Endpoint

Used specifically for S3 static website hosting.

```text
Browser
   ↓
S3 Website Endpoint
   ↓
Website Content
```

Remember:

```text
S3 API endpoint
→ S3 API operations

S3 website endpoint
→ Static website
```

---

## 15. Upload Using AWS CLI

You can upload website files using:

```bash
aws s3 cp ./website/ s3://my-website-bucket/ --recursive
```

This copies the contents of the local `website` directory to the bucket.

---

## 16. Sync Website Files

`aws s3 sync` is useful when repeatedly deploying website files.

Example:

```bash
aws s3 sync ./website/ s3://my-website-bucket/
```

If you update:

```text
index.html
style.css
```

you can run the command again to synchronize changes.

---

## 17. Example Deployment Flow

```text
Local Computer
     │
     │ Website Files
     ↓
AWS CLI
     │
     ↓
S3 Bucket
     │
     ├── index.html
     ├── error.html
     ├── style.css
     └── script.js
     │
     ↓
Website Endpoint
     │
     ↓
User Browser
```

---

## 18. Static Website Hosting with CloudFront

For a production-style architecture, **Amazon CloudFront** can be placed in front of S3.

Architecture:

```text
                 User
                   │
                   ↓
              CloudFront
                   │
                   ↓
              Amazon S3
                   │
                   ↓
             Private Bucket
```

Advantages include:

* CDN caching
* Lower latency
* HTTPS support
* Better global performance
* Ability to keep the S3 bucket private with an appropriate CloudFront origin configuration

---

## 19. Recommended Production Architecture

Instead of:

```text
Internet
   ↓
Public S3 Bucket
```

a stronger architecture is often:

```text
Internet
   ↓
CloudFront
   ↓
Private S3 Bucket
```

CloudFront can retrieve content from S3 while users access the website through CloudFront.

This reduces the need to expose the S3 bucket directly.

---

## 20. Custom Domain

A custom domain can be used with a static website.

Example:

```text
www.example.com
        ↓
     CloudFront
        ↓
        S3
```

Services commonly involved:

* Amazon Route 53
* Amazon CloudFront
* Amazon S3
* AWS Certificate Manager

---

## 21. HTTPS

The traditional S3 website endpoint does not provide HTTPS in the same way a CloudFront distribution can.

For a production website, a common architecture is:

```text
User
 ↓
HTTPS
 ↓
CloudFront
 ↓
S3
```

CloudFront can use an SSL/TLS certificate to provide HTTPS to users.

---

## 22. Error Handling

Suppose a user requests:

```text
/about.html
```

but the file doesn't exist.

S3 can return the configured error document:

```text
error.html
```

Flow:

```text
User
 ↓
Request /about.html
 ↓
File Not Found
 ↓
error.html
```

---

## 23. S3 Static Website Hosting Use Cases

S3 static hosting is useful for:

* Portfolio websites
* Documentation websites
* Landing pages
* Simple company websites
* Static frontend applications
* HTML/CSS/JavaScript projects

---

## 24. S3 Website Hosting Limitations

S3 static website hosting is not designed to run server-side applications.

You cannot directly run:

```text
Django
Flask
PHP
Node.js backend
Java Spring
```

on S3.

For backend functionality, use services such as:

```text
EC2
Lambda
ECS
API Gateway
```

depending on the architecture.

---

## 25. Practical Lab

### Step 1 – Create Bucket

Create:

```text
my-static-website-2026
```

Choose an appropriate Region.

---

### Step 2 – Create Website Files

Example:

```text
website/
├── index.html
├── error.html
└── style.css
```

---

### Step 3 – Upload Files

```bash
aws s3 sync ./website/ s3://my-static-website-2026/
```

---

### Step 4 – Configure Website Hosting

Set:

```text
Index document:
index.html

Error document:
error.html
```

---

### Step 5 – Configure Access Carefully

If testing the traditional S3 website endpoint, configure only the minimum public access required for the website.

For production, prefer:

```text
CloudFront
     ↓
Private S3 Bucket
```

---

### Step 6 – Test Website

Open the configured website endpoint in a browser.

Expected flow:

```text
Browser
   ↓
S3 Website Endpoint
   ↓
index.html
   ↓
Website
```

---

## 26. Troubleshooting

### Website Shows Access Denied

Check:

```text
Block Public Access
Bucket Policy
Object Ownership
Object permissions
```

---

### Website Shows 404

Check:

```text
Index document
File name
Object key
Error document
```

---

### CSS Is Not Loading

Check:

```text
CSS file uploaded?
Correct object path?
Correct Content-Type?
HTML references correct path?
```

---

### Website Works Through S3 but Not CloudFront

Check:

```text
CloudFront origin
Origin access configuration
Cache
Distribution status
DNS configuration
HTTPS certificate
```

---

## 27. S3 Static Website Architecture

### Basic Learning Architecture

```text
                 Internet
                    │
                    ↓
              S3 Website
                    │
                    ↓
               S3 Bucket
              /    |    \
             ↓     ↓     ↓
          HTML    CSS    JS
```

### Production-Style Architecture

```text
                    Internet
                       │
                       ↓
                    HTTPS
                       │
                       ↓
                  CloudFront
                       │
                       ↓
                Private S3 Bucket
                  /    |    \
                 ↓     ↓     ↓
              HTML    CSS    JS
```

---

## 28. Important Security Reminder

A static website may contain public content, but that does **not** mean the entire S3 bucket should be public.

For example:

```text
Public:
index.html
style.css
logo.png
```

should not imply:

```text
Public:
database-backup.zip
customer-data.csv
private-documents.pdf
```

Keep sensitive data in separate private buckets or prefixes with appropriate access controls.

---

## 29. Quick Revision

```text
Static Website
→ HTML + CSS + JS

Index Document
→ Default page

Error Document
→ Error page

S3 Website Endpoint
→ Serves static website content

Bucket Policy
→ Can provide required public read access

CloudFront
→ CDN + HTTPS + better production architecture

S3
→ Stores website files

Route 53
→ DNS

ACM
→ SSL/TLS certificates
```

---

## 30. Key Takeaways

> **S3 can host static website files, but it does not run backend/server-side code.**

Remember:

```text
HTML
CSS
JavaScript
   ↓
S3
   ↓
Static Website
```

For a more production-ready architecture:

```text
User
  ↓
HTTPS
  ↓
CloudFront
  ↓
Private S3 Bucket
```

### Cloud Engineer Mindset

When designing S3 website hosting, ask:

```text
Is the website static?
        ↓
      Yes
        ↓
Can S3 store the files?
        ↓
      Yes
        ↓
Does the bucket need to be public?
        ↓
Prefer private + CloudFront
        ↓
Do I need HTTPS?
        ↓
CloudFront + ACM
        ↓
Do I need a custom domain?
        ↓
Route 53
```

---


