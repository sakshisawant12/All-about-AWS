# Amazon VPC - Network ACL (Network Access Control List)

## What is a Network ACL?

A Network ACL (NACL) is an optional layer of security that acts as a firewall for an entire subnet.

Unlike Security Groups, which protect individual instances, a Network ACL protects every resource inside the subnet.

It controls both inbound and outbound traffic entering or leaving the subnet.

---

# Why Do We Need a Network ACL?

A Network ACL provides an additional security layer before traffic reaches an EC2 instance.

Benefits:

- Protect an entire subnet.
- Allow or deny specific traffic.
- Block malicious IP addresses.
- Add another layer of defense in addition to Security Groups.

---

# How Network ACL Works

Traffic Flow:

Internet

↓

Internet Gateway

↓

Route Table

↓

Network ACL

↓

Security Group

↓

EC2 Instance

Traffic must pass through:

1. Route Table
2. Network ACL
3. Security Group

before reaching the EC2 instance.

---

# Network ACL is Stateless

This is the most important concept.

A Network ACL is **stateless**, meaning inbound and outbound traffic are evaluated separately.

Example:

Inbound Rule

Allow SSH

↓

Outbound Rule

Must also allow the response.

If the outbound rule does not allow the response, the connection fails.

Unlike Security Groups, return traffic is NOT automatically allowed.

---

# Inbound Rules

Inbound rules control traffic entering the subnet.

Example:

| Rule # | Protocol | Port | Source | Allow/Deny |
|--------|----------|------|--------|------------|
|100|TCP|22|Your IP|ALLOW|
|110|TCP|80|0.0.0.0/0|ALLOW|
|120|TCP|443|0.0.0.0/0|ALLOW|
|*|All|All|All|DENY|

---

# Outbound Rules

Outbound rules control traffic leaving the subnet.

Example:

| Rule # | Protocol | Port | Destination | Allow/Deny |
|--------|----------|------|-------------|------------|
|100|All|All|0.0.0.0/0|ALLOW|
|*|All|All|All|DENY|

---

# Rule Numbers

Every Network ACL rule has a unique number.

Example:

100

110

120

130

AWS evaluates rules in ascending order.

The first matching rule is applied.

Example:

Rule 100

Allow SSH

Rule 110

Deny SSH

Traffic matches Rule 100 first.

Result:

SSH is allowed.

---

# Default Network ACL

Every VPC has a Default Network ACL.

Characteristics:

- Allows all inbound traffic.
- Allows all outbound traffic.
- Can be modified.
- Associated with all subnets until another NACL is assigned.

---

# Custom Network ACL

You can create your own Network ACL.

Initially:

- No inbound rules.
- No outbound rules.

Traffic is denied until you create Allow rules.

---

# Allow and Deny Rules

Unlike Security Groups, Network ACLs support both:

✅ Allow

❌ Deny

Example:

Allow:

HTTP

HTTPS

SSH

Deny:

Specific malicious IP addresses

Blocked countries

Unused ports

---

# Network ACL Association

One subnet can be associated with only **one** Network ACL.

One Network ACL can be associated with **multiple** subnets.

---

# Real-World Example

Public Subnet

↓

Network ACL

Allow:

80

443

22

Deny:

23 (Telnet)

↓

Web Server

Private Subnet

↓

Network ACL

Allow:

3306

from Application Subnet

↓

Database

---

# Security Group vs Network ACL

| Feature | Security Group | Network ACL |
|---------|----------------|-------------|
|Works At|Instance Level|Subnet Level|
|State|Stateful|Stateless|
|Supports Allow Rules|Yes|Yes|
|Supports Deny Rules|No|Yes|
|Default Behavior|Deny Inbound, Allow Outbound|Allow All (Default NACL)|
|Rule Evaluation|All rules combined|Lowest rule number first|
|Multiple Attachments|Many SGs per EC2|One NACL per subnet|

---

# Traffic Evaluation Order

Traffic enters:

Internet

↓

Internet Gateway

↓

Route Table

↓

Network ACL

↓

Security Group

↓

EC2

Traffic leaving EC2:

EC2

↓

Security Group

↓

Network ACL

↓

Route Table

↓

Internet Gateway

↓

Internet

---

# Best Practices

- Use Security Groups as the primary firewall.
- Use Network ACLs for subnet-wide filtering.
- Block known malicious IP ranges.
- Keep rule numbering organized (100, 110, 120...).
- Avoid unnecessary Deny rules.

---

# Common Mistakes

❌ Forgetting that Network ACLs are stateless.

❌ Not allowing outbound response traffic.

❌ Incorrect rule numbering.

❌ Blocking ephemeral ports accidentally.

❌ Relying only on Network ACLs instead of Security Groups.

---

# Troubleshooting

Cannot SSH?

Check:

✅ Network ACL inbound allows TCP 22.

✅ Network ACL outbound allows ephemeral ports.

✅ Security Group allows SSH.

Cannot open website?

Check:

✅ Port 80 allowed.

✅ Port 443 allowed.

✅ Outbound traffic allowed.

---

# Interview Questions

### What is a Network ACL?

A Network ACL is a stateless firewall that controls inbound and outbound traffic at the subnet level.

---

### Are Network ACLs stateful?

No.

They are stateless.

Inbound and outbound rules are evaluated separately.

---

### Can Network ACLs deny traffic?

Yes.

Unlike Security Groups, Network ACLs support both Allow and Deny rules.

---

### Can one subnet have multiple Network ACLs?

No.

A subnet can be associated with only one Network ACL at a time.

---

### Which is evaluated first?

For incoming traffic:

Route Table

↓

Network ACL

↓

Security Group

↓

EC2

---

### Which should be your primary firewall?

Security Groups.

Use Network ACLs as an additional layer of subnet-level protection.

---

# Key Takeaways

- Network ACLs protect entire subnets.
- They are stateless.
- They support both Allow and Deny rules.
- Rules are processed in ascending order.
- A subnet can have only one Network ACL.
- Security Groups and Network ACLs work together to secure AWS resources.
