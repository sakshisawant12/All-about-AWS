# Amazon VPC - Elastic IP (EIP)

## What is an Elastic IP?

An Elastic IP (EIP) is a static public IPv4 address provided by AWS that you can allocate to your AWS account and associate with supported resources such as EC2 instances or NAT Gateways.

Unlike a normal Public IP, an Elastic IP remains yours until you release it.

---

# Why Do We Need an Elastic IP?

Normally, when an EC2 instance is stopped and started, its automatically assigned Public IP changes.

This can cause problems if:

- Clients connect using the IP address.
- DNS records point directly to the IP.
- Firewalls whitelist a specific IP.
- External APIs require a fixed IP.

Elastic IP solves this by providing a permanent public IP address.

---

# Public IP vs Elastic IP

| Feature | Public IP | Elastic IP |
|---------|-----------|------------|
|Type|Dynamic|Static|
|Changes after Stop/Start|Yes|No|
|Allocated to AWS Account|No|Yes|
|Can be Reassociated|No|Yes|
|Used with NAT Gateway|No|Yes|
|Best for Production|No|Yes|

---

# How Elastic IP Works

Example:

Internet

↓

Elastic IP

54.240.10.25

↓

EC2 Instance

Private IP

10.0.1.20

Users access the Elastic IP, while the EC2 continues using its private IP inside the VPC.

---

# Allocating an Elastic IP

Steps:

1. Open the AWS Management Console.
2. Go to **VPC** or **EC2**.
3. Select **Elastic IPs**.
4. Choose **Allocate Elastic IP address**.
5. AWS reserves the IP for your account.

At this point, the Elastic IP is allocated but not yet associated with any resource.

---

# Associating an Elastic IP

After allocation:

1. Select the Elastic IP.
2. Choose **Associate Elastic IP**.
3. Select the EC2 instance or network interface.
4. Save the changes.

The instance is now reachable using the Elastic IP.

---

# Reassociating an Elastic IP

One advantage of Elastic IPs is that they can be moved between resources.

Example:

Server A fails.

Disassociate the Elastic IP from Server A.

Associate it with Server B.

Users continue connecting to the same public IP.

This reduces downtime during failures or migrations.

---

# Elastic IP with NAT Gateway

A NAT Gateway requires an Elastic IP because it needs a stable public IP to communicate with the internet on behalf of private resources.

Flow:

Private EC2

↓

NAT Gateway

↓

Elastic IP

↓

Internet

---

# Elastic IP Limits

AWS places a limit on the number of Elastic IPs per Region (this quota can usually be increased through a service quota request).

Unused Elastic IPs may incur charges.

Always release Elastic IPs that are no longer needed.

---

# Real-World Example

Company Website

Users connect to:

203.0.113.10

↓

Elastic IP

↓

EC2 Web Server

If the server is replaced, the Elastic IP is moved to the new server.

The website remains reachable using the same IP address.

---

# Best Practices

- Use Elastic IPs only when a static public IP is required.
- Release unused Elastic IPs to avoid charges.
- Use DNS names (such as Amazon Route 53) instead of hardcoding IP addresses whenever possible.
- Use Elastic IPs with NAT Gateways and Bastion Hosts when appropriate.

---

# Common Mistakes

❌ Assuming an Elastic IP changes after stopping the instance.

❌ Forgetting to associate the Elastic IP after allocating it.

❌ Leaving unused Elastic IPs allocated.

❌ Confusing Public IPs with Elastic IPs.

---

# Troubleshooting

Cannot access the EC2 using the Elastic IP?

Check:

✅ Elastic IP is associated with the correct EC2 instance.

✅ Internet Gateway is attached to the VPC.

✅ Route Table contains:
0.0.0.0/0 → Internet Gateway.

✅ Security Group allows the required inbound ports.

✅ Network ACL allows the traffic.

---

# Interview Questions

### What is an Elastic IP?

An Elastic IP is a static public IPv4 address that can be allocated to an AWS account and associated with supported resources.

---

### Does an Elastic IP change after stopping and starting an EC2 instance?

No.

The Elastic IP remains associated until it is disassociated or released.

---

### Can an Elastic IP be moved to another EC2 instance?

Yes.

It can be disassociated from one supported resource and associated with another.

---

### Why does a NAT Gateway require an Elastic IP?

The NAT Gateway needs a stable public IP address to communicate with the internet on behalf of resources in private subnets.

---

### Are unused Elastic IPs free?

No.

Unused Elastic IPs may incur charges, so they should be released when no longer needed.

---

# Key Takeaways

- Elastic IP is a static public IPv4 address.
- It belongs to your AWS account until released.
- It can be moved between supported resources.
- NAT Gateways require an Elastic IP.
- Use Elastic IPs only when a fixed public IP is necessary.
