# Amazon VPC - Interview Questions

## Beginner Level

### 1. What is Amazon VPC?

### 2. Why do we use VPC?

### 3. What is CIDR?

### 4. What are the private IPv4 ranges?

### 5. What is the difference between a Default VPC and a Custom VPC?

### 6. What is a subnet?

### 7. What is the difference between a Public Subnet and a Private Subnet?

### 8. Why can a subnet belong to only one Availability Zone?

### 9. What is a Route Table?

### 10. What is the Local Route?

---

## Intermediate Level

### 11. What is an Internet Gateway?

### 12. What is a NAT Gateway?

### 13. Why is a NAT Gateway placed in a public subnet?

### 14. Why does a NAT Gateway require an Elastic IP?

### 15. What is an Elastic IP?

### 16. What is an ENI?

### 17. What is the difference between a Public IP and an Elastic IP?

### 18. What is the difference between Security Groups and Network ACLs?

### 19. Why are Security Groups stateful?

### 20. Why are Network ACLs stateless?

---

## Advanced Level

### 21. What is VPC Peering?

### 22. Can VPC Peering work with overlapping CIDR blocks?

### 23. What does "VPC Peering is non-transitive" mean?

### 24. What is a VPC Endpoint?

### 25. Gateway Endpoint vs Interface Endpoint?

### 26. Why should you use a Gateway Endpoint for Amazon S3?

### 27. What is a Bastion Host?

### 28. Bastion Host vs AWS Systems Manager Session Manager?

### 29. What is AWS Client VPN?

### 30. Client VPN vs Site-to-Site VPN?

### 31. What is Site-to-Site VPN?

### 32. What is a Customer Gateway?

### 33. What is a Virtual Private Gateway?

### 34. What is AWS Direct Connect?

### 35. Site-to-Site VPN vs Direct Connect?

### 36. What is AWS Transit Gateway?

### 37. Why is Transit Gateway preferred over VPC Peering in large environments?

### 38. What are VPC Flow Logs?

### 39. What information do VPC Flow Logs capture?

### 40. Where can VPC Flow Logs be stored?

---

# Scenario-Based Questions

### 41. A private EC2 cannot access the internet. What will you check?

Expected points:

- NAT Gateway
- Route Table
- Internet Gateway
- Security Group
- Network ACL
- DNS settings

---

### 42. Users cannot access your website. How would you troubleshoot?

Expected points:

- Security Group
- Route Table
- Internet Gateway
- EC2 status
- Web server (Apache/Nginx)
- VPC Flow Logs

---

### 43. SSH to a public EC2 is failing. What could be the reasons?

Expected points:

- Security Group
- Public IP
- Internet Gateway
- Route Table
- Key Pair
- Network ACL

---

### 44. Two VPCs cannot communicate after peering. Why?

Expected points:

- Missing Route Table entries
- Overlapping CIDR blocks
- Security Group rules
- Network ACL rules
- Peering status

---

### 45. Which AWS networking service would you choose?

- Connect 2 VPCs → VPC Peering
- Connect many VPCs → Transit Gateway
- Connect remote employees → AWS Client VPN
- Connect office to AWS → Site-to-Site VPN
- Dedicated enterprise connection → AWS Direct Connect

---
