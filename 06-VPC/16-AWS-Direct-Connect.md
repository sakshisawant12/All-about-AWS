# Amazon VPC - AWS Direct Connect

## What is AWS Direct Connect?

AWS Direct Connect (DX) is a dedicated private network connection between your on-premises network and AWS.

Unlike a Site-to-Site VPN, Direct Connect does **not** use the public internet.

Instead, it uses a dedicated physical connection through an AWS Direct Connect location.

---

# Why Do We Need AWS Direct Connect?

Many organizations require:

- Consistent network performance
- Low latency
- High bandwidth
- Large data transfers
- Reliable Hybrid Cloud connectivity

Using the public internet may introduce:

- Variable latency
- Packet loss
- Congestion
- Unpredictable performance

Direct Connect solves these problems.

---

# Real-World Example

A bank stores customer data in its on-premises data center but runs analytics in AWS.

Instead of sending sensitive traffic over the internet, the bank establishes a Direct Connect link.

Traffic flows through a dedicated private network.

---

# How Direct Connect Works

Traffic Flow:

On-Premises Data Center

↓

Router

↓

AWS Direct Connect Location

↓

AWS Network

↓

Virtual Interface (VIF)

↓

Amazon VPC

↓

EC2

The traffic stays on dedicated network links instead of the public internet.

---

# Components of Direct Connect

## Customer Router

The router located in your office or data center.

It connects to the AWS Direct Connect location.

---

## AWS Direct Connect Location

A physical colocation facility where AWS provides Direct Connect connectivity.

Examples include data centers operated by providers such as Equinix and other approved partners.

Your network connects to AWS from this location.

---

## Direct Connect Connection

The physical network connection between your router and AWS.

Common speeds include:

- 1 Gbps
- 10 Gbps
- 100 Gbps

(Some locations also support additional speeds depending on AWS offerings.)

---

# Virtual Interfaces (VIF)

Traffic is carried over Virtual Interfaces.

There are three types.

---

## 1. Private VIF

Used to access:

- Amazon VPC
- EC2
- RDS
- Internal AWS resources

Traffic uses private IP addresses.

---

## 2. Public VIF

Used to access AWS public services such as:

- Amazon S3
- DynamoDB
- Public AWS APIs

Traffic does **not** traverse the public internet, even though it reaches public AWS service endpoints.

---

## 3. Transit VIF

Used together with **AWS Direct Connect Gateway** and **AWS Transit Gateway**.

Best for:

- Multiple VPCs
- Large enterprise networks
- Multi-account environments

---

# Direct Connect Gateway

A Direct Connect Gateway allows one Direct Connect connection to access multiple VPCs (often across Regions, subject to AWS-supported configurations).

This simplifies enterprise networking.

---

# Example Architecture

        On-Premises Data Center
                  │
             Customer Router
                  │
         AWS Direct Connect
                  │
       Direct Connect Location
                  │
             Private VIF
                  │
         Direct Connect Gateway
                  │
           Amazon VPC
                  │
             Private EC2

---

# Direct Connect vs Site-to-Site VPN

| Feature | Direct Connect | Site-to-Site VPN |
|---------|----------------|------------------|
|Uses Internet|No|Yes|
|Encryption|Not by default|Yes (IPsec)|
|Latency|Low & Predictable|Depends on Internet|
|Bandwidth|High & Consistent|Depends on Internet|
|Cost|Higher|Lower|
|Setup Time|Longer|Shorter|

---

# Direct Connect + VPN

Many production environments use both.

Direct Connect

↓

Primary Connection

VPN

↓

Backup Connection

If the Direct Connect link fails, traffic automatically switches to the VPN.

This provides high availability.

---

# Benefits

- Dedicated private connectivity
- Low latency
- Predictable performance
- High bandwidth
- Reduced internet dependence
- Supports Hybrid Cloud

---

# Best Practices

- Use Site-to-Site VPN as a backup.
- Use redundant Direct Connect connections for high availability.
- Monitor connection health using CloudWatch.
- Use BGP for dynamic route management.
- Plan bandwidth according to workload.

---

# Common Mistakes

❌ Assuming Direct Connect encrypts traffic automatically.

❌ Not configuring a VPN backup.

❌ Choosing insufficient bandwidth.

❌ Forgetting BGP configuration.

❌ Assuming Direct Connect replaces all security controls.

---

# Troubleshooting

Cannot reach AWS?

Check:

✅ Physical connection status.

✅ BGP session.

✅ Virtual Interface configuration.

✅ Route Tables.

✅ Direct Connect Gateway association.

---

# Interview Questions

### What is AWS Direct Connect?

AWS Direct Connect is a dedicated private network connection between an on-premises network and AWS.

---

### Does Direct Connect use the public internet?

No.

It uses dedicated private network links.

---

### Is Direct Connect encrypted?

Not by default.

If encryption is required, many organizations run a VPN over Direct Connect or use application-layer encryption.

---

### What are the three types of Virtual Interfaces?

- Private VIF
- Public VIF
- Transit VIF

---

### Why do companies use Direct Connect instead of a VPN?

Because it provides lower latency, consistent performance, higher bandwidth, and more predictable connectivity.

---

### Can Direct Connect and VPN be used together?

Yes.

Direct Connect is commonly used as the primary connection, with Site-to-Site VPN as the backup.

---

# Key Takeaways

- Direct Connect provides dedicated private connectivity to AWS.
- It does not use the public internet.
- It offers predictable performance and low latency.
- Private VIF connects to VPCs.
- Public VIF connects to AWS public services.
- Transit VIF supports enterprise-scale connectivity through Direct Connect Gateway and Transit Gateway.
- Direct Connect is often paired with Site-to-Site VPN for resilience.
