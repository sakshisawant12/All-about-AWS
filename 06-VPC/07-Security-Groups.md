# Amazon VPC - Security Groups

## What is a Security Group?

A Security Group (SG) is a virtual firewall that controls inbound and outbound traffic for AWS resources such as EC2, RDS, ECS, and Lambda (when attached to a VPC).

Unlike traditional firewalls, Security Groups operate at the **instance level**.

Every EC2 instance must have at least one Security Group.

---

# Why Do We Need Security Groups?

Without a Security Group, any device could potentially communicate with your EC2 instance.

Security Groups help to:

- Control who can access your resources.
- Allow only required ports.
- Block unauthorized access.
- Improve application security.
- Follow the Principle of Least Privilege.

---

# How Security Groups Work

Whenever traffic reaches an EC2 instance, AWS checks the attached Security Group.

If the rule allows the traffic:

✅ Traffic is allowed.

Otherwise:

❌ Traffic is denied.

There are **only Allow rules**.

Security Groups do not support Deny rules.

---

# Security Groups are Stateful

This is one of the most important interview concepts.

If you allow incoming traffic, the return traffic is automatically allowed.

Similarly, if outbound traffic is allowed, the response traffic is automatically allowed.

Example:

Laptop

↓

SSH Request

↓

EC2

↓

SSH Response

Even if there is no explicit outbound rule for the response, AWS allows it automatically because Security Groups are **stateful**.

---

# Security Group Components

Each rule contains:

- Protocol
- Port Number
- Source (Inbound)
- Destination (Outbound)

Example:

Protocol:

TCP

Port:

22

Source:

203.0.113.10/32

Meaning:

Allow SSH only from one specific IP address.

---

# Inbound Rules

Inbound Rules control incoming traffic.

Example:

| Protocol | Port | Source | Purpose |
|----------|------|---------|---------|
|TCP|22|Your IP|SSH|
|TCP|80|0.0.0.0/0|HTTP|
|TCP|443|0.0.0.0/0|HTTPS|

---

# Outbound Rules

Outbound Rules control traffic leaving the instance.

By default, AWS allows all outbound traffic.

Example:

| Protocol | Port | Destination |
|----------|------|-------------|
|All|All|0.0.0.0/0|

Many organizations restrict outbound traffic for better security.

---

# Common Ports

| Port | Service |
|------|----------|
|22|SSH|
|80|HTTP|
|443|HTTPS|
|3389|RDP|
|3306|MySQL|
|5432|PostgreSQL|
|1433|Microsoft SQL Server|
|1521|Oracle Database|

---

# Security Group Example

Web Server

Inbound:

- HTTP (80) → Internet
- HTTPS (443) → Internet
- SSH (22) → Your IP only

Outbound:

- Allow All

Result:

Users can access the website.

Only the administrator can connect using SSH.

---

# Multiple Security Groups

An EC2 instance can have multiple Security Groups attached.

AWS combines all Allow rules.

Example:

Security Group 1

- SSH

Security Group 2

- HTTP

Security Group 3

- HTTPS

Result:

The EC2 instance allows:

- SSH
- HTTP
- HTTPS

AWS evaluates the combined rules.

---

# Security Group Referencing

Instead of allowing an IP range, you can allow another Security Group.

Example:

Web Server SG

↓

Application Server SG

Database SG

Only the Application Server Security Group can connect to the Database Security Group.

Benefits:

- Better security
- Easier management
- No need to update IP addresses

---

# Default Security Group

Every VPC has a Default Security Group.

Characteristics:

- Allows inbound traffic only from resources using the same Security Group.
- Allows all outbound traffic.
- Automatically created with the VPC.

Best practice:

Avoid using the Default Security Group in production.

Create custom Security Groups instead.

---

# Real-World Architecture

Internet

↓

Web Server

(Security Group)

↓

Application Server

(Security Group)

↓

Database

(Security Group)

Rules:

Web SG

Allow:

80

443

22 (Admin IP)

Application SG

Allow:

8080

from Web SG

Database SG

Allow:

3306

from Application SG

Result:

The database cannot be accessed directly from the internet.

---

# Security Group vs Firewall

Security Groups behave like a firewall but are:

- Virtual
- Instance-level
- Stateful
- Allow-only

---

# Best Practices

- Allow only required ports.
- Restrict SSH to your IP.
- Never expose database ports to the internet.
- Use Security Group references instead of IP addresses where possible.
- Remove unused rules regularly.
- Create separate Security Groups for different workloads.

---

# Common Mistakes

❌ Allowing SSH from 0.0.0.0/0.

❌ Opening database ports to the internet.

❌ Using the Default Security Group for everything.

❌ Allowing unnecessary ports.

❌ Forgetting outbound restrictions in production.

---

# Troubleshooting

Cannot SSH?

Check:

✅ Security Group allows TCP 22.

✅ Correct Public IP.

✅ EC2 has a Public IP.

✅ Internet Gateway attached.

✅ Route Table configured.

✅ Network ACL allows traffic.

Cannot open website?

Check:

✅ HTTP (80)

or

HTTPS (443)

allowed.

Apache/Nginx running.

Correct Security Group attached.

---

# Interview Questions

### What is a Security Group?

A Security Group is a stateful virtual firewall that controls inbound and outbound traffic for AWS resources.

---

### Are Security Groups stateful or stateless?

Stateful.

Return traffic is automatically allowed.

---

### Can Security Groups deny traffic?

No.

They support only Allow rules.

Anything not explicitly allowed is automatically denied.

---

### Can one EC2 instance have multiple Security Groups?

Yes.

AWS combines all Allow rules from every attached Security Group.

---

### Can a Security Group reference another Security Group?

Yes.

This allows secure communication between AWS resources without relying on IP addresses.

---

### What is the default outbound rule?

By default, all outbound traffic is allowed.

---

# Key Takeaways

- Security Groups are instance-level virtual firewalls.
- They are stateful.
- They support only Allow rules.
- Every EC2 instance must have at least one Security Group.
- Multiple Security Groups can be attached to one instance.
- Security Group references improve security and simplify management.
- Follow the Principle of Least Privilege by allowing only necessary traffic.
