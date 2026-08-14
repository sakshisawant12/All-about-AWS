# Amazon S3 – Storage Classes

## 1. What are S3 Storage Classes?

Amazon S3 provides different **storage classes** to help optimize storage based on:

* How frequently data is accessed
* How quickly data needs to be retrieved
* How long data needs to be stored
* Storage cost
* Availability requirements

Choosing the right storage class can help reduce AWS storage costs.

---

## 2. S3 Storage Classes

The major S3 storage classes are:

| Storage Class                 | Best For                                      |
| ----------------------------- | --------------------------------------------- |
| S3 Standard                   | Frequently accessed data                      |
| S3 Intelligent-Tiering        | Data with changing or unknown access patterns |
| S3 Standard-IA                | Infrequently accessed data                    |
| S3 One Zone-IA                | Infrequently accessed, re-creatable data      |
| S3 Glacier Instant Retrieval  | Archive data needing immediate access         |
| S3 Glacier Flexible Retrieval | Long-term archive with occasional retrieval   |
| S3 Glacier Deep Archive       | Very long-term archival                       |

---

## 3. S3 Standard

**S3 Standard** is designed for data that is accessed frequently.

### Features

* Frequently accessed data
* High availability
* High durability
* Low latency access
* No minimum storage duration requirement
* No minimum billable object size

### Common Use Cases

* Websites
* Mobile applications
* Content distribution
* Frequently accessed files
* Application data
* Images and videos

### Example

```text
Website
   ↓
Images
Videos
CSS
JavaScript
   ↓
S3 Standard
```

Use S3 Standard when your application needs to access data regularly.

---

## 4. S3 Intelligent-Tiering

**S3 Intelligent-Tiering** automatically moves objects between access tiers based on their access patterns.

You don't need to manually predict how frequently the data will be accessed.

### Best For

* Data with unknown access patterns
* Data whose access frequency changes over time
* Applications where access patterns are unpredictable

### Example

```text
Object
  ↓
S3 Intelligent-Tiering
  ↓
Frequently Accessed Tier
  ↓
Less Frequently Accessed Tier
  ↓
Archive Tier
```

AWS automatically manages the movement between supported tiers.

### Use Case

Suppose an application stores user documents.

Some documents may be accessed frequently while others may not be accessed for months.

Instead of manually deciding when to move them, Intelligent-Tiering can automatically optimize storage based on access patterns.

---

## 5. S3 Standard-IA

**IA** means **Infrequent Access**.

S3 Standard-IA is designed for data that:

* Is accessed less frequently
* Still needs quick access when requested
* Needs to be stored for a longer period

### Common Use Cases

* Backups
* Disaster recovery data
* Older application files
* Long-term data that is occasionally accessed

### Important Point

Standard-IA has lower storage costs than S3 Standard, but retrieval charges apply when data is accessed.

---

## 6. S3 One Zone-IA

**S3 One Zone-IA** stores data in a **single Availability Zone**.

It is designed for data that:

* Is accessed infrequently
* Does not require multiple-AZ storage
* Can be recreated if lost

### Common Use Cases

* Secondary backups
* Re-creatable data
* Thumbnails
* Processed data
* Data that can be generated again

### Important Difference

```text
Standard-IA
→ Designed for data requiring multi-AZ resilience

One Zone-IA
→ Stored in a single Availability Zone
```

Because One Zone-IA uses a single Availability Zone, it costs less than Standard-IA.

---

## 7. S3 Glacier Instant Retrieval

S3 Glacier Instant Retrieval is designed for **archive data that still needs immediate access**.

### Best For

* Archived images
* Medical images
* Long-term media storage
* Data that is rarely accessed but needs fast retrieval when requested

### Key Idea

```text
Rarely Accessed
       +
Immediate Retrieval
       ↓
Glacier Instant Retrieval
```

---

## 8. S3 Glacier Flexible Retrieval

S3 Glacier Flexible Retrieval is designed for **long-term archival data** where immediate access is not always required.

It supports different retrieval speeds.

### Best For

* Backups
* Disaster recovery
* Long-term archives
* Data that is accessed occasionally

### Retrieval

Data can be retrieved using different retrieval options depending on how quickly it is needed.

```text
Archive Data
     ↓
Glacier Flexible Retrieval
     ↓
Retrieve when required
```

---

## 9. S3 Glacier Deep Archive

**S3 Glacier Deep Archive** is designed for **very long-term data archival**.

It is generally the lowest-cost S3 storage class for long-term archival.

### Best For

* Compliance data
* Regulatory records
* Historical data
* Long-term backups
* Data that is rarely accessed

### Example

```text
Company Records
       ↓
Store for several years
       ↓
S3 Glacier Deep Archive
```

