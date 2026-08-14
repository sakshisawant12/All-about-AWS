# Amazon S3 – Access Points

## 1. What is an S3 Access Point?

An **S3 Access Point** is a dedicated endpoint that provides access to a specific S3 bucket.

Access Points make it easier to manage access to a shared bucket when multiple applications, teams, or users need different permissions.

Instead of putting one large bucket policy on a bucket, you can create separate access points with separate policies.

---

## 2. Why Use S3 Access Points?

Imagine one S3 bucket is shared by multiple applications:

```text id="9x4m2p"
                    S3 Bucket
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
     Application A  Application B  Team C
```

Managing all permissions through one bucket policy can become complicated.

With Access Points:

```text id="m7q3x8"
                    S3 Bucket
                  /     |      \
                 /      |       \
                ↓       ↓        ↓
          Access Point Access Point Access Point
              A             B          C
              ↓             ↓          ↓
          App A          App B       Team C
```

Each access point can have its own policy.

---

## 3. Simple Definition

> **S3 Access Point = A dedicated way to access an S3 bucket with its own access policy.**

Remember:

```text id="p5n8q2"
One Bucket
    ↓
Multiple Access Points
    ↓
Different Access Policies
```

---

## 4. Example Scenario

Suppose a company has:

```text id="c8m3x7"
company-data-bucket
```

It contains:

```text id="v4q9m2"
company-data-bucket/
│
├── finance/
├── marketing/
├── engineering/
└── backups/
```

Different teams need different access.

### Finance

```text id="n6x2p8"
Finance
→ finance/
```

### Marketing

```text id="r3m7q5"
Marketing
→ marketing/
```

### Engineering

```text id="k8v4x1"
Engineering
→ engineering/
```

Instead of creating one complicated access structure, Access Points can provide dedicated access paths.

---

## 5. Access Point Architecture

```text id="w5m9x3"
                       S3 Bucket
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ↓                ↓                ↓
     Finance AP       Marketing AP     Engineering AP
          │                │                │
          ↓                ↓                ↓
      Finance          Marketing       Engineering
```

Each Access Point can have its own policy.

---

## 6. Access Point Policy

An Access Point has an **access point policy** that controls access through that Access Point.

Example concept:

```text id="q7x3m9"
Finance Access Point
        ↓
Access Point Policy
        ↓
Allow Finance Application
        ↓
S3 Bucket
```

The policy can control:

* Who can access
* What actions are allowed
* Which resources can be accessed

---

## 7. Access Point vs Bucket Policy

Both can control access, but they serve different purposes.

| Feature           | Bucket Policy         | Access Point Policy                               |
| ----------------- | --------------------- | ------------------------------------------------- |
| Attached to       | Bucket                | Access Point                                      |
| Scope             | Bucket access         | Access through that Access Point                  |
| Multiple policies | One bucket policy     | Multiple access points can have separate policies |
| Useful for        | General bucket access | Different applications/teams                      |

### Easy Way to Remember

```text id="a4n8m6"
Bucket Policy
→ Controls bucket access

Access Point Policy
→ Controls access through a specific Access Point
```

---

## 8. Access Point ARN

An Access Point has its own ARN.

Conceptually:

```text id="z6p2x9"
arn:aws:s3:region:account-id:accesspoint/access-point-name
```

Example:

```text id="f8m4q3"
arn:aws:s3:ap-south-1:123456789012:accesspoint/finance-access
```

The exact ARN depends on your AWS account, Region, and Access Point name.

---

## 9. Access Point Alias

An Access Point can also have an **alias** that can be used to access the underlying bucket through the Access Point.

Conceptually:

```text id="j3q8m5"
Application
     ↓
Access Point Alias
     ↓
Access Point
     ↓
S3 Bucket
```

The alias is useful for applications that need an S3-style access endpoint.

---

## 10. VPC Access Points

S3 Access Points can be associated with a **VPC**.

A VPC Access Point allows access to S3 through a VPC rather than requiring the application to use a public internet path.

Example:

```text id="x9m4p7"
                 VPC
                  │
            Private Subnet
                  │
                  ↓
               EC2
                  │
                  ↓
          S3 VPC Access Point
                  │
                  ↓
              S3 Bucket
```

This is useful for applications running inside private VPC environments.

---

## 11. Why Use a VPC Access Point?

A VPC Access Point can help restrict access so that requests come through a specific VPC.

Example:

```text id="r5n8x2"
Private Application
       ↓
VPC
       ↓
VPC Access Point
       ↓
S3
```

This can provide an additional network-level control for accessing the bucket.

---

## 12. Access Point Network Origin

An S3 Access Point can have a network origin:

### Internet

```text id="c7m3q8"
Internet
    ↓
Access Point
    ↓
S3 Bucket
```

### VPC

```text id="p2x6m9"
VPC
 ↓
Access Point
 ↓
S3 Bucket
```

A VPC-origin Access Point is intended for access from the associated VPC.

---

## 13. Access Points and IAM

IAM permissions still matter.

Example:

```text id="v8q4m1"
IAM Role
   ↓
IAM Policy
   ↓
Access Point
   ↓
S3 Bucket
```

Access Point policies and IAM policies work together to determine whether an operation is allowed.

---

## 14. Access Points Do Not Create New Storage

This is important.

An Access Point does **not** create another copy of the bucket.

```text id="m5x8q3"
Access Point A ─┐
Access Point B ─┼──→ Same S3 Bucket
Access Point C ─┘
```

