# 📘 Chapter 17 - EC2 Purchasing Options

> **"Amazon EC2 offers multiple purchasing options to help you balance cost, flexibility, and performance based on your workload requirements."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- Why EC2 has multiple purchasing options
- On-Demand Instances
- Reserved Instances (RI)
- Savings Plans
- Spot Instances
- Dedicated Instances
- Dedicated Hosts
- Capacity Reservations
- Cost comparison
- Real-world use cases
- Best Practices
- Interview Questions

---

# 📖 Why Does EC2 Have Different Purchasing Options?

Every application has different requirements.

For example:

- A developer testing an application for one day
- A company running a production database for years
- A startup processing millions of images overnight
- A bank requiring dedicated physical hardware

Using the same pricing model for all of them would be inefficient.

AWS provides multiple purchasing options so customers can choose the best balance between **cost**, **flexibility**, and **commitment**.

---

# 💡 Simple Definition

Imagine booking a hotel.

You have different choices:

- Pay for one night only
- Book for a year and get a discount
- Grab a last-minute discounted room
- Rent the entire building

EC2 purchasing options work in a similar way.

---

# 🏗️ EC2 Purchasing Options

```
Amazon EC2

│

├── On-Demand

├── Reserved Instances

├── Savings Plans

├── Spot Instances

├── Dedicated Instances

├── Dedicated Hosts

└── Capacity Reservations
```

---

# 1️⃣ On-Demand Instances

On-Demand Instances let you pay only for the compute capacity you use.

Characteristics:

- No long-term commitment
- Pay-as-you-go
- Start and stop anytime
- Most flexible option

---

### Best For

- Development
- Testing
- Short-term projects
- Unpredictable workloads

---

### Advantages

- No upfront payment
- Maximum flexibility
- Easy to launch

---

### Limitations

- Highest cost per hour compared to long-term options

---

# 2️⃣ Reserved Instances (RI)

Reserved Instances provide discounted pricing when you commit to using EC2 for **1 or 3 years**.

> **Note:** A Reserved Instance is primarily a **billing discount**, not a reserved virtual machine.

Characteristics:

- Significant discount compared to On-Demand
- Long-term commitment
- Best for predictable workloads

---

### Best For

- Production applications
- Databases
- Long-running web servers

---

### Advantages

- Lower cost
- Predictable billing

---

### Limitations

- Less flexible than On-Demand
- Commitment required

---

# 3️⃣ Savings Plans

Savings Plans provide flexible pricing based on a commitment to spend a certain amount per hour for **1 or 3 years**.

Unlike Reserved Instances, Savings Plans can automatically apply across eligible compute services.

Characteristics:

- Flexible
- Lower cost
- Automatically applies discounts to eligible usage

---

### Best For

- Organizations using multiple compute services
- Long-term workloads with changing instance types

---

### Advantages

- More flexible than Reserved Instances
- High discounts
- Simpler cost optimization

---

### Limitations

- Requires a spending commitment

---

# 4️⃣ Spot Instances

Spot Instances use AWS's unused EC2 capacity at heavily discounted prices.

Characteristics:

- Can provide discounts of up to **90%** compared to On-Demand
- AWS can interrupt the instance with short notice if capacity is needed elsewhere

---

### Best For

- Batch processing
- Data analytics
- CI/CD pipelines
- Image rendering
- Fault-tolerant workloads

---

### Advantages

- Lowest cost
- Great for flexible workloads

---

### Limitations

- Can be interrupted at any time

---

# 5️⃣ Dedicated Instances

Dedicated Instances run on hardware dedicated to a single AWS customer.

Characteristics:

- Physical server not shared with other AWS accounts
- Managed by AWS
- Helps meet certain compliance requirements

---

### Best For

- Compliance workloads
- Regulatory requirements

---

### Advantages

- Hardware isolation
- Simplified compliance

---

### Limitations

- Higher cost than shared tenancy

---

# 6️⃣ Dedicated Hosts

Dedicated Hosts provide an entire physical server exclusively for your use.

Characteristics:

- Full control over physical host placement
- Visibility into sockets, cores, and host-level details
- Supports software licenses tied to physical hardware (e.g., BYOL)

---

### Best For

- Bring Your Own License (BYOL)
- Licensing compliance
- Highly regulated environments

---

### Advantages

- Full hardware control
- License portability
- Compliance support

---

### Limitations

- Highest cost
- Requires more planning

---

# 7️⃣ Capacity Reservations

