# Amazon VPC - Route Tables

## What is a Route Table?

A Route Table is a set of rules (called routes) that determines where network traffic from a subnet or gateway should be sent.

Think of it as a GPS or Google Maps for your network. When a packet leaves an EC2 instance, the Route Table decides its next destination.

Without a Route Table, AWS resources would not know where to send traffic.

---

## Why Do We Need Route Tables?

Route Tables help:

- Control network traffic
- Enable internet access
- Enable communication between subnets
- Connect VPCs
- Connect on-premises networks
- Control traffic flow securely

---

## How Does Routing Work?

Suppose an EC2 instance in a subnet wants to communicate with another device.

The packet follows these steps:

1. EC2 sends the packet.
2. AWS checks the subnet's Route Table.
3. AWS finds the best matching route.
4. The packet is forwarded to the destination.

---

## Components of a Route

Every route has two parts:

### Destination

The IP address or CIDR block where traffic is going.

Examples:

10.0.0.0/16

172.31.0.0/16

192.168.1.0/24

0.0.0.0/0

---

### Target

The resource that receives the traffic.

Examples:

- Local
- Internet Gateway (IGW)
- NAT Gateway
- VPC Peering Connection
- Transit Gateway
- Virtual Private Gateway
- Gateway Endpoint
- Network Interface (ENI)

---

## Example Route Table

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

Meaning:

Traffic inside the VPC stays inside.

Everything else goes to the Internet Gateway.

---

## Local Route

Every Route Table automatically contains a Local Route.

Example:

Destination

10.0.0.0/16

Target

Local

Purpose:

Allows communication between all subnets inside the same VPC.

Without this route:

Resources inside the same VPC could not communicate.

This route cannot be deleted.

---

## Default Route (0.0.0.0/0)

0.0.0.0/0 means:

"Any IPv4 address."

If traffic doesn't match a more specific route, AWS uses the default route.

Example:

Destination

0.0.0.0/0

Target

Internet Gateway

Meaning:

Send all internet traffic to the Internet Gateway.

For IPv6:

::/0

---

## Route Table Example

VPC

10.0.0.0/16

Route Table

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

An EC2 instance accessing www.google.com will match the default route and the packet will be sent to the Internet Gateway.

---

## Main Route Table

Every VPC has one Main Route Table.

Characteristics:

- Created automatically
- Every subnet uses it unless another Route Table is associated
- Contains the Local Route by default

---

## Custom Route Table

You can create additional Route Tables.

Benefits:

- Different routing rules
- Better network segmentation
- Separate public and private traffic
- Easier management

---

## Route Table Association

A Route Table only affects the subnets associated with it.

Example:

Public Subnet

↓

Public Route Table

↓

Internet Gateway

Private Subnet

↓

Private Route Table

↓

NAT Gateway

---

## Public Route Table

Example:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

Result:

Resources can access the internet (if they also have a Public IP or Elastic IP).

---

## Private Route Table

Example:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|NAT Gateway|

Result:

Resources can access the internet for outbound traffic but cannot receive inbound internet connections.

---

## Longest Prefix Match

AWS always selects the most specific route.

Example:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|10.0.2.0/24|VPC Peering|
|0.0.0.0/0|Internet Gateway|

Traffic to:

10.0.2.25

matches

10.0.2.0/24

because it is more specific than

10.0.0.0/16.

---

## Route Priority

AWS checks:

1. Longest Prefix Match
2. Most specific route wins

Example:

10.0.0.0/16

10.0.1.0/24

Traffic for:

10.0.1.50

uses

10.0.1.0/24

instead of

10.0.0.0/16.

---

## Real-World Example

E-commerce Website

Public Subnet

↓

Web Server

↓

Public Route Table

↓

Internet Gateway

Private Subnet

↓

Application Server

↓

Private Route Table

↓

NAT Gateway

↓

Internet

Database

↓

Private Route Table

↓

No Internet Gateway

This keeps the database isolated while allowing application servers to download updates through the NAT Gateway.

---

## Best Practices

- Use separate Route Tables for public and private subnets.
- Keep the Local Route unchanged.
- Use the default route only when necessary.
- Design routes with future expansion in mind.
- Review routing after infrastructure changes.

---

## Common Mistakes

❌ Forgetting to associate the correct Route Table.

❌ Assuming all subnets share the same routes.

❌ Deleting or modifying required routes.

❌ Routing private subnets directly to an Internet Gateway.

❌ Forgetting that Route Tables alone do not provide internet access.

---

## Interview Questions

### What is a Route Table?

A Route Table is a collection of routing rules that determines where network traffic is sent.

---

### Can a subnet have multiple Route Tables?

No.

A subnet can be associated with only one Route Table at a time.

---

### Can one Route Table be associated with multiple subnets?

Yes.

Multiple subnets can share the same Route Table.

---

### What is the Local Route?

The Local Route allows communication between resources inside the same VPC. It is created automatically and cannot be removed.

---

### What is 0.0.0.0/0?

It is the default route that represents all IPv4 addresses. Traffic that does not match a more specific route follows this path.

---

### Does a Route Table alone make a subnet public?

No.

A subnet is considered public only if:

- Its Route Table has a route to an Internet Gateway.
- The EC2 instance has a Public IP or Elastic IP.
- Security Groups and Network ACLs allow the traffic.

---

## Key Takeaways

- A Route Table decides where network traffic goes.
- Every VPC has a Main Route Table.
- Every Route Table contains a Local Route.
- Public subnets use an Internet Gateway.
- Private subnets usually use a NAT Gateway.
- AWS always chooses the most specific matching route (Longest Prefix Match).
- Route Tables are associated with subnets, not individual EC2 instances.
