# W6 Evidence Pack — Operations Hardening & Cost-Aware Cloud

---

# Group Information

| Item | Details |
|---|---|
| Group ID | G7 |
| Project Name | Car Sales System |
| Business Domain | Automotive / Car Sales |
| Repository Link | |
| W5 Evidence Pack Link | |
| Presentation Date | |

---

# Team Members

| Name | Student ID |
|---|---|
| Ngo Huu Tai | XB-DN26-008 |
| Mai Phuoc Khoa | XB-DN26-033 |
| Nguyen Tien Hoang Thinh | XB-DN26-047 |
| Dang Thi Ngoc Thao | XB-DN26-055 |
| Nguyen Phu Trieu | XB-DN26-070 |
| Nguyen Hung Thinh | XB-DN26-077 |
| Huynh Ba Huan | XB-DN26-106 |
| Nguyen Van Tuan Anh | XB-DN26-112 |
| Le Hoang Viet | XB-DN26-134 |
| Hoang Cong Tri Dung | XB-DN26-148 |

---

# Section 1 — Project Recap

## 1.1 Project Overview

### Application Description

> Describe the car sales system, target users, and core business functions.

### Business Domain

> Explain the automotive dealership business context.

### Main Features

- Car listing management
- Vehicle search and filtering
- Customer inquiry management
- Sales management
- AI-powered recommendation/chatbot system
- Other:

---

## 1.2 Architecture Summary (W1 → W5)

### W1 — Core Architecture

> Describe the original 3-tier architecture design.

### W2 — Storage & Identity

> Describe storage services and IAM/security setup.

### W3 — Database & AI Layer

> Describe database, AI services, Bedrock, Lambda, or knowledge base integration.

### W4 — Intelligent Automation

> Describe agent tools, memory, retrieval strategy, orchestration, etc.

### W5 — Network Hardening

> Describe VPC, firewall, API Gateway, backup/restore, scaling, and security improvements.

---

## 1.3 W5 Feedback Improvements (Optional)

> Mention any W5 feedback resolved this week.

---

# Section 2 — MH-COST-V — Cost Visibility & Attribution

## 2.1 Tagging Strategy

### Required Tag Keys

| Tag Key | Example Value | Purpose |
|---|---|---|
| Owner | teamlead@email.com | Resource ownership |
| Environment | dev | Environment identification |
| CostCenter | G7 | Team cost tracking |
| Application | CarSalesSystem | Application grouping |

---

### Allowed Tag Values

| Tag Key | Allowed Values |
|---|---|
| Owner | |
| Environment | dev, test, prod |
| CostCenter | G7 |
| Application | CarSalesSystem |

---

### Tagging Enforcement Strategy

> Explain how tag consistency would be enforced in a production environment.

---

## 2.2 Resource Tagging Evidence

### Tagged Resources

- EC2 Instances
- RDS Instances
- Lambda Functions
- S3 Buckets
- API Gateway
- EFS File Systems

---

### Screenshot Evidence

> Insert screenshots showing tags applied to at least 3 resource types.

---

## 2.3 Cost Allocation Tags Activation

### Billing Console Activation

> Insert screenshots showing activated cost allocation tags.

---

## 2.4 Cost Monitoring Tools

### Tool 1 — Cost Explorer

> Describe usage and configuration.

### Tool 2 — AWS Budgets

> Describe usage and configuration.

### Tool 3 — Cost Anomaly Detection (Optional)

> Describe usage and configuration.

---

## 2.5 Baseline Cost Breakdown

### Cost Breakdown Screenshot

> Insert screenshot filtered by project tags.

---

### Top Cost Drivers

| Rank | Service | Estimated Cost | Observation |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

### Cost Analysis Observation

> Write a short paragraph explaining major cost drivers and unexpected costs.

---

# Section 3 — MH-COST-A — Cost Control & Action

## 3.1 Automated Cost Guard Overview

### Purpose

> Explain the automated cost control mechanism.

---

## 3.2 Stop Lambda Function

### Lambda Description