Capacity Reservations reserve EC2 capacity in a specific Availability Zone.

Characteristics:

- Guarantees capacity when needed
- No interruption due to capacity shortages
- Can be used independently or together with Savings Plans or Reserved Instances for pricing benefits

---

### Best For

- Business-critical applications
- Disaster recovery
- Planned events

---

### Advantages

- Guaranteed capacity
- Improves availability planning

---

### Limitations

- Charges may apply even if the reserved capacity is not fully utilized

---

# 📊 Purchasing Options Comparison

| Option | Cost | Commitment | Interruptible | Best For |
|---------|------|------------|---------------|----------|
| On-Demand | High | None | No | Development, Testing |
| Reserved Instance | Low | 1–3 Years | No | Predictable Production |
| Savings Plans | Low | 1–3 Years | No | Flexible Long-Term Workloads |
| Spot Instance | Lowest | None | Yes | Batch Jobs, CI/CD |
| Dedicated Instance | High | Optional | No | Compliance |
| Dedicated Host | Highest | Optional | No | BYOL, Licensing |
| Capacity Reservation | Varies | Optional | No | Guaranteed Capacity |

---

# 🌍 Real-World Examples

### Startup

```
Small Web Application

↓

On-Demand

↓

Maximum Flexibility
```

---

### Large Enterprise

```
Production Database

↓

Reserved Instance

↓

Lower Cost
```

---

### Media Company

```
Video Rendering

↓

Spot Instances

↓

Massive Cost Savings
```

---

### Bank

```
Licensed Database

↓

Dedicated Host

↓

Compliance + License Management
```

---

### Event Ticketing Platform

```
Expected High Demand

↓

Capacity Reservation

↓

Guaranteed EC2 Capacity
```

---

# ⭐ Best Practices

- Use **On-Demand** for short-term or unpredictable workloads.
- Use **Reserved Instances** or **Savings Plans** for long-running production workloads.
- Use **Spot Instances** for workloads that can tolerate interruptions.
- Use **Dedicated Hosts** only when licensing or compliance requires physical server visibility.
- Use **Capacity Reservations** when guaranteed EC2 capacity is essential.

---

# ❌ Common Mistakes

- Running production workloads on On-Demand for years without cost optimization.
- Using Spot Instances for critical applications that cannot tolerate interruptions.
- Confusing Reserved Instances with dedicated hardware.
- Choosing Dedicated Hosts when Dedicated Instances are sufficient.
- Assuming Capacity Reservations provide pricing discounts (they reserve capacity, not necessarily lower prices).

---

# 📝 Quick Revision

```
On-Demand
↓

Pay As You Go

--------------------

Reserved Instance
↓

Lower Cost
Long-Term

--------------------

Savings Plan
↓

Flexible Discount

--------------------

Spot Instance
↓

Lowest Cost
Interruptible

--------------------

Dedicated Instance
↓

Dedicated Hardware

--------------------

Dedicated Host
↓

Entire Physical Server

--------------------

Capacity Reservation
↓

Guaranteed Capacity
```

---

# 🎤 Interview Questions

### 1. Which EC2 purchasing option is best for short-term workloads?

**On-Demand Instances**

---

### 2. Which purchasing option offers the lowest cost?

**Spot Instances**, but they can be interrupted by AWS.

---

### 3. What is the difference between Reserved Instances and Savings Plans?

- **Reserved Instances** provide billing discounts tied to specific instance attributes.
- **Savings Plans** provide flexible discounts across eligible compute usage based on a spending commitment.

---

### 4. What is a Dedicated Host?

A Dedicated Host is an entire physical server dedicated to a single AWS customer, often used for licensing and compliance.

---

### 5. What is the purpose of a Capacity Reservation?

To guarantee that EC2 capacity is available in a specific Availability Zone when you need it.

---

### 6. Which purchasing option is best for production servers running continuously for years?

**Reserved Instances** or **Savings Plans**, depending on workload flexibility.

---

# 🎯 Chapter Summary

Amazon EC2 provides several purchasing options to meet different business needs. **On-Demand Instances** offer flexibility, **Reserved Instances** and **Savings Plans** reduce costs for long-term workloads, **Spot Instances** provide the lowest prices for interruptible tasks, **Dedicated Instances** and **Dedicated Hosts** support compliance and licensing requirements, and **Capacity Reservations** guarantee compute capacity in a specific Availability Zone. Choosing the right purchasing option helps optimize both cost and availability.
