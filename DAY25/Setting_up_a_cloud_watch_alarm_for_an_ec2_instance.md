# Day 25 – CloudWatch Alarm for EC2 (KodeKloud)

##  Overview

This lab demonstrates how to:

- Launch an Ubuntu EC2 instance
- Create a CloudWatch alarm
- Monitor CPU Utilization
- Trigger an SNS notification when CPU usage exceeds a threshold

This exercise builds foundational knowledge in monitoring, alerting, and production readiness within AWS.

---

##  Step 1: Launch EC2 Instance

### Instance Details

- **Name:** datacenter-ec2
- **AMI:** Ubuntu (any appropriate Ubuntu AMI)
- **Instance Type:** (e.g., t2.micro if in free tier)
- **Monitoring:** Basic monitoring enabled

---

##  Step 2: Create CloudWatch Alarm

### Alarm Configuration

- **Alarm Name:** datacenter-alarm
- **Metric:** CPUUtilization
- **Statistic:** Average
- **Period:** 5 minutes
- **Threshold:** Greater than or equal to 90%
- **Evaluation Periods:** 1
- **Condition:** ≥ 90% for 1 consecutive 5-minute period

---

##  Alarm Action

- **Action Type:** Send notification
- **SNS Topic:** datacenter-sns-topic

When the alarm enters the ALARM state:
- A notification is published to the SNS topic
- Subscribers (e.g., email) receive an alert

---

##  Understanding Alarm States

CloudWatch alarms have three states:

### 1️⃣ OK
Metric is within the defined threshold.

### 2️⃣ ALARM
Metric breaches the defined threshold.

### 3️⃣ INSUFFICIENT_DATA
Not enough metric data available (e.g., instance just launched or stopped).

---

##  Evaluation Logic Explained

### Example 1: Configuration
- Period = 5 minutes
- Evaluation Periods = 1

If CPU ≥ 90% for the full 5-minute period → Alarm enters ALARM state.

---

### Example 2: More Aggressive Setup
- Period = 1 minute
- Evaluation Periods = 1

If CPU ≥ 90% for just 1 minute → Immediate ALARM.

 This setup may cause false positives due to short spikes.

---

### Example 3: More Stable Production Setup
- Period = 1 minute
- Evaluation Periods = 3

CPU must remain ≥ 90% for 3 consecutive minutes before triggering ALARM.

This reduces alert noise and avoids reacting to temporary spikes.

---

##  Alarm Recovery

If CPU drops below 90% and meets the evaluation requirement:

- Alarm transitions from ALARM → OK
- Optional: OK notifications can be configured

---

##  Why This Is Important

✔ Proactive Monitoring  
✔ Automated Alerting  
✔ Improved Reliability  
✔ Reduced Downtime  
✔ Production-Ready Cloud Architecture  

Monitoring is a core DevOps and Cloud Engineering skill.

---

##  Key Takeaways

- CloudWatch evaluates metrics based on Period and Evaluation Periods.
- Average smooths spikes, Maximum detects spikes.
- Shorter periods = faster reaction.
- More evaluation periods = more stability.
- Alarm actions integrate with SNS for automated notifications.

---

##  Conclusion

This lab demonstrates how monitoring and alerting work together in AWS to create resilient and observable infrastructure.

Launching an EC2 instance is basic.

Monitoring it properly is professional.
