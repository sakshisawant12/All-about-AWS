# Amazon VPC - VPC Peering

## What is VPC Peering?

VPC Peering is a networking connection between two VPCs that allows them to communicate privately using their private IP addresses.

Traffic never travels over the public internet.

AWS uses its private global backbone network for communication.

---

# Why Do We Need VPC Peering?

Imagine a company has two separate VPCs:

VPC A

- Web Servers
- Application Servers

VPC B

- Database
- Analytics

Both VPCs need to communicate securely.

Instead of exposing resources to the internet, VPC Peering provides a private connection.

---

# How VPC Peering Works

                 AWS Backbone Network

      ┌────────────────────────────────────┐
      │                                    │
  VPC A  ─────── VPC Peering ───────  VPC B
      │                                    │
 Private EC2                         Database

Traffic flows directly between the two VPCs.

No Internet Gateway, NAT Gateway, or VPN is required.

---

# Types of VPC Peering

## 1. Intra-Region Peering

Both VPCs are in the same AWS Region.

Example:

Mumbai

↓

VPC-A

↓

VPC-B

This is the most common type.

---

## 2. Inter-Region Peering

The VPCs are in different AWS Regions.

Example:

Mumbai

↓

VPC-A

↓

VPC-B

↓

Singapore

Traffic still uses AWS's private network.

---

# Requirements for VPC Peering

To establish a peering connection:

- Create a Peering Connection.
- Accept the request from the other VPC (if different account).
- Update Route Tables.
- Update Security Groups if needed.
- Ensure CIDR blocks do not overlap.

---

# Route Table Configuration

Example:

VPC A

CIDR:

10.0.0.0/16

VPC B

CIDR:

172.16.0.0/16

Route Table in VPC A:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|172.16.0.0/16|VPC Peering Connection|

Route Table in VPC B:

| Destination | Target |
|-------------|--------|
|172.16.0.0/16|Local|
|10.0.0.0/16|VPC Peering Connection|

Without these routes, communication will fail.

---

# Security Groups

Even after creating the peering connection:

Security Groups must still allow the traffic.

Example:

Application Server

↓

Database

Security Group Rule:

Allow MySQL

Port:

3306

Source:

10.0.0.0/16

or

Reference the Security Group (same-region, where supported).

---

# CIDR Block Restriction

One of the biggest limitations:

Overlapping CIDR blocks are NOT allowed.

Example:

VPC A

10.0.0.0/16

VPC B

10.0.0.0/16

❌ Peering cannot be established.

Correct Example:

VPC A

10.0.0.0/16

VPC B

172.16.0.0/16

✅ Works perfectly.

---

# VPC Peering is Non-Transitive

This is an extremely common interview question.

Example:

VPC A

↓

Peering

↓

VPC B

↓

Peering

↓

VPC C

Can VPC A communicate with VPC C?

❌ No.

AWS does NOT forward traffic through an intermediate VPC.

Each required connection must be created explicitly.

---

# Real-World Example

Production VPC

↓

Peering

↓

Shared Services VPC

↓

Logging Server

↓

Monitoring Tools

Application servers access shared services privately.

---

# VPC Peering vs Internet Gateway

| Feature | VPC Peering | Internet Gateway |
|---------|-------------|------------------|
|Private Communication|Yes|No|
|Uses Public Internet|No|Yes|
|Between VPCs|Yes|No|
|Public Access|No|Yes|

---

# VPC Peering vs VPN

| Feature | VPC Peering | Site-to-Site VPN |
|---------|-------------|------------------|
|Encryption Over Internet|No (AWS backbone)|Yes|
|Between AWS VPCs|Yes|Can connect AWS to on-premises or AWS|
|Performance|Generally lower latency|Depends on internet connection|

---

# Advantages

- Private communication.
- High bandwidth.
- Low latency.
- No internet exposure.
- Simple to configure.
- Uses AWS backbone network.

---

# Limitations

- No overlapping CIDR blocks.
- Non-transitive routing.
- Route Tables must be updated manually.
- Security Groups and Network ACLs must still allow traffic.
- Managing many peerings becomes complex.

---

# Best Practices

- Plan CIDR blocks before creating VPCs.
- Use meaningful route entries.
- Apply least-privilege Security Group rules.
- Monitor traffic using VPC Flow Logs.
- Consider AWS Transit Gateway for large environments.

---

# Common Mistakes

❌ Forgetting Route Table updates.

❌ Overlapping CIDR blocks.

❌ Assuming peering is transitive.

❌ Forgetting Security Group rules.

❌ Expecting internet access through another VPC.

---

# Troubleshooting

Cannot communicate between VPCs?

Check:

✅ Peering connection status is Active.

✅ Route Tables updated on both VPCs.

✅ CIDR blocks do not overlap.

✅ Security Groups allow traffic.

✅ Network ACLs allow traffic.

---

# Interview Questions

### What is VPC Peering?

VPC Peering is a private networking connection that allows two VPCs to communicate using private IP addresses.

---

### Does VPC Peering use the public internet?

No.

Traffic stays on AWS's private global network.

---

### Can VPCs with overlapping CIDR blocks be peered?

No.

Overlapping CIDR blocks are not supported.

---

### Is VPC Peering transitive?

No.

If VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A cannot automatically communicate with VPC C.

---

### Do Route Tables need to be updated?

Yes.

Each VPC must have routes pointing to the VPC Peering Connection.

---

### Can VPC Peering connect VPCs in different AWS accounts?

Yes.

As long as the peering request is accepted and routing/security are configured correctly.

---

# Key Takeaways

- VPC Peering provides private communication between two VPCs.
- Traffic stays on AWS's private backbone.
- Route Tables must be updated on both sides.
- Overlapping CIDR blocks are not allowed.
- VPC Peering is non-transitive.
- It is ideal for connecting a small number of VPCs.
