# 📘 Chapter 15 - EC2 Monitoring with Amazon CloudWatch

> **"Amazon CloudWatch is a monitoring and observability service that collects metrics, logs, and events from AWS resources, helping you monitor performance and automate responses."**

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What is Amazon CloudWatch?
- Why CloudWatch is important
- Basic vs Detailed Monitoring
- EC2 Metrics
- CloudWatch Alarms
- CloudWatch Dashboard
- CloudWatch Logs
- CloudWatch Events (EventBridge)
- Real-world examples
- Best Practices
- Interview Questions

---

# 📖 What is Amazon CloudWatch?

**Amazon CloudWatch** is a monitoring service that collects data from AWS resources and applications.

It helps you:

- Monitor EC2 instances
- Track resource usage
- Detect failures
- Create alarms
- Automate actions
- View performance dashboards

CloudWatch is one of the most important AWS monitoring services.

---

# 💡 Simple Definition

Imagine driving a car.

Your dashboard shows:

- Speed
- Fuel level
- Engine temperature
- Warning lights

Without the dashboard, you wouldn't know if something was wrong.

CloudWatch is the dashboard for your AWS resources.

---

# 🌍 Real-Life Example

A company hosts an e-commerce website.

```
Customers

↓

Website

↓

EC2 Instance

↓

CloudWatch

↓

CPU Usage

↓

Alarm

↓

Administrator Notified
```

If CPU usage becomes too high, CloudWatch can automatically notify the operations team.

---

# 🤔 Why Do We Need CloudWatch?

Without CloudWatch:

- No visibility into server health
- Problems detected manually
- Slow response to failures

With CloudWatch:

- Real-time monitoring
- Automatic alerts
- Performance tracking
- Faster troubleshooting

---

# 🏗️ CloudWatch Architecture

```
              EC2 Instance
                   │
                   ▼
             CloudWatch Agent
                   │
                   ▼
          Amazon CloudWatch
          ┌────────┼────────┐
          ▼        ▼        ▼
      Metrics    Logs    Alarms
                              │
                              ▼
                      Notification / Action
```

> **Note:** EC2 automatically sends basic metrics to CloudWatch. To collect operating system logs and memory/disk metrics, you typically install the **CloudWatch Agent**.

---

# 📊 Basic Monitoring vs Detailed Monitoring

| Feature | Basic Monitoring | Detailed Monitoring |
|----------|------------------|---------------------|
| Metric Interval | 5 Minutes | 1 Minute |
| Cost | Included for EC2 | Additional Charges |
| Suitable For | Development | Production |

---

# 📈 Common EC2 Metrics

CloudWatch automatically collects several EC2 metrics.

| Metric | Description |
|----------|-------------|
| CPUUtilization | CPU usage percentage |
| NetworkIn | Incoming network traffic |
| NetworkOut | Outgoing network traffic |
| DiskReadOps | Read operations |
| DiskWriteOps | Write operations |
| DiskReadBytes | Bytes read |
| DiskWriteBytes | Bytes written |
| StatusCheckFailed | Overall health check status |

---

# 🔍 EC2 Status Checks

CloudWatch tracks EC2 health using status checks.

### 1️⃣ System Status Check

Checks the AWS infrastructure.

Examples:

- Power failure
- Network issue
- Hardware failure

---

### 2️⃣ Instance Status Check

Checks the operating system.

Examples:

- OS crash
- Kernel panic
- Network configuration issue

---

### 3️⃣ Attached EBS Status (where applicable)

Helps identify storage-related health issues.

---

# 🚨 What is a CloudWatch Alarm?

A **CloudWatch Alarm** watches a metric and performs an action when a condition is met.

Example:

```
CPU > 80%

↓

CloudWatch Alarm

↓

Send Notification

↓

Administrator
```

---

# 🚀 Creating a CloudWatch Alarm

Steps:

```
CloudWatch

↓

Alarms

↓

Create Alarm

↓

Select Metric

↓

Set Threshold

↓

Configure Action

↓

Create
```

---

# 📱 Alarm Actions

