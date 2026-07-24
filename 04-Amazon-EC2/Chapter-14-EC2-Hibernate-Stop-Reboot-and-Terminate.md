# 📘 Chapter 14 - EC2 Hibernate, Stop, Reboot and Terminate

> **"EC2 instance actions determine how your instance behaves, what happens to your data, networking, memory, and billing."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- EC2 instance actions
- Stop
- Start
- Reboot
- Hibernate
- Terminate
- Billing behavior
- Storage behavior
- IP Address behavior
- When to use each action
- Best Practices
- Interview Questions

---

# 📖 What are EC2 Instance Actions?

Once an EC2 instance is launched, AWS allows you to perform different operations on it.

The most common actions are:

- Start
- Stop
- Reboot
- Hibernate
- Terminate

Each action affects the instance differently.

---

# 💡 Simple Definition

Imagine using your laptop.

You can:

- Restart it
- Shut it down
- Put it into Sleep mode
- Throw it away permanently

EC2 works in a similar way.

---

# 🏗️ EC2 Actions Overview

```
               EC2 Instance
                     │
     ┌───────────────┼────────────────┐
     ▼               ▼                ▼
  Reboot          Stop          Hibernate
     │               │                │
     ▼               ▼                ▼
 Continue        Powered Off      Memory Saved
 Running         Can Start        Resume Later
                     │
                     ▼
                 Terminate
                     │
                     ▼
             Permanently Deleted
```

---

# 🔄 Reboot

Reboot restarts only the operating system.

AWS does **not** move the instance to different hardware.

### What Happens?

- Operating system restarts
- Same EC2 instance
- Same Private IP
- Public IP usually remains the same
- Elastic IP remains the same
- EBS volumes remain attached
- Instance Store data remains available because the instance is not stopped

---

### Billing

✅ Compute charges continue.

---

### Best Use Cases

- Apply software updates
- Restart services
- Fix temporary issues

---

# ⏸️ Stop

Stopping powers off the virtual machine.

AWS releases the underlying compute host.

### What Happens?

- Instance shuts down
- Compute resources released
- Same EBS volumes
- Same Private IP
- Auto-assigned Public IP is released
- Elastic IP remains associated (if attached)
- Instance Store data is lost

---

### Billing

❌ Compute charges stop.

✅ EBS storage charges continue.

---

### Best Use Cases

- Development servers
- Cost saving
- Temporary maintenance

---

# ▶️ Start

Starting powers on a previously stopped instance.

AWS allocates new compute resources.

### What Happens?

- Operating system boots
- Same EBS volume
- Same Private IP
- New Public IP (unless using Elastic IP)
- Applications start normally

---

### Billing

✅ Compute charges resume.

---

# 💾 Hibernate

Hibernate saves the contents of RAM to the root EBS volume before stopping the instance.

When started again, applications continue from the same state.

---

### What Happens?

- RAM contents saved to EBS
- CPU stops
- Same Private IP
- Same EBS volume
- Elastic IP remains associated
- Auto-assigned Public IP changes after restart unless an Elastic IP is used
- Instance Store data is lost because the instance stops

---

### Requirements

- Supported instance type
- Supported operating system
- Encrypted EBS root volume recommended
- Hibernation enabled during launch

---

### Billing

❌ Compute charges stop.

✅ EBS charges continue.

---

### Best Use Cases

- Long-running applications
- Large development environments
- Memory-intensive workloads

---

# ❌ Terminate

Terminate permanently deletes the EC2 instance.

### What Happens?

- Instance deleted
- Root EBS volume deleted by default
- Public IP released
- Private IP released
- Elastic IP is disassociated (it can be reused if not released)
- Cannot restart the instance
- Instance Store data lost

---

### Billing

❌ Compute charges stop.

❌ Storage charges stop for deleted volumes.

---

### Best Use Cases

- Temporary environments
- Finished projects
- Unused servers

---

# 📊 Comparison Table

| Feature | Reboot | Stop | Hibernate | Terminate |
|---------|--------|------|-----------|-----------|
| OS Restart | ✅ | ❌ | ❌ | ❌ |
| Compute Stops | ❌ | ✅ | ✅ | ✅ |
| RAM Preserved | ❌ | ❌ | ✅ | ❌ |
| Root EBS Preserved | ✅ | ✅ | ✅ | ❌ (Default) |
| Instance Store Preserved | ✅ | ❌ | ❌ | ❌ |
| Private IP Preserved | ✅ | ✅ | ✅ | ❌ |
| Auto Public IP Preserved | Usually | ❌ | ❌ | ❌ |
| Elastic IP Preserved | ✅ | ✅ | ✅ | Disassociated |
| Restart Possible | N/A | ✅ | ✅ | ❌ |

---

# 🌍 Real-World Example

A software company hosts a development server.

During working hours:

```
Start EC2

↓

Develop Application

↓

Stop EC2
```

Result:

- Saves compute cost.
- Project files remain on EBS.

---

Another example:

```
Financial Application

↓

Hibernate

↓

Resume Later

↓

Continue Processing
```

No need to reopen applications or reload memory.

---

# ⭐ When Should You Use Each Action?

### Reboot

- Restart operating system
- Install updates
- Restart services

---

### Stop

- Save costs
- Temporary shutdown
- Development environments

---

### Hibernate

- Resume work quickly
- Preserve memory state
- Long-running applications

---

### Terminate

- Remove unused servers
- Delete temporary environments
- Clean up resources

---

# ⭐ Best Practices

- Stop development instances when not in use.
- Use Hibernate for workloads that benefit from fast resume.
- Take EBS snapshots before terminating important instances.
- Enable termination protection for production servers.
- Use Elastic IP if a fixed public IP is required.

---

# ❌ Common Mistakes

- Thinking Stop deletes data.
- Forgetting Instance Store data is lost after Stop or Hibernate.
- Assuming the public IP remains the same after Stop → Start.
- Terminating production instances accidentally.
- Not creating backups before termination.

---

# 📝 Quick Revision

```
Reboot
↓

Restart OS

--------------------

Stop
↓

Power Off

--------------------

Hibernate
↓

Save RAM

--------------------

Terminate
↓

Delete Instance
```

---

# 🎤 Interview Questions

### 1. What is the difference between Stop and Terminate?

**Stop**

- Instance can be started again.
- EBS data remains.
- Compute charges stop.

**Terminate**

- Instance is permanently deleted.
- Root EBS volume is deleted by default.
- Instance cannot be restarted.

---

### 2. What is Hibernate?

Hibernate saves the RAM contents to the root EBS volume and allows the instance to resume from the same state later.

---

### 3. Does Reboot change the Private IP?

No.

The Private IP remains the same.

---

### 4. What happens to an auto-assigned Public IP after Stop → Start?

It is released when the instance stops, and a new public IP may be assigned when it starts again.

---

### 5. Is the Elastic IP lost when an instance is stopped?

No.

The Elastic IP remains associated with the instance unless you explicitly disassociate or release it.

---

### 6. Which action permanently deletes an EC2 instance?

**Terminate**

---

# 🎯 Chapter Summary

EC2 provides several instance actions for different operational needs. **Reboot** restarts the operating system without affecting storage, **Stop** powers off the instance while preserving EBS data, **Hibernate** saves the contents of RAM to the root EBS volume so applications can resume quickly, and **Terminate** permanently deletes the instance. Choosing the correct action helps balance cost, availability, and operational efficiency.
