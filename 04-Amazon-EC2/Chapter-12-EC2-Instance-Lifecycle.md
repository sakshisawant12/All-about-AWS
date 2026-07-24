# 📘 Chapter 12 - EC2 Instance Lifecycle

> **"The EC2 Instance Lifecycle describes the different states an EC2 instance goes through from creation until termination."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is the EC2 Instance Lifecycle?
- Different EC2 instance states
- State transitions
- Start vs Stop vs Reboot vs Terminate
- Hibernate
- Billing in each state
- EBS behavior
- Public IP behavior
- Best Practices
- Interview Questions

---

# 📖 What is the EC2 Instance Lifecycle?

The **EC2 Instance Lifecycle** is the sequence of states an EC2 instance passes through from the moment it is launched until it is permanently deleted.

Each state affects:

- Billing
- Storage
- Networking
- Availability

Understanding these states helps you manage EC2 instances efficiently.

---

# 💡 Simple Definition

Think of a laptop.

You can:

- Turn it on
- Restart it
- Shut it down
- Throw it away

An EC2 instance follows a similar lifecycle.

---

# 🌍 Real-Life Example

Imagine renting a hotel room.

```
Book Room

↓

Room Preparing

↓

Stay in Room

↓

Leave Room

↓

Return Later

↓

Check Out Permanently
```

This is similar to the EC2 lifecycle.

---

# 🏗️ EC2 Instance Lifecycle

```
Launch

   │

   ▼

Pending

   │

   ▼

Running

   │

 ┌─┴──────────────┐

 ▼                ▼

Reboot          Stop

 │                │

 ▼                ▼

Running        Stopped

                  │

                  ▼

               Start

                  │

                  ▼

               Running

                  │

                  ▼

              Terminate

                  │

                  ▼

            Terminated
```

---

# 🚀 Pending State

When you launch an EC2 instance, it first enters the **Pending** state.

AWS performs tasks such as:

- Allocating hardware
- Attaching storage
- Configuring networking
- Booting the operating system

Characteristics:

- Temporary state
- Cannot connect yet
- Short duration

---

# 🟢 Running State

The instance is fully operational.

You can:

- SSH into Linux
- RDP into Windows
- Host websites
- Run applications

Billing:

✅ Compute charges apply.

---

# 🔄 Reboot State

Reboot restarts the operating system without moving the instance to another host.

Characteristics:

- Same instance
- Same EBS volume
- Usually retains the same public IP (if auto-assigned and not stopped)
- Private IP remains the same

Billing:

✅ Compute charges continue.

---

# ⏸️ Stop State

Stopping shuts down the operating system and powers off the virtual machine.

Characteristics:

- Compute resources are released.
- EBS volumes remain attached.
- Data stored on EBS is preserved.
- Auto-assigned public IPv4 address is released.
- Private IP remains the same.

Billing:

❌ Compute charges stop.

✅ EBS storage charges continue.

---

# ▶️ Start State

Starting a stopped instance powers it on again.

AWS:

- Allocates compute resources
- Boots the operating system
- Reattaches EBS volumes

Characteristics:

- Same EBS data
- Same Private IP
- New Public IP (unless using an Elastic IP)

Billing:

✅ Compute charges start again.

---

# 💾 Hibernate State

Hibernate saves the contents of RAM to the root EBS volume before stopping the instance.

When restarted:

- Applications continue from where they left off.
- Memory contents are restored.

Requirements:

- Supported instance type
- Supported operating system
- Hibernation enabled during launch
- Root EBS volume large enough to store RAM contents

Billing:

❌ Compute charges stop.

✅ EBS storage charges continue.

---

# ❌ Terminate State

Termination permanently deletes the EC2 instance.

Characteristics:

- Compute resources released
- Root EBS volume deleted by default (unless configured otherwise)
- Public IP released
- Cannot restart the instance

Billing:

❌ Compute charges stop.

EBS charges also stop for volumes that are deleted.

---

# 📊 Billing in Each State

| State | Compute Charges | EBS Charges |
|--------|----------------:|------------:|
| Pending | ✅ Yes | ✅ Yes |
| Running | ✅ Yes | ✅ Yes |
| Reboot | ✅ Yes | ✅ Yes |
| Stopped | ❌ No | ✅ Yes |
| Hibernate | ❌ No | ✅ Yes |
| Terminated | ❌ No | ❌ No (if volumes deleted) |

---

# 🌐 Public IP Behavior

| Action | Public IP | Private IP |
|--------|-----------|------------|
| Running | Available | Same |
| Reboot | Usually Unchanged | Same |
| Stop → Start | Changes (unless Elastic IP) | Same |
| Terminate | Released | Released |

---

# 📊 Lifecycle Summary

| State | Description |
|--------|-------------|
| Pending | Instance is launching |
| Running | Instance is active |
| Reboot | Operating system restarts |
| Stopped | Instance powered off |
| Start | Instance powers on again |
| Hibernate | RAM saved to EBS, then stopped |
| Terminated | Instance permanently deleted |

---

# 🌍 Real-World Example

A company hosts a development server.

During office hours:

```
Start EC2

↓

Running
```

After work:

```
Stop EC2

↓

No Compute Charges
```

The next morning:

```
Start EC2

↓

Continue Working
```

This helps reduce AWS costs while preserving the server's data.

---

# ⭐ Best Practices

- Stop development instances when not in use.
- Use Hibernate if applications need to resume quickly.
- Use Elastic IP if a fixed public IP is required.
- Terminate unused instances to avoid unnecessary storage charges.
- Take EBS snapshots before terminating important instances.

---

# ❌ Common Mistakes

- Assuming stopping an instance deletes data.
- Forgetting that EBS storage is still billed when an instance is stopped.
- Expecting the same public IP after stopping and starting an instance.
- Accidentally terminating production instances.
- Not enabling termination protection for critical workloads.

---

# 📝 Quick Revision

```
Launch

↓

Pending

↓

Running

↓

Stop

↓

Stopped

↓

Start

↓

Running

↓

Terminate

↓

Deleted
```

---

# 🎤 Interview Questions

### 1. What is the EC2 Instance Lifecycle?

It is the sequence of states an EC2 instance passes through from launch to termination.

---

### 2. Does stopping an EC2 instance delete its data?

No.

Data stored on attached EBS volumes remains available.

---

### 3. Does AWS charge for a stopped instance?

Compute charges stop, but EBS storage charges continue.

---

### 4. What happens to the Public IP after stopping and starting an instance?

An auto-assigned public IPv4 address may change.

If an Elastic IP is associated, it remains the same.

---

### 5. What is the difference between Stop and Terminate?

**Stop**

- Can start the instance again.
- EBS data is preserved.
- Compute charges stop.

**Terminate**

- Permanently deletes the instance.
- Root EBS volume is deleted by default.
- Instance cannot be restarted.

---

### 6. What is Hibernate?

Hibernate saves the contents of RAM to the root EBS volume before stopping the instance, allowing applications to resume from where they left off.

---

# 🎯 Chapter Summary

The EC2 Instance Lifecycle defines how an instance moves between different operational states such as Pending, Running, Stopped, Hibernate, and Terminated. Each state affects billing, storage, and networking differently. Understanding these transitions helps optimize costs, maintain availability, and manage EC2 instances effectively in AWS.