CloudWatch can perform actions like:

- Send an Amazon SNS notification
- Stop an EC2 instance
- Start an EC2 instance
- Reboot an EC2 instance
- Recover a supported EC2 instance
- Trigger Auto Scaling

---

# 📋 CloudWatch Dashboards

Dashboards display multiple metrics in one place.

Example:

```
CloudWatch Dashboard

----------------------------

CPU Usage

Memory Usage

Network Traffic

Disk Activity

Status Checks

----------------------------
```

This provides a centralized view of infrastructure health.

---

# 📄 CloudWatch Logs

CloudWatch Logs stores log files from AWS resources and applications.

Examples:

- Apache Logs
- Nginx Logs
- Application Logs
- System Logs
- Custom Application Logs

To send operating system logs from EC2, install and configure the CloudWatch Agent.

---

# ⚡ Amazon EventBridge

Amazon EventBridge (formerly CloudWatch Events) reacts to AWS events.

Example:

```
EC2 Instance Stops

↓

EventBridge Rule

↓

AWS Lambda

↓

Send Email
```

This enables event-driven automation.

---

# 🌍 Real-World Example

A company's production server experiences unusually high CPU usage.

```
EC2

↓

CPU 90%

↓

CloudWatch Alarm

↓

Amazon SNS

↓

Email Notification

↓

Administrator Investigates
```

The issue is detected before customers notice a problem.

---

# ⭐ Advantages

- Centralized monitoring
- Automatic alerts
- Performance analysis
- Historical metrics
- Easy integration with AWS services
- Supports automation

---

# ⚠️ Common Mistakes

- Not creating alarms for critical metrics.
- Ignoring status check failures.
- Assuming memory usage is available by default (it requires the CloudWatch Agent).
- Collecting too many custom metrics unnecessarily.
- Forgetting to review dashboards regularly.

---

# ⭐ Best Practices

- Enable Detailed Monitoring for production workloads when appropriate.
- Create alarms for CPU, status checks, and important application metrics.
- Use Amazon SNS for notifications.
- Install the CloudWatch Agent to monitor memory, disk usage, and logs.
- Build dashboards for critical applications.

---

# 📊 CloudWatch Summary

| Feature | Description |
|----------|-------------|
| Service Type | Monitoring & Observability |
| Default EC2 Metrics | CPU, Network, Disk, Status Checks |
| Log Collection | CloudWatch Agent |
| Alarm Support | Yes |
| Dashboard Support | Yes |
| Automation | SNS, Auto Scaling, EventBridge |

---

# 📝 Quick Revision

```
EC2

↓

CloudWatch

├── Metrics

├── Logs

├── Dashboards

├── Alarms

└── Notifications
```

---

# 🎤 Interview Questions

### 1. What is Amazon CloudWatch?

Amazon CloudWatch is a monitoring and observability service that collects metrics, logs, and events from AWS resources.

---

### 2. What is the difference between Basic and Detailed Monitoring?

- **Basic Monitoring:** Metrics every 5 minutes.
- **Detailed Monitoring:** Metrics every 1 minute.

---

### 3. Does CloudWatch monitor memory usage by default?

No.

Memory utilization requires the CloudWatch Agent.

---

### 4. What is a CloudWatch Alarm?

A CloudWatch Alarm monitors a metric and performs an action when a defined threshold is reached.

---

### 5. Name three EC2 metrics collected by CloudWatch.

- CPUUtilization
- NetworkIn
- NetworkOut

---

### 6. Can CloudWatch automatically reboot an EC2 instance?

Yes.

A CloudWatch Alarm can trigger actions such as rebooting, stopping, recovering, or integrating with other AWS services, depending on the configuration.

---

# 🎯 Chapter Summary

Amazon CloudWatch is AWS's monitoring and observability service. It collects metrics, logs, and events from EC2 instances and other AWS resources, enabling administrators to monitor performance, create alarms, automate actions, and troubleshoot issues quickly. CloudWatch is an essential service for maintaining reliable, secure, and highly available cloud environments.
