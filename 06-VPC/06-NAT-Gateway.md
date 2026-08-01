# Amazon VPC - NAT Gateway

## What is a NAT Gateway?

A NAT (Network Address Translation) Gateway is a managed AWS service that allows resources in a private subnet to access the internet without exposing them to inbound internet traffic.

It enables **outbound-only internet access** for private resources.

---

## Why Do We Need a NAT Gateway?

Private EC2 instances often need internet access to:

- Install software updates
- Download packages
- Connect to AWS services
- Access external APIs
- Download application dependencies

However, we do not want these servers to be accessible from the internet.

A NAT Gateway solves this problem.

---

## Real-World Example

Suppose you have:

- Web Server (Public)
- Application Server (Private)
- Database (Private)

The Application Server needs to:

- Run `sudo yum update`
- Install Docker
- Download Python packages

Since it has no Public IP, it cannot reach the internet directly.

Instead, traffic flows through the NAT Gateway.

---

## How NAT Gateway Works

Traffic Flow:

Private EC2

↓

Private Route Table

↓

NAT Gateway

↓

Internet Gateway

↓

Internet

Return Traffic

Internet

↓

Internet Gateway

↓

NAT Gateway

↓

Private EC2

The internet can only send responses to requests initiated by the private EC2. It cannot start a new connection to the private EC2.

---

## Where Should a NAT Gateway Be Placed?

A NAT Gateway must always be deployed in a **Public Subnet**.

Why?

Because it needs direct internet connectivity through an Internet Gateway.

---

## Why Does a NAT Gateway Need an Elastic IP?

Private EC2 instances use private IP addresses, which are not routable on the internet.

The NAT Gateway replaces the private source IP with its own **Elastic IP** before sending traffic to the internet.

Example:

Private EC2:

10.0.2.15

↓

NAT Gateway

Elastic IP:

52.66.10.20

↓

Internet

The destination server only sees the Elastic IP of the NAT Gateway.

---

## Route Table Configuration

### Public Route Table

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

---

### Private Route Table

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|NAT Gateway|

Notice that the private subnet does **not** route directly to the Internet Gateway.

---

## Complete Architecture

Internet

↓

Internet Gateway

↓

Public Subnet

↓

NAT Gateway (Elastic IP)

↓

Private Route Table

↓

Private Subnet

↓

Private EC2

The NAT Gateway acts as a bridge between the private subnet and the internet.

---

## Can the Internet Reach the Private EC2?

No.

The NAT Gateway does **not** allow inbound connections initiated from the internet.

Only response traffic for connections started by the private EC2 is allowed.

This is what makes it secure.

---

## NAT Gateway vs Internet Gateway

| Feature | Internet Gateway | NAT Gateway |
|---------|------------------|-------------|
|Connects VPC to Internet|Yes|No (uses IGW)|
|Inbound Internet Access|Yes|No|
|Outbound Internet Access|Yes|Yes|
|Placed In|Attached to VPC|Public Subnet|
|Requires Elastic IP|No|Yes|
|Used By|Public Subnets|Private Subnets|

---

## NAT Gateway vs NAT Instance

| Feature | NAT Gateway | NAT Instance |
|---------|-------------|--------------|
|Managed by AWS|Yes|No|
|High Availability|Yes (within an AZ)|No (user-managed)|
|Scaling|Automatic|Manual|
|Maintenance|AWS|User|
|Performance|High|Depends on EC2 type|

AWS recommends using NAT Gateway instead of NAT Instance for most workloads.

---

## High Availability Best Practice

NAT Gateway is an Availability Zone resource.

If you have private subnets in multiple AZs, create one NAT Gateway **per Availability Zone**.

Example:

AZ-A

- Public Subnet A
- NAT Gateway A
- Private Subnet A

AZ-B

- Public Subnet B
- NAT Gateway B
- Private Subnet B

This avoids a single point of failure.

---

## Common Mistakes

❌ Placing the NAT Gateway in a private subnet.

❌ Forgetting to attach an Internet Gateway to the VPC.

❌ Forgetting to assign an Elastic IP to the NAT Gateway.

❌ Adding the Internet Gateway instead of the NAT Gateway to the private Route Table.

❌ Expecting inbound internet connections to reach private EC2 instances.

---

## Best Practices

- Deploy the NAT Gateway in a public subnet.
- Use an Elastic IP.
- Create one NAT Gateway per Availability Zone for production.
- Keep databases in private subnets.
- Use VPC Endpoints for AWS services like S3 and DynamoDB to reduce NAT Gateway traffic and cost.

---

## Troubleshooting

Private EC2 cannot access the internet?

Check:

✅ NAT Gateway exists.

✅ NAT Gateway is in a public subnet.

✅ NAT Gateway has an Elastic IP.

✅ Internet Gateway is attached to the VPC.

✅ Public Route Table has:
0.0.0.0/0 → Internet Gateway.

✅ Private Route Table has:
0.0.0.0/0 → NAT Gateway.

✅ Security Groups allow outbound traffic.

✅ Network ACLs allow traffic.

---

## Interview Questions

### What is a NAT Gateway?

A NAT Gateway is a managed AWS service that provides outbound internet access for resources in private subnets without allowing inbound internet access.

---

### Why is a NAT Gateway placed in a public subnet?

Because it needs internet connectivity through the Internet Gateway and requires an Elastic IP.

---

### Can a NAT Gateway receive unsolicited inbound traffic from the internet?

No.

It only forwards return traffic for connections initiated by resources in private subnets.

---

### Does a NAT Gateway require an Elastic IP?

Yes.

The Elastic IP is used to communicate with the public internet.

---

### Can a private EC2 instance directly use an Internet Gateway?

No.

Private subnets should send outbound traffic to a NAT Gateway, which then uses the Internet Gateway.

---

## Key Takeaways

- A NAT Gateway provides secure outbound internet access for private subnets.
- It must be deployed in a public subnet.
- It requires an Elastic IP.
- Private Route Tables point to the NAT Gateway, not the Internet Gateway.
- NAT Gateway blocks unsolicited inbound internet traffic.
- For high availability, deploy one NAT Gateway per Availability Zone.
