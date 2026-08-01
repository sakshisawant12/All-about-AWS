# Amazon VPC - Internet Gateway (IGW)

## What is an Internet Gateway?

An Internet Gateway (IGW) is a highly available and fully managed AWS service that allows communication between a VPC and the public internet.

It acts as the bridge between your private AWS network and the internet.

Without an Internet Gateway, resources inside a VPC cannot communicate directly with the internet.

---

## Why Do We Need an Internet Gateway?

An Internet Gateway enables:

- Outbound internet access from EC2 instances
- Inbound internet access to public resources
- Communication with websites and public APIs
- Hosting public-facing applications

Examples:

- Web Servers
- Public APIs
- Bastion Hosts
- Application Load Balancers

---

## How Does an Internet Gateway Work?

Example:

Internet

↓

Internet Gateway

↓

Route Table

↓

Public Subnet

↓

EC2 Instance

Traffic Flow:

1. User opens your website.
2. Request reaches the Internet Gateway.
3. The Internet Gateway forwards the request into the VPC.
4. The Route Table sends the traffic to the correct subnet.
5. The EC2 instance processes the request.
6. The response follows the same path back to the user.

---

## Internet Gateway Characteristics

- Fully managed by AWS
- Highly available
- Horizontally scalable
- No bandwidth limits managed by users
- Supports IPv4 and IPv6
- Performs one-to-one NAT for public IPv4 addresses
- Attached to one VPC at a time

---

## Does Attaching an Internet Gateway Automatically Provide Internet Access?

No.

Attaching an Internet Gateway alone is NOT enough.

For internet connectivity, all of the following are required:

- Internet Gateway attached to the VPC
- Route Table contains:
  0.0.0.0/0 → Internet Gateway
- EC2 has a Public IP or Elastic IP
- Security Group allows required traffic
- Network ACL allows required traffic

If any of these are missing, the instance cannot communicate with the internet.

---

## Route Table Configuration

Example Public Route Table:

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

This means:

Internal traffic stays inside the VPC.

All other traffic goes to the Internet Gateway.

---

## Public IP Requirement

Even if an Internet Gateway is attached, an EC2 instance without a Public IP (or Elastic IP) cannot communicate directly with the internet.

Example:

EC2

Private IP:

10.0.1.25

Public IP:

54.210.18.35

The Internet Gateway uses the public IP to communicate with external networks.

---

## Public Subnet Example

VPC

10.0.0.0/16

↓

Public Subnet

10.0.1.0/24

↓

Public Route Table

↓

Internet Gateway

↓

Internet

EC2 instances in this subnet can communicate with the internet if they have a Public IP.

---

## Private Subnet Example

VPC

10.0.0.0/16

↓

Private Subnet

10.0.2.0/24

↓

Private Route Table

↓

No Internet Gateway Route

↓

Internet

Result:

No direct internet access.

---

## Internet Gateway vs NAT Gateway

| Feature | Internet Gateway | NAT Gateway |
|----------|------------------|-------------|
|Inbound Internet Access|Yes|No|
|Outbound Internet Access|Yes|Yes|
|Requires Public IP|Yes|No (for private EC2)|
|Used In|Public Subnets|Public Subnet (serving Private Subnets)|
|Allows Public Access|Yes|No|

---

## Real-World Example

Company Website

Internet

↓

Internet Gateway

↓

Public Subnet

↓

Web Server

↓

Private Subnet

↓

Application Server

↓

Database

Only the web server is directly accessible from the internet.

The application server and database remain protected inside private subnets.

---

## Common Mistakes

❌ Attaching an Internet Gateway but forgetting the Route Table.

❌ Launching an EC2 instance without a Public IP.

❌ Assuming Internet Gateway alone provides internet access.

❌ Deploying databases in public subnets.

❌ Forgetting Security Group rules.

---

## Best Practices

- Keep only internet-facing resources in public subnets.
- Use private subnets for databases and backend services.
- Attach only one Internet Gateway per VPC.
- Use least-privilege Security Group rules.
- Use Elastic IPs only when necessary.

---

## Troubleshooting Internet Connectivity

If an EC2 instance cannot access the internet, check:

✅ Is an Internet Gateway attached to the VPC?

✅ Does the subnet's Route Table contain:
0.0.0.0/0 → Internet Gateway?

✅ Does the EC2 instance have a Public IP or Elastic IP?

✅ Does the Security Group allow outbound traffic?

✅ Does the Security Group allow required inbound traffic (SSH, HTTP, HTTPS)?

✅ Does the Network ACL allow traffic?

✅ Is the instance actually running in a public subnet?

---

## Interview Questions

### What is an Internet Gateway?

An Internet Gateway is a managed AWS component that enables communication between a VPC and the public internet.

---

### Can a VPC have multiple Internet Gateways?

No.

A VPC can have only one attached Internet Gateway at a time.

---

### Does attaching an Internet Gateway make every subnet public?

No.

Only subnets whose Route Tables contain a route to the Internet Gateway are public.

---

### Can a private subnet communicate directly with the Internet Gateway?

No.

Private subnets should use a NAT Gateway for outbound internet access.

---

### Does an EC2 instance need a Public IP to communicate through an Internet Gateway?

Yes.

Without a Public IP or Elastic IP, an EC2 instance cannot communicate directly with the internet through an Internet Gateway.

---

## Key Takeaways

- An Internet Gateway connects a VPC to the internet.
- It is attached at the VPC level.
- Public subnets require a route to the Internet Gateway.
- EC2 instances also need a Public IP or Elastic IP.
- Internet Gateway alone does not provide internet connectivity.
- Security Groups and Network ACLs must also allow the traffic.
