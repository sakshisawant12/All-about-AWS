# Amazon VPC - Subnets

## What is a Subnet?

A subnet (Subnetwork) is a smaller network created by dividing a VPC into multiple sections.

It allows you to organize and isolate AWS resources within a VPC.

Every subnet gets a portion of the VPC's CIDR block.

Example:

VPC CIDR:
10.0.0.0/16

Possible Subnets:

10.0.1.0/24

10.0.2.0/24

10.0.3.0/24

10.0.4.0/24

Each subnet belongs to the same VPC but is treated as an independent network.

---

## Why Do We Need Subnets?

Without subnets, every resource would be in the same network.

Subnets help to:

- Improve security
- Separate application layers
- Control network traffic
- Organize resources
- Increase availability

---

## Real-World Example

Imagine a company building an e-commerce application.

The application has:

- Web Server
- Application Server
- Database Server

A secure architecture would be:

Internet

↓

Public Subnet

↓

Web Server

↓

Private Subnet

↓

Application Server

↓

Private Subnet

↓

Database

This prevents direct internet access to sensitive resources.

---

## Public Subnet

A Public Subnet is a subnet that has a route to an Internet Gateway (IGW).

Resources inside a public subnet can communicate with the internet if they have:

- Public IP or Elastic IP
- Route to Internet Gateway
- Security Group allows traffic

Common Resources:

- Web Servers
- Bastion Hosts
- Load Balancers
- NAT Gateway

Example:

10.0.1.0/24

---

## Private Subnet

A Private Subnet does NOT have a direct route to the Internet Gateway.

Resources cannot be accessed directly from the internet.

If outbound internet access is needed (for updates, package installation, etc.), traffic is routed through a NAT Gateway.

Common Resources:

- Databases
- Application Servers
- Internal Services
- Backend APIs

Example:

10.0.2.0/24

---

## Public vs Private Subnet

| Feature | Public Subnet | Private Subnet |
|---------|---------------|----------------|
| Internet Gateway Route | Yes | No |
| Internet Accessible | Yes | No |
| Public IP | Usually | No |
| Elastic IP | Yes | No |
| Database Deployment | No | Yes |
| Web Server | Yes | No |
| Application Server | Possible | Yes |

---

## How Does AWS Decide Whether a Subnet is Public or Private?

AWS does NOT have a "Public Subnet" or "Private Subnet" option.

The subnet type depends entirely on its Route Table.

If the Route Table contains:

0.0.0.0/0 → Internet Gateway

the subnet is Public.

If it does not have this route, it is Private.

---

## Subnets and Availability Zones

A subnet can exist in ONLY ONE Availability Zone.

Example:

Mumbai Region (ap-south-1)

Availability Zones:

ap-south-1a

ap-south-1b

ap-south-1c

Example Design:

Public Subnet A → ap-south-1a

Private Subnet A → ap-south-1a

Public Subnet B → ap-south-1b

Private Subnet B → ap-south-1b

This provides High Availability.

---

## Why Can't a Subnet Span Multiple Availability Zones?

AWS designs each subnet within a single Availability Zone to ensure:

- Fault isolation
- Low latency
- High availability
- Independent infrastructure

If one AZ fails, the others continue running.

---

## Multi-AZ Architecture

                Region
                   │
      ┌────────────┴────────────┐
      │                         │
 Availability Zone A      Availability Zone B
      │                         │
 Public Subnet A          Public Subnet B
 Private Subnet A         Private Subnet B

Applications are deployed across multiple AZs to improve reliability.

---

## Communication Between Subnets

Resources in different subnets can communicate if:

- They are in the same VPC.
- Route Tables allow communication.
- Security Groups permit traffic.
- Network ACLs permit traffic.

Example:

Web Server (Public Subnet)

↓

Application Server (Private Subnet)

↓

Database (Private Subnet)

---

## AWS Reserved IP Addresses

Every subnet reserves five IP addresses.

Example:

Subnet:

10.0.1.0/24

Reserved:

10.0.1.0

10.0.1.1

10.0.1.2

10.0.1.3

10.0.1.255

Usable:

10.0.1.4

to

10.0.1.254

---

## Example VPC Design

VPC

10.0.0.0/16

Public Subnet

10.0.1.0/24

Private Subnet

10.0.2.0/24

Database Subnet

10.0.3.0/24

Management Subnet

10.0.4.0/24

---

## Best Practices

- Keep databases in private subnets.
- Use public subnets only for internet-facing resources.
- Create subnets in multiple Availability Zones.
- Leave room for future subnet expansion.
- Follow a clear IP addressing strategy.
- Avoid placing sensitive workloads in public subnets.

---

## Common Mistakes

❌ Assuming assigning a Public IP automatically makes a subnet public.

❌ Forgetting to associate the correct Route Table.

❌ Deploying databases in public subnets.

❌ Using only one Availability Zone.

❌ Poor subnet planning.

---

## Interview Questions

### What is a subnet?

A subnet is a smaller network created from a VPC's CIDR block to organize and isolate resources.

---

### What makes a subnet public?

A subnet becomes public when its Route Table contains a route to an Internet Gateway (0.0.0.0/0 → IGW).

---

### What makes a subnet private?

A subnet is private when it does not have a route to an Internet Gateway.

---

### Can a subnet span multiple Availability Zones?

No. A subnet belongs to only one Availability Zone.

---

### Can resources in different subnets communicate?

Yes, if they are in the same VPC and routing, Security Groups, and Network ACLs allow the traffic.

---

## Key Takeaways

- A subnet is a smaller network inside a VPC.
- Public and private subnets are determined by their Route Tables.
- Each subnet belongs to a single Availability Zone.
- Public subnets host internet-facing resources.
- Private subnets host secure internal resources.
- Using multiple Availability Zones improves availability and fault tolerance.
