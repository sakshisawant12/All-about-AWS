# Amazon VPC - AWS Transit Gateway (TGW)

## What is AWS Transit Gateway?

AWS Transit Gateway (TGW) is a fully managed network transit hub that connects multiple VPCs, VPNs, and Direct Connect connections through a single central gateway.

Instead of creating many VPC Peering connections, all networks connect to the Transit Gateway.

Think of it as a **central router** for your AWS network.

---

# Why Do We Need Transit Gateway?

Imagine a company with:

- Production VPC
- Development VPC
- Testing VPC
- Shared Services VPC
- Logging VPC

Using VPC Peering, every VPC must connect to every other VPC.

This becomes difficult to manage.

Transit Gateway simplifies this architecture.

---

# Problem with VPC Peering

Example:

VPC A

↓

VPC B

↓

VPC C

↓

VPC D

Every VPC requires separate peering connections.

As the number of VPCs grows, the number of connections increases rapidly.

This creates a complex mesh network.

---

# Solution: Transit Gateway

Instead of connecting every VPC together:

               Transit Gateway
                      │
      ┌───────────────┼───────────────┐
      │               │               │
   VPC-A           VPC-B          VPC-C
      │                               │
   VPN                           Direct Connect

Every network connects to the Transit Gateway.

No direct VPC Peering is required.

---

# How Transit Gateway Works

Traffic Flow:

EC2

↓

VPC Attachment

↓

Transit Gateway

↓

Destination VPC

↓

EC2

The Transit Gateway acts as a central routing hub.

---

# Transit Gateway Attachments

Resources connect to the Transit Gateway using attachments.

Supported attachments:

- VPC Attachment
- Site-to-Site VPN
- Direct Connect Gateway
- Connect Attachment (for SD-WAN and supported network appliances)

---

# Transit Gateway Route Tables

Transit Gateway has its own Route Tables.

These determine where traffic should be forwarded.

Example:

Destination

10.1.0.0/16

↓

Production VPC

Destination

10.2.0.0/16

↓

Development VPC

---

# Example Architecture

                 On-Premises
                       │
                Direct Connect
                       │
               Transit Gateway
         ┌────────┼────────┐
         │        │        │
      Prod VPC Dev VPC Test VPC

Every VPC communicates through the Transit Gateway.

---

# Hub-and-Spoke Architecture

Transit Gateway follows the Hub-and-Spoke model.

                Transit Gateway
                 /      |      \
                /       |       \
           Prod      Dev      Test

Benefits:

- Easier management
- Better scalability
- Fewer network connections
- Centralized routing

---

# Transit Gateway vs VPC Peering

| Feature | Transit Gateway | VPC Peering |
|---------|-----------------|-------------|
|Scalability|High|Limited|
|Central Hub|Yes|No|
|Supports Many VPCs|Yes|Less suitable as environments grow|
|Supports VPN|Yes|No|
|Supports Direct Connect|Yes|No|
|Management|Centralized|Distributed|

---

# Transit Gateway vs Virtual Private Gateway

| Feature | Transit Gateway | Virtual Private Gateway |
|---------|-----------------|--------------------------|
|Connect Multiple VPCs|Yes|No|
|Connect Multiple VPNs|Yes|Limited|
|Enterprise Networking|Yes|Basic Hybrid Connectivity|
|Scalability|High|Lower|

---

# Benefits

- Centralized routing
- Easy expansion
- Hybrid Cloud support
- Reduced network complexity
- Supports multi-account environments (using AWS Resource Access Manager where appropriate)
- High availability

---

# Best Practices

- Use separate Transit Gateway Route Tables for different environments.
- Plan CIDR blocks carefully to avoid overlaps.
- Monitor traffic using VPC Flow Logs and CloudWatch.
- Use AWS Resource Access Manager (RAM) for sharing Transit Gateways across accounts.
- Apply least-privilege routing.

---

# Common Mistakes

❌ Overlapping CIDR blocks.

❌ Incorrect Transit Gateway Route Tables.

❌ Forgetting attachment associations.

❌ Assuming Transit Gateway automatically routes all traffic.

❌ Not planning for future VPC growth.

---

# Troubleshooting

Cannot communicate between VPCs?

Check:

✅ Transit Gateway Attachment.

✅ Transit Gateway Route Table.

✅ VPC Route Tables.

✅ Security Groups.

✅ Network ACLs.

---

# Interview Questions

### What is AWS Transit Gateway?

AWS Transit Gateway is a managed network hub that connects multiple VPCs, VPNs, and Direct Connect connections through a central gateway.

---

### Why is Transit Gateway better than VPC Peering for large environments?

It reduces the number of network connections and centralizes routing, making large environments much easier to manage.

---

### Can Transit Gateway connect to Direct Connect?

Yes.

Through a Direct Connect Gateway.

---

### Can Transit Gateway connect to Site-to-Site VPN?

Yes.

Transit Gateway supports VPN attachments.

---

### Does Transit Gateway replace VPC Peering?

Not completely.

VPC Peering is still a good option for connecting a small number of VPCs, while Transit Gateway is better suited for large, complex environments.

---

### What architecture does Transit Gateway use?

Hub-and-Spoke.

---

# Key Takeaways

- Transit Gateway is a central networking hub.
- It connects multiple VPCs, VPNs, and Direct Connect connections.
- It simplifies enterprise networking.
- It scales much better than many VPC Peering connections.
- It uses attachments and Transit Gateway Route Tables for routing.
- It is ideal for large AWS and Hybrid Cloud environments.
