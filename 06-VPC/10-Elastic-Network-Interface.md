# Amazon VPC - Elastic Network Interface (ENI)

## What is an Elastic Network Interface (ENI)?

An Elastic Network Interface (ENI) is a virtual network card that connects an AWS resource, such as an EC2 instance, to a VPC network.

Just like a physical computer uses a Network Interface Card (NIC) to connect to a network, an EC2 instance uses an ENI.

Every EC2 instance must have at least one ENI.

---

# Why Do We Need an ENI?

An ENI provides:

- Network connectivity
- Private IP addresses
- Public or Elastic IP association
- Security Groups
- MAC Address
- Network traffic management

Without an ENI, an EC2 instance cannot communicate on the network.

---

# Components of an ENI

An ENI contains:

- Primary Private IP
- Secondary Private IP(s)
- Public IP (optional)
- Elastic IP (optional)
- MAC Address
- Security Groups
- Source/Destination Check setting

Think of the ENI as a container that holds all the networking configuration for an EC2 instance.

---

# Primary ENI

When you launch an EC2 instance, AWS automatically creates a Primary ENI.

Characteristics:

- Created automatically
- Cannot be detached while the instance is running
- Contains the primary private IP
- Used for the main network connection

Example:

EC2

↓

Primary ENI

↓

Private IP

10.0.1.25

---

# Secondary ENIs

You can manually create and attach additional ENIs to supported EC2 instances.

Use Cases:

- Multi-network applications
- Network appliances
- High Availability
- Traffic separation
- Failover

Example:

EC2

↓

Primary ENI

↓

Application Traffic

+

Secondary ENI

↓

Management Traffic

---

# Private IP Addresses

Each ENI has:

One Primary Private IP

Example:

10.0.1.20

It can also have multiple Secondary Private IPs.

Example:

10.0.1.21

10.0.1.22

10.0.1.23

Applications can bind to different private IPs on the same ENI.

---

# Public IP and Elastic IP

A Public IP or Elastic IP is associated with an ENI, not directly with the EC2 instance.

Example:

Elastic IP

54.210.15.20

↓

ENI

↓

EC2

If the ENI moves to another compatible instance, the associated Elastic IP moves with it.

---

# Security Groups

Security Groups are attached to ENIs.

This means different ENIs on the same EC2 instance can have different Security Groups.

Example:

ENI-1

SSH + HTTP

ENI-2

Database Traffic

This provides flexible traffic control.

---

# Source/Destination Check

By default, AWS checks that traffic sent or received by an ENI belongs to that instance.

This is called **Source/Destination Check**.

For certain networking appliances, such as:

- NAT Instance
- Firewall
- Router

this check must be disabled because these instances forward traffic for other resources.

---

# ENI Lifecycle

An ENI can be:

- Created
- Attached
- Detached (secondary ENIs only)
- Reattached to another compatible instance
- Deleted

The Primary ENI cannot normally be detached while the instance is running.

---

# High Availability Example

Suppose:

Server A fails.

Instead of changing IP addresses:

Detach Secondary ENI

↓

Attach it to Server B

↓

Applications continue using the same network configuration.

This reduces downtime.

---

# Example Architecture

                VPC
                 │
         ┌───────┴────────┐
         │                │
      EC2 Instance
         │
   ┌─────┴─────┐
   │           │
Primary ENI   Secondary ENI
   │           │
App Traffic  Admin Traffic

---

# ENI vs Security Group

| Feature | ENI | Security Group |
|---------|-----|----------------|
|Purpose|Network Interface|Firewall|
|Stores IP Address|Yes|No|
|Stores MAC Address|Yes|No|
|Attached To|EC2|ENI|
|Controls Traffic|No|Yes|

---

# ENI vs Elastic IP

| Feature | ENI | Elastic IP |
|---------|-----|------------|
|Virtual Network Card|Yes|No|
|Contains Private IP|Yes|No|
|Contains Security Groups|Yes|No|
|Static Public IP|No|Yes|
|Can Be Associated Together|Yes|Yes|

---

# Best Practices

- Keep the Primary ENI for normal application traffic.
- Use Secondary ENIs only when required.
- Use Security Groups on ENIs for fine-grained access control.
- Disable Source/Destination Check only for supported networking appliances.
- Avoid unnecessary ENIs to simplify management.

---

# Common Mistakes

❌ Confusing an ENI with an Elastic IP.

❌ Assuming Security Groups attach directly to EC2 instances.

❌ Forgetting that Security Groups are attached to ENIs.

❌ Disabling Source/Destination Check unnecessarily.

❌ Assuming the Primary ENI can be detached like a Secondary ENI.

---

# Troubleshooting

Cannot communicate over the network?

Check:

✅ ENI is attached.

✅ Correct subnet.

✅ Security Group rules.

✅ Route Table.

✅ Network ACL.

✅ Source/Destination Check (if acting as a router or NAT instance).

---

# Interview Questions

### What is an ENI?

An Elastic Network Interface is a virtual network card that provides network connectivity to AWS resources within a VPC.

---

### Can an EC2 instance have multiple ENIs?

Yes.

Supported instance types can have multiple ENIs attached.

---

### Can an ENI have multiple private IP addresses?

Yes.

An ENI has one Primary Private IP and can have multiple Secondary Private IPs.

---

### Are Security Groups attached to the EC2 instance?

No.

Security Groups are attached to the ENI.

---

### Can an Elastic IP be associated with an ENI?

Yes.

Elastic IPs are associated with ENIs, not directly with EC2 instances.

---

### What is Source/Destination Check?

It is a security feature that ensures an instance only sends or receives traffic intended for itself. It must be disabled for instances acting as routers, NAT instances, or firewalls.

---

# Key Takeaways

- An ENI is a virtual network interface.
- Every EC2 instance has at least one Primary ENI.
- Secondary ENIs can be attached for advanced networking.
- ENIs contain IP addresses, MAC addresses, and Security Groups.
- Elastic IPs are associated with ENIs.
- Source/Destination Check is enabled by default and should only be disabled for specific networking use cases.