> Describe what the Lambda does.

### Target Resources

- EC2
- RDS
- Other:

---

### Lambda Logic

> Explain stop conditions and filtering logic.

---

### IAM Least-Privilege Policy

> Describe permissions granted to the Lambda role.

---

### Lambda Screenshot

> Insert screenshots of Lambda configuration and code.

---

## 3.3 EventBridge Scheduled Trigger

### Schedule Configuration

| Setting | Value |
|---|---|
| Schedule Type | |
| Cron Expression | |
| Trigger Frequency | |

---

### EventBridge Screenshot

> Insert screenshots of EventBridge rule/scheduler.

---

## 3.4 Demonstrated Automated Stop Action

### Test Scenario

> Describe the resource selected for testing.

---

### Before State

> Insert screenshot showing resource in running state.

---

### After State

> Insert screenshot showing resource stopped by automation.

---

### CloudTrail Evidence

> Insert CloudTrail screenshots for StopInstances or StopDBInstance.

---

## 3.5 Budget-Driven Automation

### AWS Budget Configuration

| Setting | Value |
|---|---|
| Budget Type | |
| Budget Limit | $150 |
| Notification Threshold | |

---

### SNS Integration

> Describe SNS topic and subscription setup.

---

### Lambda Integration

> Explain how SNS triggers the Lambda.

---

### Test Message Demonstration

> Insert screenshots of SNS test publish and automation result.

---

## 3.6 Cost Data Latency ADR

### ADR Title

> Cost Data Delay in AWS Budgets

---

### Problem

> Explain AWS cost reporting delay.

### Decision

> Explain why scheduled automation was added.

### Expected Production Behavior

> Explain production expectations.

---

# Section 4 — MH-OBS — CloudWatch Observability

## 4.1 Monitoring Strategy

### Application Components Monitored

- API Gateway
- Lambda
- Database
- AI Recommendation Layer
- Vehicle Search Service
- Other:

---

## 4.2 CloudWatch Dashboard

### Dashboard Overview

> Describe dashboard purpose and widgets.

---

### Custom Metric Widget

| Metric Name | Namespace | Unit |
|---|---|---|
| | | |

---

### Standard Metrics

- Lambda Errors
- API Gateway 4XX/5XX Errors
- RDS Connections
- EC2 CPU Utilization

---

### Dashboard Screenshot

> Insert dashboard screenshot.

---

## 4.3 Custom Metric Implementation

### Metric Description

> Explain what business/application metric is being tracked.

---

### PutMetricData Code Snippet

```python
# Insert PutMetricData code here
```

---

## 4.4 CloudWatch Alarm

### Alarm Configuration

| Setting | Value |
|---|---|
| Alarm Name | |
| Metric | |
| Threshold | |
| Evaluation Period | |
| Alarm Action | |

---

### Alarm State

- OK
- ALARM

---

### Alarm Screenshot

> Insert screenshots showing alarm configuration and state.

---

## 4.5 CloudWatch Logs Insights

### Log Group

> Specify log group used.

---

### Saved Query Name

> Enter saved query name.

---

### Query Purpose

> Explain what the query analyzes.

---

### Logs Insights Query

```sql
# Insert Logs Insights query here
```

---

### Query Results Screenshot

> Insert screenshot showing query results.

---

# Section 5 — MH-SEC — Self-Healing Security Guard

## 5.1 Security Guard Overview

### Security Threat Addressed

> Explain the security misconfiguration detected.

---

### Potential Blast Radius

> Explain production impact if not remediated.

---

## 5.2 Automated Remediation Lambda

### Lambda Purpose

> Describe remediation logic.

---

### Detection Method

- Security Group Detection
- S3 Public Access Detection
- Other:

---

### Remediation Action

> Explain how the Lambda fixes the issue.

---

### IAM Least-Privilege Policy

> Describe permissions granted.

---

### Lambda Screenshot

> Insert screenshots of Lambda code/configuration.

---

## 5.3 Trigger Mechanism

### Trigger Type

