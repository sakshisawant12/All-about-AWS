# Amazon VPC - VPC Flow Logs

## What are VPC Flow Logs?

VPC Flow Logs is an AWS feature that captures information about the IP network traffic flowing to and from network interfaces in your VPC.

It helps you monitor, troubleshoot, and analyze network traffic.

**Important:**
Flow Logs record **metadata about network traffic**, not the actual contents of packets.

---

# Why Do We Need VPC Flow Logs?

Imagine:

- An EC2 instance cannot connect to another EC2 instance.
- Users cannot access your website.
- SSH suddenly stops working.
- A Security Group blocks traffic.

Instead of guessing what happened, VPC Flow Logs show whether the traffic was accepted or rejected.

---

# What Can You Enable Flow Logs On?

You can enable Flow Logs at three levels:

### 1. VPC Level

Captures traffic for the entire VPC.

Best for:

- Organization-wide monitoring
- Security auditing

---

### 2. Subnet Level

Captures traffic only for a specific subnet.

Best for:

- Public subnet monitoring
- Private subnet troubleshooting

---

### 3. Network Interface (ENI) Level

Captures traffic for a specific ENI.

Best for:

- Troubleshooting a single EC2 instance
- Detailed network analysis

---

# Where Can Flow Logs Be Stored?

AWS supports:

- Amazon CloudWatch Logs
- Amazon S3

---

## CloudWatch Logs

Best for:

- Real-time monitoring
- Searching logs
- Creating CloudWatch Alarms

---

## Amazon S3

Best for:

- Long-term storage
- Compliance
- Analytics
- Athena queries

---

# What Information Does a Flow Log Record?

A Flow Log contains metadata such as:

- Source IP
- Destination IP
- Source Port
- Destination Port
- Protocol
- Number of Packets
- Number of Bytes
- Start Time
- End Time
- ACCEPT or REJECT

It does **not** capture:

- Packet contents
- Application data
- Request payloads
- Usernames or passwords

---

# ACCEPT vs REJECT

Every record has an action field.

### ACCEPT

Traffic was allowed.

Example:

Laptop

↓

SSH

↓

EC2

Security Group allows Port 22

Result:

ACCEPT

---

### REJECT

Traffic was blocked.

Possible reasons:

- Security Group
- Network ACL
- Missing route
- Other networking configuration

Example:

Internet

↓

Port 3389

↓

Linux EC2

↓

Security Group blocks traffic

Result:

REJECT

---

# Example Flow Log Record

```text
2 123456789 eni-0123456789abcdef0 10.0.1.10 10.0.2.15 49152 443 6 12 840 1719849600 1719849660 ACCEPT OK
```

---

# Understanding the Record

| Field | Meaning |
|--------|---------|
|eni-0123456789abcdef0|Network Interface|
|10.0.1.10|Source IP|
|10.0.2.15|Destination IP|
|49152|Source Port|
|443|Destination Port|
|6|TCP Protocol|
|12|Packets|
|840|Bytes|
|ACCEPT|Traffic Allowed|
|OK|Log Status|

---

# Example Architecture

                Internet
                     │
              Internet Gateway
                     │
                 Public EC2
                     │
                   ENI
                     │
              VPC Flow Logs
                     │
      ┌──────────────┴──────────────┐
      │                             │
CloudWatch Logs                 Amazon S3

---

# Real-World Example

Problem:

Users cannot access your web server.

Steps:

1. Check Security Group.
2. Check Network ACL.
3. Check Route Table.
4. Check VPC Flow Logs.

Flow Log:

REJECT

Destination Port:

80

Now you know traffic is being blocked before reaching the application.

---

# Use Cases

- Troubleshooting connectivity
- Security investigations
- Compliance
- Traffic analysis
- Detecting unauthorized access attempts
- Monitoring communication between resources

---

# Limitations

VPC Flow Logs do **not** capture:

- DNS query contents
- Packet payloads
- Application-level logs
- Traffic generated before Flow Logs were enabled

They are not a replacement for tools like packet capture (tcpdump/Wireshark).

---

# Best Practices

- Enable Flow Logs for production VPCs.
- Store logs in Amazon S3 for long-term retention.
- Use CloudWatch for operational monitoring.
- Restrict access to Flow Logs using IAM.
- Regularly review rejected traffic.

---

# Common Mistakes

❌ Expecting Flow Logs to show packet contents.

❌ Forgetting to enable Flow Logs before troubleshooting.

❌ Ignoring rejected traffic.

❌ Not protecting log access with IAM.

---

# Troubleshooting Example

Cannot SSH?

Flow Log shows:

Destination Port:

22

Action:

REJECT

Possible causes:

- Security Group
- Network ACL
- Route issue

Flow Logs help narrow down the problem.

---

# Interview Questions

### What are VPC Flow Logs?

VPC Flow Logs capture metadata about IP network traffic flowing to and from resources in a VPC.

---

### Do Flow Logs capture packet contents?

No.

They capture only metadata, not the actual packet data.

---

### Where can Flow Logs be stored?

- Amazon CloudWatch Logs
- Amazon S3

---

### What does ACCEPT mean?

The traffic was allowed.

---

### What does REJECT mean?

The traffic was blocked somewhere in the network path.

---

### At which levels can Flow Logs be enabled?

- VPC
- Subnet
- Network Interface (ENI)

---

# Key Takeaways

- VPC Flow Logs help monitor and troubleshoot network traffic.
- They capture metadata, not packet contents.
- They can be enabled at the VPC, subnet, or ENI level.
- Logs can be stored in CloudWatch Logs or Amazon S3.
- ACCEPT and REJECT records are valuable for troubleshooting and security analysis.
