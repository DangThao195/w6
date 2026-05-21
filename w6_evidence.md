# W6 Evidence Pack — Operations Hardening & Cost-Aware Cloud

---

# Group Information

| Item | Details |
|---|---|
| Group ID | G2 |
| Project Name | Car Sales System |
| Business Domain | Automotive / Car Sales |
| Presentation Date |22/05/2026 |

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

The project is a cloud-based car sales management system designed to support vehicle dealerships and customers through a scalable and intelligent web platform. The system allows administrators and sales staff to manage vehicle inventory, customer inquiries, and sales operations, while customers can browse available cars, search based on preferences, and interact with an AI-powered chatbot for recommendations and support.

Target users include:

- Customers searching for vehicles online
- Sales staff managing inquiries and transactions
- System administrators maintaining infrastructure and operations

The platform was designed using AWS cloud-native services with a strong focus on scalability, security, automation, and intelligent customer interaction.

### Main Features

- Car listing management
- Vehicle search and filtering
- Customer inquiry management
- Sales management
- AI-powered recommendation/chatbot system
- User authentication and authorization
- Automated backup and recovery
- Secure API integration
- Monitoring and operational logging

---

## 1.2 Architecture Summary (W1 → W5)

### W1 — Core Architecture

The initial architecture followed a standard 3-tier cloud architecture:

1. Presentation Layer
   - Frontend web application hosted behind CloudFront and Application Load Balancer (ALB)
   - Public access handled through secure HTTPS routing

2. Application Layer
   - EC2 instances deployed in private subnets
   - Auto Scaling Group used for scalability and high availability
   - Application logic processed through Node.js backend services

3. Data Layer
   - Database services isolated inside private subnets (using DocumentDB)
   - Storage and persistent services separated from application instances

The architecture was distributed across multiple Availability Zones to improve fault tolerance and availability.

### W2 — Storage & Identity

Week 2 focused on storage configuration and identity/security management.

Storage services implemented:

- Amazon EBS for EC2 persistent storage
- Amazon EFS for shared file storage across multiple EC2 instances
- Amazon S3 for static assets, backup files, and storage operations

Identity and security setup included:

- IAM Roles for EC2, Lambda, and application services
- Least-privilege access policies
- Security Groups and NACL configuration
- Secure access control between frontend, backend, and database layers
- Controlled resource permissions using IAM policies

This week established the foundation for secure infrastructure operations and shared storage management.

### W3 — Database & AI Layer

Week 3 introduced the database infrastructure and AI-powered application layer.

Implemented services included:

- Amazon DocumentDB for application database storage
- AWS Lambda functions for serverless backend processing
- Amazon Bedrock integration for generative AI capabilities
- Knowledge Base integration for chatbot retrieval and contextual responses

#### Database Layer

Amazon DocumentDB was used as the primary database service for storing:

- Vehicle inventory data
- Customer inquiries
- Sales-related information

The database was deployed inside private subnets within the VPC to improve security and isolate backend resources from direct public access.

#### AI & Automation Layer

The intelligent application layer integrated Amazon Bedrock with backend Lambda functions to support AI-powered customer interactions.

Implemented AI capabilities included:

- Vehicle recommendation support
- Context-aware chatbot responses
- Retrieval-Augmented Generation (RAG)
- Knowledge Base querying
- Backend orchestration using Lambda functions

This architecture enabled intelligent customer support and automated recommendation functionality while maintaining scalable and serverless backend operations.

### W4 — Intelligent Automation

Week 4 focused on building the AI chatbot and intelligent orchestration system.

The chatbot system included:

- Bedrock Agent integration
- Retrieval-based response generation
- Knowledge Base querying
- Lambda-powered tool execution
- Multi-step orchestration workflow

Core chatbot capabilities:

- Vehicle recommendation assistance
- Customer support automation
- Contextual conversation handling
- Intelligent retrieval from stored data
- API-based action execution using Lambda tools

Agent workflow architecture:

1. User submits a request
2. Bedrock Agent processes intent
3. Knowledge Base retrieves related information
4. Lambda tools execute backend operations if required
5. Final response is generated and returned to the user

The system improved customer interaction by automating responses and reducing manual support workload.

### W5 — Network Hardening

Week 5 focused on operational security, backup strategy, and network protection.

Implemented improvements included:

#### Network Security

- VPC segmentation across multiple Availability Zones
- Public and private subnet isolation
- AWS Network Firewall integration
- Secure routing through inspection layers
- NAT Gateway configuration for outbound traffic

#### API & Access Security

- API Gateway integration for controlled API exposure
- Usage plans and throttling configuration
- IAM-based permission control
- Secure communication between services

#### Backup & Restore

- AWS Backup Plan implementation
- EBS snapshot automation
- EFS backup and restore validation
- Data recovery testing and integrity verification

#### Scalability & Reliability

- Auto Scaling Group configuration
- Load balancing using Application Load Balancer
- Multi-AZ deployment for high availability
- Monitoring and logging preparation for operational visibility

The infrastructure was hardened to improve resilience against network threats, operational failures, and scaling challenges.

---

## 1.3 W5 Feedback Improvements 

Several issues identified during the W5 review were resolved and improved:

### Backup & Restore Improvements

- Replaced placeholder EBS volumes with actual tagged production volumes inside the backup plan
- Updated backup selection strategy to automatically include newly created instances during scale-out
- Re-ran restore validation to ensure restored data matched original written files
- Verified backup integrity after restore testing

### Network Firewall Improvements

- Extended AWS Network Firewall coverage beyond Zone A
- Removed direct outbound NAT access from private subnets in other Availability Zones
- Improved inspection routing consistency across multi-AZ architecture

### IAM & Secrets Management Improvements

- Removed database connection strings from Lambda environment variables
- Integrated AWS Secrets Manager for secure secret retrieval
- Updated IAM permissions to allow secure runtime access to secrets

### Concurrency & API Optimization

- Increased Lambda Reserved Concurrency after measuring baseline traffic
- Adjusted concurrency configuration to align with API Gateway usage plan limits
- Reduced risk of unintended request throttling during higher traffic loads

### Documentation & Testing Improvements

- Added CloudFront user-flow screenshots for frontend access validation
- Standardized testing documentation terminology from:
  - “Expected Result”
  to
  - “Observed Result”

---

# Section 2 — MH-COST-V — Cost Visibility & Attribution

## 2.1 Tagging Strategy

### Required Tag Keys

| Tag Key | Example Value | Purpose |
|---|---|---|
| Owner | teamlead@email.com | Resource ownership |
| Environment | dev | Environment identification |
| CostCenter | G2 | Team cost tracking |
| Application | CarSalesSystem | Application grouping |

---

### Allowed Tag Values

| Tag Key | Allowed Values |
|---|---|
| Owner | |
| Environment | dev, test, prod |
| CostCenter | G2 |
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