- EventBridge Rule
- EventBridge Scheduler

---

### Event Source

> Describe CloudTrail/API event or schedule.

---

### Trigger Screenshot

> Insert screenshots of trigger configuration.

---

## 5.4 Demonstrated Detect → Fix Loop

### Security Violation Scenario

> Describe the intentional security violation.

---

### Before State (Insecure)

> Insert screenshot showing insecure configuration.

---

### Automated Remediation

> Explain remediation flow.

---

### After State (Remediated)

> Insert screenshot showing fixed configuration.

---

### CloudTrail Evidence

> Insert screenshots showing remediation API calls.

---

# Section 5.5 — Supporting Preventive Control

## Selected Path

- Path A — KMS Customer Managed Key
- Path B — S3 Block Public Access + Deny Policy
- Path C — IAM Access Analyzer

---

## Path A — KMS Customer Managed Key

### CMK Configuration

| Setting | Value |
|---|---|
| Key Alias | |
| Rotation Enabled | |
| Applied Service | |

---

### Encryption Usage Evidence

> Insert screenshots and CloudTrail evidence.

---

## Path B — S3 Block Public Access + Deny Policy

### Account-Level Block Public Access

> Insert screenshots showing all settings enabled.

---

### Bucket Policy

```json
// Insert deny policy here
```

---

### Denied Request Evidence

> Insert screenshots showing denied request.

---

## Path C — IAM Access Analyzer

### Findings

> Describe findings identified.

---

### Triage Decision

> Explain whether the access was intended or not.

---

## 5.6 Security-Cost Trade-Off

### Cost Impact

> Explain the cost of the selected control.

---

### Business Justification

> Explain why the cost is justified.

---

# Section 6 — Deployment Demonstration Summary

## MH-COST-V Verification

> Summary of demonstrated cost visibility features.

---

## MH-COST-A Verification

> Summary of automated cost control demonstration.

---

## MH-OBS Verification

> Summary of monitoring and observability evidence.

---

## MH-SEC Verification

> Summary of self-healing security remediation evidence.

---

# Section 7 — Lessons Learned

## Operational Challenges

> Describe major operational issues encountered.

---

## Improvements Made

> Describe optimizations and fixes implemented.

---

## Production Recommendations

> Explain what would be improved in a real production environment.

---

# Section 8 — Stretch Goals (Optional)

## 8.1 gp2 → gp3 Migration

### Before Migration

> Document gp2 configuration and metrics.

---

### After Migration

> Document gp3 configuration and metrics.

---

### Cost & Performance Analysis

> Compare before/after results.

---

## 8.2 Trusted Advisor Remediations

### Finding 1

| Item | Details |
|---|---|
| Finding | |
| Action Taken | |
| Result | |

---

### Finding 2

| Item | Details |
|---|---|
| Finding | |
| Action Taken | |
| Result | |

---

## 8.3 RI / Savings Plans Analysis

### Break-Even Analysis

> Document calculations and recommendation.

---

## 8.4 Wasteful → Changed Reflection

> Write a 100–150 word reflection describing waste identified and optimization performed.

---

## 8.5 Cost Anomaly Automation

### Configuration

> Describe Cost Anomaly Detection setup.

---

### Alert Flow

> Explain EventBridge → SNS integration.

---

## 8.6 Config Conformance Pack

### Conformance Pack Used

> Specify selected conformance pack.

---

### Compliance Findings

> Summarize important findings.

---

# Final Notes

## Repository Commit Information

| Item | Value |
|---|---|
| Repository URL | |
| Commit Hash | |
| Evidence Pack File | docs/W6_evidence.md |

---

## Submission Checklist

- [ ] MH-COST-V completed
- [ ] MH-COST-A completed
- [ ] MH-OBS completed
- [ ] MH-SEC completed
- [ ] Evidence screenshots added
- [ ] CloudTrail evidence added
- [ ] Budget automation tested
- [ ] Alarm state verified
- [ ] Cost allocation tags activated
- [ ] Repository link submitted
- [ ] Total AWS account cost below $150