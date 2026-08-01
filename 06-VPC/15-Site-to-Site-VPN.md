# Amazon VPC - AWS Site-to-Site VPN

## What is AWS Site-to-Site VPN?

AWS Site-to-Site VPN is a managed service that creates a secure, encrypted connection between an on-premises network (or another supported network) and an AWS VPC over the public internet.

It enables Hybrid Cloud by allowing on-premises resources and AWS resources to communicate securely.

---

# Why Do We Need Site-to-Site VPN?

Many organizations already have:

- Corporate Data Centers
- Branch Offices
- Existing Office Networks

Instead of moving everything to AWS immediately, they connect their existing network to AWS.

This allows applications and users to access AWS resources privately.

---

# Real-World Example

Company Office

↓

Router / Firewall

↓

Internet

↓

AWS Site-to-Site VPN

↓

Amazon VPC

↓

Private EC2

↓

RDS

Employees in the office can access AWS resources using private IP addresses.

---

# How Site-to-Site VPN Works

Traffic Flow:

Office Network

↓

Customer Gateway (CGW)

↓

Encrypted VPN Tunnel

↓

Virtual Private Gateway (VGW)

↓

Amazon VPC

↓

EC2

All traffic is encrypted while traveling across the public internet.

---

# Main Components

## Customer Gateway (CGW)

The Customer Gateway represents your on-premises VPN device.

It can be:

- Physical Router
- Firewall
- VPN Appliance

Examples:

- Cisco
- Fortinet
- Palo Alto
- Juniper

The Customer Gateway initiates the VPN connection.

---

## Virtual Private Gateway (VGW)

The Virtual Private Gateway is the AWS-side VPN endpoint.

It attaches to a VPC and receives encrypted traffic from the Customer Gateway.

Think of it as the AWS router for the VPN connection.

---

## VPN Connection

The VPN Connection links the Customer Gateway and the Virtual Private Gateway.

AWS automatically creates **two VPN tunnels** for high availability.

---

# High Availability

AWS creates two IPsec tunnels.

Example:

Office

↓

Tunnel 1

↓

AWS

Office

↓

Tunnel 2

↓

AWS

If one tunnel fails, traffic automatically switches to the second tunnel.

This provides redundancy.

---

# Static Routing

Routes are manually configured on both sides.

Suitable for:

- Small environments
- Stable networks

---

# Dynamic Routing (BGP)

AWS supports **Border Gateway Protocol (BGP)** for dynamic route exchange.

Benefits:

- Automatic route updates
- Better failover
- Easier management
- Preferred for enterprise environments

---

# Example Architecture

           Office Network
                  │
         Customer Gateway
                  │
        ==================
         Tunnel 1  Tunnel 2
        ==================
                  │
      Virtual Private Gateway
                  │
                Amazon VPC
                  │
             Private EC2

---

# Site-to-Site VPN vs Client VPN

| Feature | Site-to-Site VPN | Client VPN |
|---------|------------------|------------|
|Connects|Entire Office Network|Individual User|
|Requires VPN Client|No|Yes|
|Authentication|VPN Device|User|
|Typical Use|Hybrid Cloud|Remote Employees|

---

# Site-to-Site VPN vs VPC Peering

| Feature | Site-to-Site VPN | VPC Peering |
|---------|------------------|-------------|
|Uses Internet|Yes (Encrypted)|No (AWS Backbone)|
|Connects|On-premises ↔ AWS|AWS VPC ↔ AWS VPC|
|Encryption|Yes|Traffic stays on AWS private network|

---

# Site-to-Site VPN vs Direct Connect

| Feature | Site-to-Site VPN | Direct Connect |
|---------|------------------|----------------|
|Connection Type|Public Internet|Dedicated Private Link|
|Encryption|Yes|Not by default|
|Speed|Depends on Internet|Consistent High Speed|
|Cost|Lower|Higher|
|Setup Time|Fast|Longer|

---

# Benefits

- Secure encrypted communication
- Supports Hybrid Cloud
- Two VPN tunnels for redundancy
- Easy to deploy
- Lower cost than Direct Connect
- Supports static and dynamic routing

---

# Best Practices

- Use BGP whenever possible.
- Monitor tunnel health with CloudWatch.
- Configure both tunnels for redundancy.
- Secure Customer Gateway devices.
- Restrict access using Security Groups and Network ACLs.

---

# Common Mistakes

❌ Configuring only one VPN tunnel.

❌ Incorrect route propagation.

❌ Forgetting Security Group rules.

❌ Incorrect Customer Gateway configuration.

❌ Assuming VPN provides faster performance than Direct Connect.

---

# Troubleshooting

VPN not connecting?

Check:

✅ Customer Gateway public IP.

✅ Shared secret (pre-shared key).

✅ BGP configuration (if used).

✅ Virtual Private Gateway attached to the VPC.

✅ Route Tables.

✅ Firewall allows IPsec traffic.

---

# Interview Questions

### What is AWS Site-to-Site VPN?

AWS Site-to-Site VPN is a managed service that securely connects an on-premises network to an AWS VPC using encrypted IPsec tunnels.

---

### What is a Customer Gateway?

A Customer Gateway is the on-premises VPN device or its AWS representation that establishes the VPN connection.

---

### What is a Virtual Private Gateway?

A Virtual Private Gateway is the AWS-side VPN endpoint attached to a VPC.

---

### How many VPN tunnels does AWS create?

Two tunnels for high availability and failover.

---

### Does Site-to-Site VPN use the public internet?

Yes.

However, all traffic is encrypted using IPsec.

---

### Which routing protocols are supported?

- Static Routing
- Dynamic Routing using BGP

---

# Key Takeaways

- Site-to-Site VPN securely connects on-premises networks to AWS.
- It enables Hybrid Cloud architectures.
- AWS creates two VPN tunnels for redundancy.
- Customer Gateway is on-premises; Virtual Private Gateway is on AWS.
- Supports static routes and BGP.
- Lower cost than Direct Connect but depends on internet performance.