The trade-off is that retrieving data takes longer compared with standard S3 storage classes.

---

## 10. Storage Class Comparison

| Storage Class              | Access Pattern      | Retrieval | Main Use                 |
| -------------------------- | ------------------- | --------- | ------------------------ |
| S3 Standard                | Frequently accessed | Fast      | Active data              |
| Intelligent-Tiering        | Unknown/changing    | Fast      | Changing access patterns |
| Standard-IA                | Infrequent          | Fast      | Backups                  |
| One Zone-IA                | Infrequent          | Fast      | Re-creatable data        |
| Glacier Instant Retrieval  | Rare                | Immediate | Archive with fast access |
| Glacier Flexible Retrieval | Rare                | Flexible  | Long-term archive        |
| Glacier Deep Archive       | Very rare           | Longest   | Deep archival            |

---

## 11. Easy Way to Remember

```text
Frequently Used
      ↓
S3 Standard

Unknown / Changing Access
      ↓
S3 Intelligent-Tiering

Infrequently Used
      ↓
S3 Standard-IA

Infrequently Used + Single AZ
      ↓
S3 One Zone-IA

Archive + Immediate Access
      ↓
Glacier Instant Retrieval

Archive + Flexible Retrieval
      ↓
Glacier Flexible Retrieval

Very Long-Term Archive
      ↓
Glacier Deep Archive
```

---

## 12. S3 Storage Class Decision

A simple decision-making process:

```text
                Start
                  │
                  ↓
       Is data frequently accessed?
             /          \
           Yes           No
           ↓              ↓
     S3 Standard    Is access pattern
                    unpredictable?
                       /      \
                     Yes       No
                     ↓          ↓
             Intelligent     Is fast
               -Tiering     retrieval needed?
                              /       \
                            Yes        No
                            ↓           ↓
                     Standard-IA     Archive
                                      ↓
                              Glacier Classes
```

---

## 13. Storage Cost vs Retrieval

Generally:

```text
Higher Access
     ↓
Higher Storage Cost
     ↓
Faster / Easier Access
```

While:

```text
Lower Access
     ↓
Lower Storage Cost
     ↓
Potential Retrieval Charges / Longer Retrieval
```

The cheapest storage class is not automatically the best choice.

You should consider:

* Access frequency
* Retrieval speed
* Storage duration
* Retrieval charges
* Availability requirements
* Data recovery requirements

---

## 14. Example – Choosing a Storage Class

### Example 1: Website Images

A website receives thousands of image requests every day.

```text
Frequently accessed
        ↓
S3 Standard
```

---

### Example 2: Backup Files

A company creates backups that are rarely accessed but must be quickly available when needed.

```text
Infrequent access
        ↓
S3 Standard-IA
```

---

### Example 3: Unpredictable Data

An application stores data where nobody knows how frequently individual objects will be accessed.

```text
Unknown access pattern
        ↓
S3 Intelligent-Tiering
```

---

### Example 4: Old Company Records

A company must retain records for many years and rarely needs to access them.

```text
Very rare access
        ↓
S3 Glacier Deep Archive
```

---

## 15. Important Points for AWS Cloud Engineers

Remember these differences:

### Standard vs Standard-IA

```text
Standard
→ Frequently accessed

Standard-IA
→ Infrequently accessed
```

### Standard-IA vs One Zone-IA

```text
Standard-IA
→ Multiple Availability Zones

One Zone-IA
→ Single Availability Zone
```

### Glacier Classes

```text
Glacier Instant Retrieval
→ Archive + immediate access

Glacier Flexible Retrieval
→ Archive + flexible retrieval time

Glacier Deep Archive
→ Very long-term archival
```

---

## 16. Storage Classes and Lifecycle Management

S3 Lifecycle rules can automatically move objects between storage classes.

Example:

```text
Day 0
  ↓
S3 Standard
  ↓
Day 30
  ↓
S3 Standard-IA
  ↓
Day 90
  ↓
Glacier Flexible Retrieval
  ↓
Day 365
  ↓
Delete
```

This helps automatically optimize storage costs over time.

---

## 17. Key Takeaways

> **S3 Storage Classes allow you to choose storage based on access frequency, retrieval requirements, and cost.**

Remember:

```text
S3 Standard
→ Frequently accessed

Intelligent-Tiering
→ Unknown / changing access

Standard-IA
→ Infrequently accessed

One Zone-IA
→ Infrequent + single AZ

Glacier Instant Retrieval
→ Archive + immediate retrieval

Glacier Flexible Retrieval
→ Long-term archive

Glacier Deep Archive
→ Very long-term archive
```

---

This will cover **creating buckets, bucket naming, objects, prefixes, uploading/downloading objects, bucket properties, and important bucket settings**.
