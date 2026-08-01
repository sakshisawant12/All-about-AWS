# Amazon VPC - Bastion Host (Jump Server)

## What is a Bastion Host?

A Bastion Host (also called a Jump Server) is an EC2 instance deployed in a **public subnet** that is used to securely access EC2 instances in **private subnets**.

Instead of exposing private servers directly to the internet, administrators first connect to the Bastion Host and then connect to the private instances.

---

# Why Do We Need a Bastion Host?

Private EC2 instances do not have:

- Public IP addresses
- Direct Internet Gateway access

This means you cannot SSH directly from your laptop to a private EC2 instance.

A Bastion Host acts as a secure entry point into the private network.

---

# Real-World Example

Company Infrastructure:

- Web Server (Public)
- Application Server (Private)
- Database (Private)

The administrator needs to manage the Application Server.

Instead of giving the Application Server a Public IP:

Laptop

↓

Bastion Host

↓

Application Server

↓

Database

Only the Bastion Host is exposed to the internet.

---

# How a Bastion Host Works

Traffic Flow:

Administrator Laptop

↓

SSH (Port 22)

↓

Bastion Host (Public Subnet)

↓

SSH (Private IP)

↓

Private EC2

The Bastion Host forwards the SSH connection to the private instance.

---

# Architecture

                    Internet
                        │
                  Internet Gateway
                        │
               Public Route Table
                        │
                Public Subnet
                        │
                 Bastion Host
                        │
               Private Route Table
                        │
               Private Subnet
                        │
                 Private EC2

Only the Bastion Host has a Public IP.

---

# Security Group Configuration

## Bastion Host Security Group

Inbound:

| Protocol | Port | Source |
|----------|------|--------|
|TCP|22|Administrator Public IP|

Outbound:

Allow SSH to the private subnet.

---

## Private EC2 Security Group

Inbound:

| Protocol | Port | Source |
|----------|------|--------|
|TCP|22|Bastion Host Security Group|

Notice:

The private EC2 does **not** allow SSH from the internet.

It only trusts the Bastion Host.

---

# SSH Connection Example

Step 1

SSH to Bastion Host

```bash
ssh -i key.pem ec2-user@54.201.10.25
```

Step 2

From the Bastion Host

```bash
ssh ec2-user@10.0.2.15
```

Now you are connected to the private EC2 instance.

---

# Benefits

- Improved security
- No Public IP on private servers
- Centralized administrative access
- Reduced attack surface
- Easy auditing of administrator access

---

# Bastion Host vs Direct SSH

| Feature | Bastion Host | Direct SSH |
|---------|--------------|------------|
|Public IP on Private EC2|No|Yes|
|Internet Exposure|Minimal|High|
|Security|High|Lower|
|Production Recommended|Yes|No|

---

# Bastion Host vs AWS Session Manager

| Feature | Bastion Host | Session Manager |
|---------|--------------|-----------------|
|Requires Public EC2|Yes|No|
|Requires SSH Port 22|Yes|No|
|Uses IAM|No|Yes|
|Uses SSM Agent|No|Yes|
|Recommended by AWS|Good|Best Practice|

Today, AWS recommends **Systems Manager Session Manager** because it eliminates the need for a public Bastion Host in many environments.

---

# Best Practices

- Restrict SSH access to trusted administrator IPs.
- Use Multi-Factor Authentication (MFA) for administrator accounts.
- Keep the Bastion Host updated with security patches.
- Use EC2 Instance Connect or Session Manager where possible.
- Do not install unnecessary software on the Bastion Host.

---

# Common Mistakes

❌ Allowing SSH from `0.0.0.0/0`.

❌ Giving private EC2 instances Public IPs unnecessarily.

❌ Using the same Security Group for all instances.

❌ Forgetting to rotate SSH keys.

❌ Leaving the Bastion Host unpatched.

---

# Troubleshooting

Cannot SSH to Bastion Host?

Check:

✅ Internet Gateway attached.

✅ Public IP assigned.

✅ Route Table has:
0.0.0.0/0 → Internet Gateway.

✅ Security Group allows TCP 22 from your IP.

Cannot SSH from Bastion Host to Private EC2?

Check:

✅ Private EC2 Security Group allows SSH from the Bastion Host Security Group.

✅ Route Tables are correct.

✅ Private IP address is used.

---

# Interview Questions

### What is a Bastion Host?

A Bastion Host is an EC2 instance in a public subnet that provides secure administrative access to resources in private subnets.

---

### Why is a Bastion Host placed in a public subnet?

Because administrators need internet access to connect to it.

---

### Can private EC2 instances have Public IP addresses?

Yes, technically they can, but in a properly designed architecture they normally should not.

---

### What is the biggest advantage of a Bastion Host?

It prevents private EC2 instances from being directly exposed to the internet.

---

### Is a Bastion Host still recommended today?

It is still used, but AWS generally recommends **Systems Manager Session Manager** because it provides secure access without requiring a public EC2 instance or opening SSH ports.

---

# Key Takeaways

- A Bastion Host is a secure jump server.
- It is deployed in a public subnet.
- Private EC2 instances remain inaccessible from the internet.
- SSH access flows through the Bastion Host.
- Restrict SSH access to trusted administrator IPs.
- AWS Systems Manager Session Manager is the modern alternative in many environments.
