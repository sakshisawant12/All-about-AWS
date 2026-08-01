# CIDR and IP Addressing in AWS

## What is an IP Address?

An IP (Internet Protocol) address is a unique numerical identifier assigned to a device on a network. It allows devices to communicate with each other.

Example:

192.168.1.10

Think of an IP address like a home address. Just as a courier uses your address to deliver a package, the internet uses IP addresses to deliver data to the correct device.

---

## Types of IP Addresses

### IPv4

IPv4 is a 32-bit address represented in dotted decimal notation.

Example:

192.168.10.15

Total addresses:

2³² = 4,294,967,296

Example:

172.31.25.100

---

### IPv6

IPv6 is a 128-bit address created to solve IPv4 exhaustion.

Example:

2001:db8:85a3::8a2e:370:7334

Advantages:

- Huge address space
- Better routing
- Improved security
- Efficient communication

AWS VPC supports both IPv4 and IPv6.

---

## Public IP Address

A Public IP is reachable over the internet.

Example:

54.201.10.15

Characteristics:

- Globally unique
- Internet accessible
- Assigned by AWS (unless using Elastic IP)

Use Cases:

- Web Servers
- Bastion Hosts
- Public APIs

---

## Private IP Address

Private IPs are used only inside private networks.

These IPs cannot be accessed directly from the internet.

AWS Resources communicate internally using private IP addresses.

Example:

10.0.1.25

---

## Private IP Ranges

AWS follows RFC 1918 private address ranges.

| Range | CIDR |
|--------|------|
|10.0.0.0 - 10.255.255.255|10.0.0.0/8|
|172.16.0.0 - 172.31.255.255|172.16.0.0/12|
|192.168.0.0 - 192.168.255.255|192.168.0.0/16|

These ranges are commonly used while creating VPCs.

---

## What is CIDR?

CIDR stands for

Classless Inter-Domain Routing

CIDR defines:

- Network portion
- Host portion

Example:

10.0.0.0/16

Here,

10.0 = Network

Remaining bits = Hosts

---

## CIDR Syntax

Example:

192.168.1.0/24

24 indicates:

24 bits represent Network

Remaining

32 - 24 = 8 bits

represent Hosts.

---

## Common CIDR Blocks

|CIDR|Total IPs|Usable in AWS|
|----|---------|-------------|
|/16|65,536|65,531|
|/17|32,768|32,763|
|/18|16,384|16,379|
|/19|8,192|8,187|
|/20|4,096|4,091|
|/21|2,048|2,043|
|/22|1,024|1,019|
|/23|512|507|
|/24|256|251|
|/25|128|123|
|/26|64|59|
|/27|32|27|
|/28|16|11|

AWS reserves 5 IP addresses in every subnet.

---

## AWS Reserved IP Addresses

Suppose subnet:

10.0.1.0/24

AWS reserves:

10.0.1.0 → Network Address

10.0.1.1 → VPC Router

10.0.1.2 → Amazon DNS

10.0.1.3 → Future Use

10.0.1.255 → Broadcast Reserved

Available:

10.0.1.4

to

10.0.1.254

except reserved addresses.

---

## Example

VPC

10.0.0.0/16

Possible Subnets

Public

10.0.1.0/24

Private

10.0.2.0/24

Database

10.0.3.0/24

Management

10.0.4.0/24

Each subnet contains 251 usable IPs.

---

## Public vs Private IP

|Feature|Public|Private|
|-------|------|--------|
|Internet Accessible|Yes|No|
|Globally Unique|Yes|No|
|Inside VPC|Yes|Yes|
|Used for Internal Communication|No|Yes|
|Can Communicate Over Internet|Yes|No|

---

## CIDR Planning Example

Company Requirement:

Web Servers

100 IPs

Application Servers

80 IPs

Database

20 IPs

Possible Design

VPC

10.0.0.0/16

Web

10.0.1.0/24

Application

10.0.2.0/24

Database

10.0.3.0/27

Future Expansion

10.0.4.0/24 onwards

Always leave room for future growth.

---

## Best Practices

- Plan CIDR carefully before creating a VPC.
- Avoid overlapping CIDR blocks between VPCs.
- Leave unused address space for future expansion.
- Use smaller subnets for databases and internal services.
- Use meaningful subnet allocation.

---

## Common Mistakes

❌ Creating overlapping CIDRs

❌ Using very small CIDR blocks

❌ Forgetting AWS reserves 5 IPs

❌ Poor subnet planning

❌ Running out of IP addresses

---

## Interview Questions

### What is CIDR?

CIDR (Classless Inter-Domain Routing) is a method of allocating IP address ranges using prefix notation.

---

### Why do we use CIDR?

It helps divide networks efficiently, reduces IP wastage, and allows flexible subnetting.

---

### How many IP addresses are available in a /24 subnet?

256 total IPs.

AWS reserves 5 IPs.

Usable = 251.

---

### Which private IP ranges can be used in AWS?

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

---

### Can two VPCs have the same CIDR?

Yes, but they cannot communicate directly through VPC Peering because overlapping CIDRs are not supported.

---

## Key Takeaways

- Every device needs an IP address to communicate.
- CIDR defines the size of a network.
- AWS reserves 5 IP addresses in every subnet.
- Private IPs are used for internal communication.
- Public IPs enable internet access.
- Proper CIDR planning is essential for scalable AWS network design.