All Access Points point to the same underlying bucket.

---

## 15. Access Points and Prefixes

Access Points can be useful when different applications need access to different parts of a bucket.

Example:

```text id="q4n7m2"
company-bucket/
│
├── finance/
├── engineering/
├── marketing/
└── public/
```

You can design access controls around different application requirements.

For example:

```text id="x6p3m9"
Finance Application
       ↓
Finance Access Point
       ↓
Finance Data
```

---

## 16. Creating an Access Point

### AWS Console Flow

```text id="w9m4q7"
S3
 ↓
Access Points
 ↓
Create Access Point
 ↓
Choose Bucket
 ↓
Choose Access Point Name
 ↓
Choose Network Origin
 ↓
Configure Policy
 ↓
Create
```

---

## 17. AWS CLI

Create an Access Point:

```bash id="k3x8m5"
aws s3control create-access-point \
  --account-id 123456789012 \
  --name finance-access \
  --bucket company-data-bucket
```

For a VPC Access Point, the configuration also includes the VPC information.

---

## 18. Listing Access Points

You can list Access Points for an AWS account using:

```bash id="p7m2x4"
aws s3control list-access-points \
  --account-id 123456789012
```

---

## 19. Access Point Security

Follow the principle of least privilege.

Avoid policies that give unnecessary permissions.

### Bad

```text id="n4q8x2"
Allow:
s3:*
```

### Better

```text id="m6p3v9"
Allow:
s3:GetObject
s3:PutObject
```

depending on the application's actual requirements.

---

## 20. Access Points and Block Public Access

S3 Access Points also have public-access considerations.

For private applications:

```text id="c8x5m1"
Block Public Access
        ↓
Enabled
```

Avoid making an Access Point publicly accessible unless there is a legitimate requirement.

---

## 21. Real-World Example

Suppose a company has one central bucket:

```text id="r7m3q9"
company-data
│
├── finance/
├── hr/
├── engineering/
└── marketing/
```

There are four applications:

```text id="x4p8m2"
Finance App
HR App
Engineering App
Marketing App
```

Architecture:

```text id="z6n2q5"
                         company-data
                              │
            ┌─────────────────┼─────────────────┐
            ↓                 ↓                 ↓
       Finance AP          HR AP          Engineering AP
            │                 │                 │
            ↓                 ↓                 ↓
       Finance App         HR App        Engineering App
```

Each application can have a dedicated Access Point and policy.

---

## 22. Access Point with Private EC2

A common cloud architecture:

```text id="b8m4x7"
                 AWS VPC
                    │
             Private Subnet
                    │
                    ↓
                   EC2
                    │
                    ↓
             S3 VPC Endpoint
                    │
                    ↓
          S3 VPC Access Point
                    │
                    ↓
               S3 Bucket
```

This is useful when a private EC2 workload needs controlled access to S3.

---

## 23. Access Point vs Presigned URL

These solve different problems.

| Feature        | Access Point                  | Presigned URL                   |
| -------------- | ----------------------------- | ------------------------------- |
| Main purpose   | Controlled application access | Temporary object access         |
| Temporary      | Not necessarily               | Yes                             |
| Policy         | Access Point policy           | Permissions of signing identity |
| Common use     | Multiple applications/teams   | Temporary upload/download       |
| Bucket public? | Not required                  | Not required                    |

### Easy Way to Remember

```text id="m7q2x5"
Access Point
→ Who can access the bucket through this endpoint?

Presigned URL
→ Who can temporarily access this object?
```

---

## 24. Access Point vs VPC Endpoint

These are also different.

### S3 VPC Endpoint

Provides private connectivity from a VPC to S3.

```text id="q5n8m3"
VPC
 ↓
VPC Endpoint
 ↓
S3
```

### S3 Access Point

Provides a dedicated access endpoint and policy for a bucket.

```text id="x8m4p6"
Application
 ↓
Access Point
 ↓
S3 Bucket
```

They can be used together:

```text id="j3q7n9"
Private EC2
     ↓
S3 VPC Endpoint
     ↓
VPC Access Point
     ↓
S3 Bucket
```

---

## 25. Troubleshooting Access Point Access

If an application cannot access S3 through an Access Point, check:

### 1. IAM Permissions

Does the IAM role have the required S3 permissions?

### 2. Access Point Policy

Does the Access Point allow the request?

### 3. Bucket Policy

Does the bucket policy allow the required access?

### 4. VPC Configuration

For a VPC Access Point, verify:

```text id="n6x3q8"
Correct VPC
Correct routing
S3 connectivity
VPC endpoint
```

### 5. Block Public Access

Check whether public-access restrictions are affecting the intended design.

---

## 26. Quick Revision

```text id="p8m4x2"
S3 Access Point
→ Dedicated access endpoint for an S3 bucket

Access Point Policy
→ Controls access through the Access Point

VPC Access Point
→ Access Point associated with a VPC

Access Point Alias
→ S3-compatible endpoint name

One Bucket
→ Can have multiple Access Points

Access Points
→ Do not create additional copies of data
```

---

## 27. Key Takeaways

> **S3 Access Points simplify access management when many applications or teams need to use the same S3 bucket.**

Remember:

```text id="w4n7q2"
                S3 Bucket
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
   Access Point Access Point Access Point
        ↓           ↓           ↓
      App A       App B       App C
```

For private workloads:

```text id="x6m3p8"
Private EC2
     ↓
VPC
     ↓
VPC Access Point
     ↓
S3 Bucket
```


