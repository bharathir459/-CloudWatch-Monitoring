# EC2 Monitoring & Auto-Remediation with CloudWatch

An AWS monitoring setup using CloudWatch, SNS, and Bash scripts to automatically detect and remediate common EC2 issues across production environments.

## What This Project Does

- Monitors **CPU, memory, disk, and network** metrics for EC2 instances via CloudWatch
- Sends **SNS alerts** to the on-call team when thresholds are breached
- **Auto-remediates** common issues (disk cleanup, service restart) via Bash scripts triggered by alarms
- Detects **network anomalies** using VPC Flow Logs and CloudWatch metric filters
- Reduces manual intervention for low-severity alerts

## Results Achieved

- Reduced manual intervention for low-severity alerts by **35%**
- Reduced time to detect network anomalies from **hours to under 10 minutes**
- Maintained **99.9% production uptime** across monitored environments

## Project Structure

```
project2-cloudwatch-monitoring/
├── README.md
├── cloudwatch/
│   ├── alarms-config.json              # CloudWatch alarm definitions
│   └── flow-logs-metric-filter.json    # VPC Flow Logs metric filter for anomaly detection
├── scripts/
│   ├── disk-cleanup.sh                 # Auto-remediates high disk usage
│   ├── service-restart.sh              # Restarts a failed service automatically
│   └── memory-report.sh                # Reports memory usage (CloudWatch custom metric)
└── sns/
    └── sns-alert-policy.json           # SNS topic policy for alert delivery
```

## How It Works

```
CloudWatch Alarm Triggered
        |
        v
SNS Topic receives alert
        |
        +---> Email to on-call engineer
        |
        +---> Triggers auto-remediation script (for low-severity)
                    |
                    v
            disk-cleanup.sh / service-restart.sh runs on EC2
                    |
                    v
            CloudWatch logs confirm remediation success
```

## How to Use

### 1. Create SNS topic for alerts
```bash
aws sns create-topic --name cloud-ops-alerts
aws sns subscribe \
  --topic-arn arn:aws:sns:<region>:<account-id>:cloud-ops-alerts \
  --protocol email \
  --notification-endpoint bharathir459@gmail.com
```

### 2. Deploy CloudWatch alarms
```bash
aws cloudwatch put-metric-alarm \
  --cli-input-json file://cloudwatch/alarms-config.json
```

### 3. Enable VPC Flow Logs metric filter
```bash
aws logs put-metric-filter \
  --cli-input-json file://cloudwatch/flow-logs-metric-filter.json
```

### 4. Deploy auto-remediation scripts on EC2
```bash
chmod +x scripts/disk-cleanup.sh
chmod +x scripts/service-restart.sh
chmod +x scripts/memory-report.sh
```

## Requirements

- AWS CLI configured
- EC2 instances with CloudWatch agent installed
- SNS topic created for alert delivery
- VPC Flow Logs enabled and streaming to CloudWatch Logs

## Tech Stack

`AWS CloudWatch` `AWS SNS` `VPC Flow Logs` `EC2` `Bash` `Python` `AWS CLI`

---
*Based on real-world monitoring work at Accenture (2023–2026)*
