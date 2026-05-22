# W6 Evidence Pack — Operations Hardening & Cost-Aware Cloud

---

# Group Information

| Item              | Details                |
| ----------------- | ---------------------- |
| Group ID          | G2                     |
| Project Name      | Car Sales System       |
| Business Domain   | Automotive / Car Sales |
| Presentation Date | 22/05/2026             |

---

# Team Members

| Name                    | Student ID  |
| ----------------------- | ----------- |
| Ngo Huu Tai             | XB-DN26-008 |
| Mai Phuoc Khoa          | XB-DN26-033 |
| Nguyen Tien Hoang Thinh | XB-DN26-047 |
| Dang Thi Ngoc Thao      | XB-DN26-055 |
| Nguyen Phu Trieu        | XB-DN26-070 |
| Nguyen Hung Thinh       | XB-DN26-077 |
| Huynh Ba Huan           | XB-DN26-106 |
| Nguyen Van Tuan Anh     | XB-DN26-112 |
| Le Hoang Viet           | XB-DN26-134 |
| Hoang Cong Tri Dung     | XB-DN26-148 |

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

![alt text](image-66.png)

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

**Vault - Backup Plan - Restore Job**

![alt text](image-6.png)
![alt text](image-7.png)
![alt text](image-8.png)

**EFS Restore**

![alt text](image-5.png)
![alt text](image-4.png)

**ABS Restore**

![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image.png)

### Network Firewall Improvements

- Extended AWS Network Firewall coverage beyond Zone A
- Removed direct outbound NAT access from private subnets in other Availability Zones
- Improved inspection routing consistency across multi-AZ architecture

![alt text](image-9.png)
![alt text](image-10.png)
![alt text](image-11.png)
![alt text](image-12.png)
![alt text](image-13.png)

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

### Lambda & API Gateway Feedback Improvements

This subsection documents the W6 improvements made specifically for the Lambda + API Gateway implementation after the W5 feedback.

#### 1. Removed DocumentDB connection string from Lambda environment variables

The Lambda function no longer stores the full DocumentDB connection string or password in environment variables. It only stores non-sensitive runtime configuration:

```text
SECRET_NAME=group2/car/docdb
DB_NAME=social
USE_MOCK_DATA=false
```

This reduces credential exposure and allows database credentials to be rotated from AWS Secrets Manager without redeploying the Lambda function.

![W5 Fix 01 — Lambda environment variables without MongoDB URI](./images/w5-fix-01-lambda-env-no-mongodb-uri.png)

#### 2. Stored DocumentDB credentials in AWS Secrets Manager

The DocumentDB connection details are stored in AWS Secrets Manager under:

```text
group2/car/docdb
```

The secret value is not exposed in the evidence screenshot.

![W5 Fix 02 — Secrets Manager DocumentDB secret](./images/w5-fix-02-secrets-manager-docdb-secret.png)

#### 3. Updated Lambda code to retrieve the secret at runtime

The Lambda code was updated to read the secret using AWS SDK at runtime. The function uses `SECRET_NAME`, calls `GetSecretValueCommand`, parses the secret, and then builds the DocumentDB connection string internally.

![W5 Fix 03 — Lambda code reads secret using GetSecretValueCommand](./images/w5-fix-03-lambda-code-get-secret-value.png)

#### 4. Added IAM permission to read the specific secret

The Lambda execution role includes a dedicated policy for reading the DocumentDB secret.

```json
{
  "Effect": "Allow",
  "Action": ["secretsmanager:GetSecretValue"],
  "Resource": "arn:aws:secretsmanager:us-west-2:462972379716:secret:group2/car/docdb*"
}
```

This follows least-privilege access because the Lambda only needs permission to read the specific DocumentDB secret.

![W5 Fix 04 — Lambda role includes secret read policy](./images/w5-fix-04-iam-get-secret-value-policy.png)

#### 5. Deployed Lambda inside private VPC subnets

The Lambda function was configured inside the `car-ecommerce-vpc` private application subnets. This allows the Lambda to connect privately to DocumentDB without exposing the database to the public internet.

![W5 Fix 05 — Lambda VPC configuration](./images/w5-fix-05-lambda-vpc-config.png)

#### 6. Rebuilt API Gateway routes for Lambda integration

The REST API exposes only the required demo endpoints:

```text
GET  /demo/products
GET  /demo/products/{id}
POST /demo/request
OPTIONS routes for CORS preflight
```

![W5 Fix 06 — API Gateway resources and routes](./images/w5-fix-06-api-gateway-routes.png)

#### 7. Configured API Gateway Usage Plan

The API Gateway Usage Plan was configured to protect the Lambda from excessive traffic:

```text
Rate limit: 10 requests/second
Burst limit: 20 requests
Stage: prod
```

![W5 Fix 07 — API Gateway usage plan throttle](./images/w5-fix-07-usage-plan-throttle.png)

#### 8. Increased Lambda Reserved Concurrency

The Lambda Reserved Concurrency was increased to:

```text
Reserved concurrency = 12
```

This is aligned better with the API Gateway Usage Plan and reduces the risk of valid traffic being throttled by Lambda too early.

![W5 Fix 08 — Lambda reserved concurrency set to 12](./images/w5-fix-08-lambda-reserved-concurrency.png)

#### 9. Validated API through CloudFront domain

The API was tested through the CloudFront application domain. The response returned HTTP 200 and live DocumentDB-backed data.

![W5 Fix 09 — CloudFront API request returns 200](./images/w5-fix-09-api-test-200.png)

#### 10. Removed API key from browser requests

The frontend was updated so the browser no longer sends `x-api-key` directly. Instead, the request goes to the application domain:

```text
https://nvtank.dev/demo/products?limit=10
```

The API key is injected by CloudFront as an origin custom header when forwarding requests to API Gateway.

![W5 Fix 10 — Browser Network request without x-api-key](./images/w5-fix-10-browser-network-no-api-key.png)

#### Summary

The W5 feedback was addressed by moving database credentials out of Lambda environment variables, integrating AWS Secrets Manager, granting least-privilege secret access, deploying Lambda inside private VPC subnets, aligning Reserved Concurrency with the API Gateway Usage Plan, and removing direct API key exposure from browser requests.

---

# Section 2 — MH-COST-V — Cost Visibility & Attribution

## 2.1 Tagging Strategy

### Required Tag Keys

| Tag Key | Example Value | Purpose |
|---|---|---|
| **Owner** | `huutai.ngo2409@gmail.com` | Identifies the accountable person if a resource is misconfigured, expensive, or left running. |
| **Environment** | `dev` | Separates the workshop/development stack from future test, staging, or production resources. |
| **CostCenter** | `G2` | Attributes cloud cost to the correct project group (Group 2). |
| **Application** | `AAP-CarApp` | Groups all resources belonging to the same workload across different AWS services. |

---

### Allowed Tag Values

| Tag Key | Allowed Values |
|---|---|
| **Owner** | `huutai.ngo2409@gmail.com` |
| **Environment** | `dev` *(Strictly lowercase; Dev, DEV, or development are unauthorized for this evidence pack)* |
| **CostCenter** | `G2` |
| **Application** | `AAP-CarApp` |

---

### Tagging Enforcement Strategy

In a production enterprise environment, maintaining manual tag compliance is highly error-prone. To guarantee full financial accountability, the following layered automation enforcement strategy is designed:
1. **Organizational Governance:** Implement **AWS Organizations Tag Policies** to globally standardize capitalizations and restrict allowed values, blocking non-compliant structures at the root.
2. **Infrastructure as Code (IaC) Defaults:** Leverage **Terraform** default provider tags or **AWS CDK** aspects to inject mandatory `Owner`, `Environment`, `CostCenter`, and `Application` tags automatically into every provisioned asset.
3. **CI/CD Quality Gates:** Embed automated validation linter checks (such as `tflint` or `checkov`) inside GitHub Actions deployment pipelines to break the build if mandatory tags are missing.
4. **Preventative Controls:** Deploy **AWS Organizations Service Control Policies (SCPs)** or strict IAM explicit deny boundaries to actively reject the creation of high-cost services (EC2, RDS, Network Firewalls, NAT Gateways) unless all four target allocation tags are attached to the API creation request.
5. **Continuous Compliance Auditing:** Run scheduled **AWS Config Rules** paired with a custom **AWS Lambda script** to continually scan existing topologies, automatically marking any untagged assets for remediation and sending instant Slack/Email alerts to the respective group.

---

## 2.2 Resource Tagging Evidence

### Tagged Resources

- [x] EC2 Instances
- [x] DocumentDB
- [x] Lambda Functions
- [x] S3 Buckets
- [x] API Gateway
- [x] EFS File Systems
- ...

---

### Screenshot Evidence

![alt text](image-32.png)
![alt text](image-33.png)
![alt text](image-34.png)
![alt text](image-35.png)
![alt text](image-36.png)

---

## 2.3 Cost Monitoring Tools

### Tool 1 — Cost Explorer

* **Usage:** Used as the primary visualization engine to dissect cost attribution, track spending trends, and analyze the financial impact of the Week 6 redeployed application stack.
* **Configuration:** * **Date Range:** Month-to-Date (MTD) / Last 7 Days.
    * **Granularity:** Daily.
    * **Dimension / Group by:** Service.

![alt text](image-37.png)

### Tool 2 — AWS Budgets

* **Usage:** Functions as an automated financial guardrail to enforce cost control, preventing the sandbox environment from exceeding the weekly allocated cap.
* **Configuration:**
    * **Budget Name:** `group2-budget`
    * **Budget Type:** Cost Budget.
    * **Budget Limit / Period:** USD 150.00 / Daily tracking baseline.
    * **Alert Thresholds:** Triggered at **50%**, **80%**, and **100%** of actual cost.
    * **Notification Subscribers:** `anhnvt.23ite@vku.udn.vn` and `huutai.ngo2409@gmail.com`.

![alt text](image-38.png)

### Tool 3 — Cost Anomaly Detection (Optional)

* **Usage:** Leverages AWS machine learning models to continuously inspect underlying usage logs, dynamically identifying unusual spend deviations that standard static thresholds might miss.
* **Configuration:**
    * **Monitor Name & Type:** `Group-2-monitor` / AWS Services (All services scope).
    * **Alert Subscription & Threshold:** `group2` / Alert triggered when an anomaly exceeds a threshold of **USD 10.00**.
    * **Alert Frequency & Subscriber:** Daily summaries / Sent directly to `huutai.ngo2409@gmail.com`.

![alt text](image-39.png)
![alt text](image-40.png)

---

## 2.4 Baseline Cost Breakdown

### Cost Breakdown Screenshot

![alt text](image-41.png)

---

### Top Cost Drivers

| Rank | Service | Estimated Cost | Observation |
|---|---|---|---|
| **1** | `Network Firewall` | $10.67 | Represents the single highest cost driver in the architecture. This is caused by the persistent operational status of the firewall endpoints and deep inspection layer deployed during the Week 5 network hardening phase. The cost accumulates continuously even when application search traffic is near zero. |
| **2** | `EC2-Other` | $1.59 | This category aggregates non-compute auxiliary infrastructure running alongside instances. It encapsulates storage fees for EBS volumes, automated snapshots, NAT Gateway traffic processing, unassigned Elastic IPs, or cross-AZ data transfer overhead. |
| **3** | `DocumentDB` | $1.58 | Represents the underlying cost for the fully provisioned DocumentDB cluster required to support persistent vehicle metadata, customer queries, and transaction tables. As a database instance, it generates costs continuously as long as it remains provisioned. |

---

### Cost Analysis Observation

- Following a detailed analysis of the AWS Cost Explorer data for the `AAP-CarApp` workload, the total baseline spend is heavily dominated by Network Firewall infrastructure ($10.67), followed by EC2-Other utility costs ($1.59) and DocumentDB instances ($1.58). 

- The primary cost driver, Network Firewall, behaves as a fixed overhead that reflects strict security hardening rules rather than scaling with actual application usage. This is standard behavior for perimeter defense architectures, though expensive for isolated development environments. The secondary driver, EC2-Other, highlights that ancillary storage (EBS) and data transfer paths generate billable metrics independently of whether the core EC2 instance runtimes are active. DocumentDB acts as an expected operational cost for stateful persistence, representing an area where infrastructure right-sizing or off-hours cluster suspension can be applied to capture further cost efficiencies.

---

# Section 3 — MH-COST-A — Cost Control & Action

## 3.1 Objective

The goal of **MH-COST-A** is to prove that our system does not only observe cloud cost, but can also automatically take action to reduce unnecessary spending.

For this workload, we implemented an **Automated Cost Guard** using AWS Lambda. The Lambda function scans running EC2 instances and automatically stops compute resources that should not continue running in a development environment.

The cost guard is connected to two trigger paths:

```text
Path 1 — Scheduled cost control

EventBridge Scheduler
→ CostGuard_Stop_Compute Lambda
→ Stop EC2 instances without keep=true
```

```text
Path 2 — Cost-driven control

AWS Budget $150
→ SNS Topic
→ CostGuard_Stop_Compute Lambda
→ Stop EC2 instances without keep=true
```

This proves that our application stack has an operational cost-control mechanism instead of only a passive cost alert.

---

## 3.2 Automated Cost Guard Lambda

### 3.2.1 Lambda function

| Item            | Value                                   |
| --------------- | --------------------------------------- |
| Lambda name     | `CostGuard_Stop_Compute`                |
| Runtime         | Python                                  |
| AWS SDK         | `boto3`                                 |
| Target resource | EC2 instances                           |
| Action          | `StopInstances`                         |
| Protection rule | Skip EC2 instances with tag `keep=true` |
| Environment     | `dev`                                   |
| Application     | `AAP-CarApp`                            |
| CostCenter      | `G2`                                    |

The Lambda function scans all EC2 instances in the `running` state. For each instance, it checks whether the instance has the tag:

```text
keep=true
```

If the tag is missing, the Lambda treats the instance as a cost-risk resource and stops it using the EC2 `StopInstances` API.

If the tag exists, the Lambda logs the instance as protected and skips it. This prevents important infrastructure such as the bastion host from being stopped accidentally.

### 3.2.2 Lambda code summary

```python
import boto3
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)


![alt text](image-21.png)
![alt text](image-23.png)
![alt text](ec2_before.jpg)
![alt text](image-24.png)

    logger.info("Scanning for running EC2 instances...")

    ec2_response = ec2.describe_instances(
        Filters=[{'Name': 'instance-state-name', 'Values': ['running']}]
    )

    for reservation in ec2_response.get('Reservations', []):
        for instance in reservation.get('Instances', []):
            instance_id = instance['InstanceId']
            tags = instance.get('Tags', [])

            has_keep = any(
                t.get('Key', '').lower() == 'keep'
                and t.get('Value', '').lower() == 'true'
                for t in tags
            )

            if not has_keep:
                logger.info(f"Stopping EC2 {instance_id} - Missing 'keep=true' tag.")
                ec2.stop_instances(InstanceIds=[instance_id])
            else:
                logger.info(f"Skipping EC2 {instance_id} - Protected by 'keep=true' tag.")

    return {"status": "Automated Cost Guard Executed (EC2 Only)"}
```

### Evidence

![MH-COST-A-01 Lambda code for CostGuard_Stop_Compute](./images/mh-cost-a-01-lambda-code.png)

![MH-COST-A-02 Lambda manual test succeeded](./images/mh-cost-a-02-lambda-test-success.png)

![MH-COST-A-03 Lambda CloudWatch logs showing scanned and stopped EC2](./images/mh-cost-a-03-lambda-logs-stop-ec2.png)

---

## 3.3 IAM Least-Privilege Role

The Lambda function uses a dedicated IAM role with only the permissions required for this cost guard.

Required permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DescribeEC2Instances",
      "Effect": "Allow",
      "Action": ["ec2:DescribeInstances"],
      "Resource": "*"
    },
    {
      "Sid": "StopEC2Instances",
      "Effect": "Allow",
      "Action": ["ec2:StopInstances"],
      "Resource": "*"
    },
    {
      "Sid": "WriteCloudWatchLogs",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    }
  ]
}
```

This role does not use `AdministratorAccess`. It only allows the Lambda to describe EC2 instances, stop EC2 instances, and write logs to CloudWatch.

### Evidence

![MH-COST-A-04 Lambda IAM role least privilege policy](./images/mh-cost-a-04-iam-least-privilege-policy.png)

![MH-COST-A-05 Lambda execution role attached to CostGuard_Stop_Compute](./images/mh-cost-a-05-lambda-execution-role.png)

---

## 3.4 Component (b) — Daily EventBridge Schedule

To make the cost guard automatic, we configured an EventBridge Scheduler rule to invoke the Lambda every day.

| Item                | Value                                                |
| ------------------- | ---------------------------------------------------- |
| Scheduler name      | `cost-guard-daily-schedule`                          |
| Target              | `CostGuard_Stop_Compute`                             |
| Schedule type       | Recurring                                            |
| Schedule expression | Daily cron                                           |
| Purpose             | Automatically stop unprotected dev compute resources |
| State               | Enabled                                              |

Example production behavior:

```text
Every day at the scheduled time
→ EventBridge invokes CostGuard_Stop_Compute
→ Lambda scans EC2 running instances
→ Lambda stops EC2 instances without keep=true
```

This prevents development EC2 instances from running idle overnight and helps keep the weekly account cost under the $150 cap.

### Evidence

![MH-COST-A-06 EventBridge daily schedule overview](./images/mh-cost-a-06-eventbridge-daily-schedule.png)

![MH-COST-A-07 EventBridge schedule target Lambda CostGuard_Stop_Compute](./images/mh-cost-a-07-eventbridge-target-lambda.png)

---

## 3.5 Component (c) — Demonstrated Action

### 3.5.1 Before state

Before invoking the automation, at least one EC2 instance was running and did not have the protection tag:

```text
keep=true
```

Target examples:

| Instance name              | Instance ID           | Initial state | `keep=true` |
| -------------------------- | --------------------- | ------------- | ----------- |
| `car-backend-huutai-a`     | `i-01669f35cc53b5899` | Running       | Missing     |
| Additional unprotected EC2 | `i-04caa3aea23b844ed` | Running       | Missing     |

Protected instance example:

| Instance name              | Instance ID           | State   | Reason                   |
| -------------------------- | --------------------- | ------- | ------------------------ |
| `bastion-backend-huutai-a` | `i-0b11cacc05b1a432e` | Running | Protected by `keep=true` |

### Evidence — Before

![MH-COST-A-08 EC2 before automation running state](./images/mh-cost-a-08-ec2-before-running.png)

![MH-COST-A-09 EC2 tag view showing missing keep tag or protected keep=true tag](./images/mh-cost-a-09-ec2-tags-keep-true.png)

### 3.5.2 Lambda execution

The Lambda was invoked and scanned running EC2 instances.

The execution log showed two types of behavior:

```text
Stopping EC2 i-01669f35cc53b5899 - Missing 'keep=true' tag.
Stopping EC2 i-04caa3aea23b844ed - Missing 'keep=true' tag.
Skipping EC2 i-0b11cacc05b1a432e - Protected by 'keep=true' tag.
```

This proves that the Lambda applied selective cost control instead of blindly stopping every instance.

The log evidence for this execution is already captured in:

```text
mh-cost-a-03-lambda-logs-stop-ec2.png
```

### 3.5.3 After state

After the Lambda executed, the unprotected EC2 instances moved from `running` to `stopped`.

| Instance name              | Instance ID           | Final state |
| -------------------------- | --------------------- | ----------- |
| `car-backend-huutai-a`     | `i-01669f35cc53b5899` | Stopped     |
| Additional unprotected EC2 | `i-04caa3aea23b844ed` | Stopped     |
| `bastion-backend-huutai-a` | `i-0b11cacc05b1a432e` | Running     |

The protected bastion instance remained running, proving that the `keep=true` safeguard worked correctly.

### Evidence — After

![MH-COST-A-11 EC2 after automation stopped state](./images/mh-cost-a-11-ec2-after-stopped.png)

---

## 3.6 CloudTrail Evidence — StopInstances

CloudTrail recorded the EC2 `StopInstances` API call.

This proves that the EC2 stop action was performed by AWS automation through the Lambda execution role, not manually by a human user.

Important fields to capture:

| Field              | Expected value                                     |
| ------------------ | -------------------------------------------------- |
| Event name         | `StopInstances`                                    |
| Event source       | `ec2.amazonaws.com`                                |
| User identity type | `AssumedRole`                                      |
| Role / session     | Lambda execution role for `CostGuard_Stop_Compute` |
| Resource           | EC2 instance ID that was stopped                   |
| AWS Region         | `us-west-2`                                        |

Example CloudTrail lookup command:

```bash
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=StopInstances \
  --region us-west-2 \
  --max-results 5
```

### Evidence

![MH-COST-A-12 CloudTrail Event History filtered by StopInstances](./images/mh-cost-a-12-cloudtrail-stopinstances-list.png)

![MH-COST-A-13 CloudTrail StopInstances event detail](./images/mh-cost-a-13-cloudtrail-stopinstances-detail.png)

![MH-COST-A-14 CloudTrail event record JSON showing Lambda assumed role](./images/mh-cost-a-14-cloudtrail-event-json-assumed-role.png)

---

## 3.7 Component (d) — Cost-Driven Path: AWS Budget → SNS → Lambda

### 3.7.1 AWS Budget

We configured an AWS Budget with a hard workshop cost cap of:

```text
$150
```

The budget is connected to SNS so that cost-related events can trigger the same Lambda cost guard.

| Item                | Value                                |
| ------------------- | ------------------------------------ |
| Budget name         | `group2-budget` or `W6-150-Cost-Cap` |
| Budget amount       | `$150`                               |
| Budget type         | Cost budget                          |
| Notification type   | Actual cost                          |
| Notification target | SNS topic                            |
| SNS topic           | `group2-cost-guard-budget-topic`     |

### Evidence — Budget

![MH-COST-A-15 AWS Budget overview 150 USD](./images/mh-cost-a-15-budget-overview-150.png)

![MH-COST-A-16 AWS Budget alert notification to SNS topic](./images/mh-cost-a-16-budget-sns-notification.png)

---

### 3.7.2 SNS topic and Lambda subscription

The SNS topic is used as the event bridge between AWS Budgets and the Lambda cost guard.

```text
AWS Budget
→ SNS topic
→ CostGuard_Stop_Compute Lambda
```

| Item                  | Value                                             |
| --------------------- | ------------------------------------------------- |
| SNS topic name        | `group2-cost-guard-budget-topic`                  |
| Topic type            | Standard                                          |
| Region                | `us-west-2`                                       |
| Purpose               | Trigger cost guard when budget alert is published |
| Subscription protocol | AWS Lambda                                        |
| Subscription endpoint | `CostGuard_Stop_Compute`                          |
| Subscription status   | Confirmed                                         |

The screenshot for this section includes both the SNS topic overview and the Lambda subscription, so a separate SNS subscription screenshot is not required.

### Evidence — SNS topic and Lambda subscription

![MH-COST-A-17 SNS topic overview and Lambda subscription confirmed](./images/mh-cost-a-17-sns-topic-overview.png)

---

### 3.7.3 Lambda permission for SNS invoke

The Lambda resource policy allows SNS to invoke the function.

Expected principal:

```text
sns.amazonaws.com
```

Expected source ARN:

```text
arn:aws:sns:us-west-2:462972379716:group2-cost-guard-budget-topic
```

### Evidence

![MH-COST-A-19 Lambda resource policy allows SNS invoke](./images/mh-cost-a-19-lambda-sns-invoke-permission.png)

---

## 3.8 Manual SNS Publish Demonstration

Because AWS Budgets cost data can be delayed, the real Budget threshold may not trigger during the 48-hour workshop window. To prove that the cost-driven automation path is wired correctly, we manually published a test message to the SNS topic.

Command used:

```bash
aws sns publish \
  --topic-arn arn:aws:sns:us-west-2:462972379716:group2-cost-guard-budget-topic \
  --message '{"source":"manual-sns-test","reason":"simulate-budget-alert"}' \
  --region us-west-2
```

Expected result:

```text
SNS publish succeeds
→ SNS invokes CostGuard_Stop_Compute
→ Lambda scans running EC2
→ Lambda stops EC2 instances without keep=true
→ CloudTrail records StopInstances
```

The EC2 stopped state is already demonstrated in the main before/after evidence, so an additional EC2 screenshot after SNS publish is not required.

### Evidence — SNS publish

![MH-COST-A-20 Terminal SNS publish message ID](./images/mh-cost-a-20-terminal-sns-publish.png)

![MH-COST-A-21 Lambda invocation after SNS publish](./images/mh-cost-a-21-lambda-invocation-after-sns.png)

![MH-COST-A-22 Lambda logs after SNS publish](./images/mh-cost-a-22-lambda-logs-after-sns-publish.png)

---

## 3.9 ADR — AWS Budgets Cost Data Latency

### Context

AWS Budgets does not always trigger immediately because AWS cost and usage data can be delayed. In a short workshop account that only runs for about 48 hours, the real budget threshold may not fire during the demo window.

### Decision

We still wired the production-like path:

```text
AWS Budget $150
→ SNS topic
→ CostGuard_Stop_Compute Lambda
→ Stop EC2
```

To validate the chain within the workshop timeframe, we manually published a test message to the SNS topic. This simulated a real budget alert event.

### Result

The same Lambda function was invoked through SNS and successfully stopped unprotected EC2 instances. This proves that the cost-driven automation chain works even though the actual AWS Budget alert may not trigger immediately in the lab environment.

### Production behavior

In production, when the actual cost crosses the configured threshold, AWS Budgets would publish to the SNS topic automatically. The same Lambda would then execute and stop non-protected development compute resources to reduce unnecessary cost.

---

## 3.10 Final Summary

The MH-COST-A cost guard demonstrates that our W6 application is cost-aware and operationally controlled.

Instead of only monitoring cost, we implemented an automated action loop:

```text
Detect cost-risk compute
→ decide based on keep=true tag
→ stop unprotected EC2
→ verify through CloudTrail
```

The scheduled path prevents daily idle compute waste. The cost-driven path wires AWS Budget $150 to SNS and Lambda, proving that a budget event can trigger the same stop automation.

This implementation helps keep the workshop account under the $150 cost cap and provides a production-ready pattern for controlling development compute cost.

---

---

# Section 4 — MH-OBS — CloudWatch Observability

## 4.1 Monitoring Strategy

### Application Components Monitored

- API Gateway
- Lambda (CarSales-Search-Service)
- Database (Amazon DocumentDB)
- AI Recommendation Layer
- Vehicle Search Servicef
- Other: CloudWatch Logs & Metrics Engine

---

## 4.2 CloudWatch Dashboard

### Code Lambda

``` python
import os
import time
import json
import boto3
from urllib.parse import quote_plus
from pymongo import MongoClient
from botocore.config import Config

aws_config = Config(
    connect_timeout=3,
    read_timeout=3,
    retries={"max_attempts": 1}
)

secrets_client = boto3.client(
    "secretsmanager",
    region_name="us-west-2",
    config=aws_config
)

_cached_secret = None


def get_docdb_secret():
    global _cached_secret

    if _cached_secret:
        return _cached_secret

    secret_name = os.environ.get("SECRET_NAME", "group2/car/docdb")
    print(f"[INFO] Reading DocumentDB secret from Secrets Manager: {secret_name}")

    response = secrets_client.get_secret_value(SecretId=secret_name)
    _cached_secret = json.loads(response["SecretString"])
    return _cached_secret


def get_value(secret, *keys):
    for key in keys:
        value = str(secret.get(key, "")).strip()
        if value:
            return value
    return ""


def lambda_handler(event, context):
    start_time = time.time()

    try:
        print("[INFO] Loading DocumentDB credentials from Secrets Manager...")

        secret = get_docdb_secret()

        host = get_value(secret, "DOCDB_HOST", "host")
        user = get_value(secret, "DOCDB_USER", "username")
        password = get_value(secret, "DOCDB_PASS", "password")
        db_name = get_value(secret, "DOCDB_DB", "database") or os.environ.get("DB_NAME", "social")
        ca_file = get_value(secret, "DOCDB_CA_FILE", "ca_file") or "global-bundle.pem"

        if not host:
            raise Exception("DOCDB_HOST is missing in Secrets Manager")
        if not user:
            raise Exception("DOCDB_USER is missing in Secrets Manager")
        if not password:
            raise Exception("DOCDB_PASS is missing in Secrets Manager")

        mongo_uri = (
            f"mongodb://{quote_plus(user)}:{quote_plus(password)}@{host}:27017/{db_name}"
            f"?retryWrites=false"
        )

        print("[INFO] Attempting to connect to DocumentDB...")
        print(f"[INFO] Target database: {db_name}")

        client = MongoClient(
            mongo_uri,
            tls=True,
            tlsCAFile=ca_file,
            serverSelectionTimeoutMS=3000,
            connectTimeoutMS=3000,
            socketTimeoutMS=3000,
            directConnection=True,
            authSource="admin",
            authMechanism="SCRAM-SHA-1"
        )

        db = client[db_name]

        query_params = event.get("queryStringParameters") or {}
        keyword = query_params.get("keyword", "")

        print(f"[INFO] User started searching for car with keyword: {keyword}")
        print("[INFO] Fetching collection names from real DocumentDB...")

        collections = db.list_collection_names()

        print(f"[INFO] Connection established successfully! Collections in DB: {collections}")

        latency_ms = int((time.time() - start_time) * 1000)

        print(json.dumps({
            "metric_name": "CarSearchLatencyMs",
            "latency_value": latency_ms
        }))

        return {
            "statusCode": 200,
            "headers": {"Content-Type": "application/json"},
            "body": json.dumps({
                "status": "success",
                "database_connected": True,
                "database": db_name,
                "available_collections": collections,
                "latency_ms": latency_ms
            })
        }

    except Exception as e:
        print(f"[ERROR] Detailed failure: {str(e)}")

        return {
            "statusCode": 500,
            "headers": {"Content-Type": "application/json"},
            "body": json.dumps({
                "error": "Internal Server Error",
                "details": str(e)
            })
        }
```

![alt text](image-75.png)

### Dashboard Overview

The **CarSales-Performance-Dashboard** is designed to provide a comprehensive view of the health and operational efficiency of **the vehicle search system**. It integrates both core Infrastructure Metrics and custom Application Performance Metrics to enable rapid anomaly detection and proactive troubleshooting.

The dashboard includes the following widgets:

1. **CarSearchLatencyMs (Line Widget):** Monitors real-time latency (in milliseconds) when the Lambda function establishes connections and fetches collection data from the Amazon DocumentDB cluster.
2. **Lambda Duration (Line Widget):** Tracks the average and maximum execution time of the Lambda function to detect potential VPC networking bottlenecks or driver resolution delays.
3. **Lambda Errors (Line Widget):** Monitors the count of execution failures (crashes or timeouts) to assess overall system stability.

---

### Custom Metric Widget

| Metric Name          | Namespace                | Unit         |
| -------------------- | ------------------------ | ------------ |
| `CarSearchLatencyMs` | `CarSalesApp/Operations` | Milliseconds |

---

### Standard Metrics

- Lambda Errors (For `CarSales-Search-Service` function)
- Lambda Duration (`CarSales-Search-Service` Execution Time)
- Amazon DocumentDB Connections (Monitoring open connections within the Private Subnets)

---

### Dashboard Screenshot

![alt text](image-18.png)
![alt text](image-19.png)

---

## 4.3 Custom Metric Implementation

### Metric Description

The tracked metric is **CarSearchLatencyMs**. It measures the total end-to-end duration (in milliseconds) from the moment the Lambda function receives a search request, establishes a secure TLS handshake across the internal VPC network to the Primary Instance of Amazon DocumentDB, and successfully returns the collection names.

Since the Lambda function resides inside a Private Subnet without public Internet access, calling the native `put_metric_data` API would block execution and cause execution timeouts. To solve this, a highly optimized **Log-Based Custom Metric architecture** was implemented. The Lambda function logs structured data in JSON format to CloudWatch Logs, and a **Metric Filter** automatically parses the `$.latency_value` property to publish a dynamic custom metric.

---

### PutMetricData Code Snippet

```python
# Structured JSON logging snippet used for Metric Filter value extraction
import json
import time

start_time = time.time()

# ... Connection logic and db.list_collection_names() from DocumentDB ...

latency_ms = int((time.time() - start_time) * 1000)

# Print structured JSON to stdout for CloudWatch Logs Engine to capture $.latency_value
print(json.dumps({
    "metric_name": "CarSearchLatencyMs",
    "latency_value": latency_ms
}))
```

---

## 4.4 CloudWatch Alarm

### Alarm Configuration

| Setting           | Value                                       |
| ----------------- | ------------------------------------------- |
| Alarm Name        | Lambda-Errors-Alarm                         |
| Metric            | Errors (Standard Lambda Metric)             |
| Threshold         | Greater/equal (>=) 5 errors within 5 minute |
| Evaluation Period | 5 minute (Statistic: Sum)                   |
| Alarm Action      | None / SNS Notification Topic               |

---

### Alarm State

- OK
- ALARM

---

### Alarm Screenshot

![alt text](image-14.png)
![alt text](image-15.png)
![alt text](image-31.png)

---

## 4.5 CloudWatch Logs Insights

### Log Group

/aws/lambda/CarSales-Search-Service

---

### Saved Query Name

thao

---

### Query Purpose

This advanced analytical query scans system execution logs to filter and parse the latency_value attribute from the structured JSON stream. The extracted data points are aggregated into 5-minute time buckets (bin(5m)) to compute: Total Search Volume (TotalSearches), Average Latency (AvgLatencyMs), and Peak Latency (MaxLatencyMs). This allows engineers to easily isolate database connection degradation or spike periods during high traffic.

---

### Logs Insights Query

```sql
fields @timestamp, @message
| filter @message like /CarSearchLatencyMs/
| parse @message /"latency_value":\s*(?<latency_value>\d+)/
| stats count(*) as TotalSearches, avg(latency_value) as AvgLatencyMs, max(latency_value) as MaxLatencyMs by bin(5m)
| sort @timestamp desc
```

---

### Query Results Screenshot

![alt text](image-16.png)
![alt text](image-74.png)

---

# Section 5 — MH-SEC — Self-Healing Security Guard

## 5.1 Security Guard Overview

### Security Threat Addressed

The security misconfiguration targeted by this automation is the catastrophic public exposure of administrative management ports to the open internet (`0.0.0.0/0`). Specifically, the guard actively monitors and intercepts insecure Security Group ingress rules allowing unconfined inbound traffic over:
* **SSH (TCP Port 22):** Remote command-line administration for Linux instances.
* **RDP (TCP Port 3389):** Remote Desktop Protocol for Windows instances.

Leaving these management ports open to the world represents an extreme security risk, inviting automated malicious scanners to execute brute-force attacks, credential-stuffing campaigns, or exploit zero-day remote execution vulnerabilities within core OS daemons.

---

### Potential Blast Radius

In a production tier, an unremediated public management port drastically widens the infrastructure's attack surface. The potential blast radius includes:
* Unauthorized root-level interactive access to backend application compute instances (`car-backend-huutai-a`).
* Potential side-channel lateral movement across the internal AWS VPC network to compromise private stateful clusters (e.g., Amazon DocumentDB).
* Ransomware injection, unauthorized data exfiltration, malicious modification of system files, and full operational service disruption.

---

## 5.2 Automated Remediation Lambda

### Lambda Purpose

The **`SecurityGuard_SelfHealing`** Lambda function acts as an automated, continuous compliance officer. It is designed to inspect active Security Group configurations, audit inbound rules against defined compliance bounds, and programmatically strip away any discovered high-risk external entry points without waiting for human operator intervention.

---

### Detection Method

- [x] Security Group Detection
- [ ] S3 Public Access Detection
- [ ] Other:

---

### Remediation Action

Upon identifying a non-compliant ingress rule containing a source CIDR of `0.0.0.0/0` mapped to port 22 or 3389, the Lambda function acts as an active interceptor. It isolates the violating parameters and issues an immediate, targeted programmatic command via the **`RevokeSecurityGroupIngress`** API call. This instantly deletes the dangerous rule from the active security firewall layer, neutralizing the threat vector in near real-time.

---

### IAM Least-Privilege Policy

To enforce proper separation of duties and contain blast radius risks, the Lambda's execution role completely avoids the generic `AdministratorAccess` policy. It utilizes a strict, custom least-privilege IAM policy limited to:
* **`ec2:DescribeSecurityGroups`**: Permission to read current inbound firewall profiles.
* **`ec2:RevokeSecurityGroupIngress`**: Permission to surgically strip out non-compliant ingress rules.
* **`logs:CreateLogGroup` / `logs:CreateLogStream` / `logs:PutLogEvents`**: Core logging capabilities for auditing and historical trail preservation.

![alt text](image-70.png)
![alt text](image-71.png)

The Lambda does not use AdministratorAccess. This reduces blast radius because the function can only describe Security Groups, revoke insecure ingress rules, and write logs.

---

### Lambda Screenshot

![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-67.png)
![alt text](image-68.png)
![alt text](image-69.png)
![alt text](image-47.png)

---

## 5.3 Trigger Mechanism

### Trigger Type

- [ ] EventBridge Rule
- [x] EventBridge Scheduler

---

### Event Source

The execution is wired to an **Amazon EventBridge Scheduler** cron policy. For the demonstration phase, the scheduler is throttled down to execute every few minutes to prove active remediation during grading. For true production environments, it is configured as a daily recurring background audit to maintain a clean baseline state.

---

### Schedule Setting

![alt text](image-48.png)
![alt text](image-72.png)
![alt text](image-49.png)


---

## 5.4 Demonstrated Detect → Fix Loop

### Security Violation Scenario

To demonstrate the robust automated loop, a manual violation was forced upon the environment. An insecure administrative rule was deliberately appended to the target asset:
* **Security Group ID:** `sg-0c1085a97f4479ac7`
* **Security Group Name:** `selfhealing-test`
* **Injected Violation:** Inbound Protocol `TCP`, Port `22` (SSH), Source CIDR `0.0.0.0/0` (Anywhere) with Rule ID `sgr-055bce34319787083`.

---

### Before State (Insecure)

![alt text](image-50.png)
---

### Automated Remediation

Once triggered, the Lambda function captured the state configuration, isolated the rule violation on `sg-0c1085a97f4479ac7`, and printed the following audit confirmation JSON log to CloudWatch while calling the API:

```json
{
  "Status": "Revoking dangerous ingress from SG sg-0c1085a97f4479ac7 (selfhealing-test)",
  "TargetRule": {
    "IpProtocol": "tcp",
    "FromPort": 22,
    "ToPort": 22,
    "IpRanges": [
      {
        "CidrIp": "0.0.0.0/0"
      }
    ]
  }
}
```

---

### After State (Remediated)

![alt text](image-51.png)

---

### CloudTrail Evidence

![alt text](image-52.png)

---

# Section 5.5 — Supporting Preventive Control

## Selected Path

- [x] Path A — KMS Customer Managed Key
- [x] Path B — S3 Block Public Access + Deny Policy
- [ ] Path C — IAM Access Analyzer

---

## Path A — KMS Customer Managed Key

### CMK Configuration

| Setting | Value |
| ---------------- | ----- |
| **Key Alias** | `alias/carsales-s3-prod-key` |
| **Rotation Enabled** | `True (Enabled)` |
| **Applied Service** | `Amazon S3 (Bucket: s3-kms-131234)` |

---

### Encryption Usage Evidence
![alt text](image-58.png)
![alt text](image-59.png)
![alt text](image-60.png)
![alt text](image-61.png)
![alt text](image-62.png)
![alt text](image-63.png)
![alt text](image-64.png)
![alt text](image-65.png)

## Path B — S3 Block Public Access + Deny Policy

### Account-Level Block Public Access

To proactively neutralize the risk of accidental bucket exposure via misconfigured Access Control Lists (ACLs) or loose bucket-level updates, **S3 Block Public Access** has been fully enabled at the root AWS Account level. All four core structural configuration options are turned on:
1. Block public ACLs for new buckets and objects.
2. Remove public ACLs for existing buckets and objects.
3. Block public bucket policies for new buckets.
4. Block public and cross-account access for buckets with public policies.

![alt text](image-73.png)

---

### Bucket Policy

```json
{
  "Id": "PolicyForCloudFrontPrivateContent",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontServicePrincipal",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::group2-car-frontend-dev/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::462972379716:distribution/EBLC5THV5AEAZ"
        }
      }
    },
    {
      "Sid": "DenyNonTLSRequests",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::group2-car-frontend-dev",
        "arn:aws:s3:::group2-car-frontend-dev/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
```

---

### Denied Request Evidence

HTTPS upload → Success
HTTP upload using --endpoint-url http://s3.us-west-2.amazonaws.com → AccessDenied
The bucket policy contains two controls:


![alt text](image-53.png)
![alt text](image-54.png)
![alt text](image-55.png)

---


## 5.6 Security-Cost Trade-Off

### Cost Impact

The combined financial overhead of the selected security measures is exactly USD 0.00 (Free):

Account-Level S3 Block Public Access: Offered as a native, built-in governance platform feature by AWS with zero supplemental subscription or resource management fees.

IAM Bucket Policy Modification: Evaluating structural conditions (aws:SecureTransport) occurs natively inside the AWS IAM policy validation layer and does not generate transactional billing charges.

Implementing a dedicated Customer Managed Key (CMK) introduces a predictable, flat structural fee of **USD 1.00 per month** for storing the baseline cryptographic key material inside the regional AWS KMS repository. 

Additionally, nominal transactional variable costs are accumulated based on the volume of KMS API requests (such as `GenerateDataKey` and `Decrypt`) processed during file upload and download operations.

---

### Business Justification

The zero-cost profile yields an exceptionally high security return on investment (ROI). Data leaks stemming from exposed S3 static hosting origins represent one of the most common vectors for corporate data exposure. Enforcing centralized account blocks and mandatory TLS encryption completely neutralizes accidental public access vectors and wiretapping risks.

The minimal operational friction—ensuring that deployment pipelines communicate explicitly over TLS-enabled endpoints—is a standard best practice that guarantees the integrity of our front-end distribution.

While default AWS-managed keys (`SSE-S3`) are provided completely free of charge, they operate as shared infrastructure and lack the granular access boundaries required for an enterprise-grade car sales system. 

The minimal fee associated with the custom CMK is thoroughly justified by the following business advantages:
1. **Granular Audit Mapping:** Provides distinct, resource-isolated execution audit trails inside AWS CloudTrail, making it simple to track exactly who accessed which data blocks.
2. **Instant Blast Radius Containment:** In the event of a credential exposure or security anomaly, security administrators can execute an immediate programmatic shutdown (disabling the key). This instantaneously neutralizes the exposed data store at rest, rendering the files unreadable to attackers.
3. **Compliance Readiness:** Supports automated annual cryptographic rotation natively, checking off mandatory security compliance boxes for cloud-native applications. 

Furthermore, the strategic adoption of the **S3 Bucket Key** sub-feature safely suppresses unnecessary API transactional costs, achieving a highly optimized, production-ready balance between data protection and budget guardrails.

---

# Section 6 — Deployment Demonstration Summary

## MH-COST-V Verification

The implementation of Group G2's cost allocation and tracking architecture has been successfully verified through several automated cloud visibility tools:
* **Resource Tagging Compliance:** Full structural compliance was demonstrated across all six core billable infrastructure components (EC2, DocumentDB, Lambda, S3, API Gateway, and EFS File Systems). Verification screenshots (Figures 2.1 to 2.3) confirm the successful injection of the four exact mandatory keys: `Owner` (`huutai.ngo2409@gmail.com`), `Environment` (`dev`), `CostCenter` (`G2`), and `Application` (`AAP-CarApp`).
* **Financial Guardrails and Anomaly Alerts:** AWS Cost Explorer reports were successfully initialized to monitor baseline metrics. Financial boundaries were actively established using an AWS Budget configured with a strict threshold cap of **USD 150.00**. Additionally, an AWS Cost Anomaly Detection monitor (`Group-2-monitor`) was deployed and linked to an alert subscription with a **USD 10.00** threshold to immediately flag abnormal cost drivers, such as the fixed operational overhead of the `Network Firewall` ($10.67).

---

## MH-COST-A Verification

The operational capability of the active cost containment loop was verified through both scheduled and event-driven testing mechanisms:
* **Selective Cost Safeguard Loop:** Manual invocation and CloudWatch log validation of the `CostGuard_Stop_Compute` function verified the accuracy of the underlying Python (`boto3`) script. The automation scanned the compute topology and applied selective filtering: it successfully bypassed the protected bastion host (`i-0b11cacc05b1a432e`) containing the `keep=true` metadata tag while forcefully executing a `StopInstances` API command on the untagged nodes `i-01669f35cc53b5899` (`car-backend-huutai-a`) and `i-04caa3aea23b844ed`.
* **Budget Trigger Chain Validation:** To circumvent sandbox processing delays, a real budget alert event was simulated by publishing an explicit JSON test string to the standard SNS topic (`group2-cost-guard-budget-topic`) via the AWS CLI. Execution logs verified that the Lambda function was triggered successfully via the SNS endpoint principal (`sns.amazonaws.com`), confirming that the cost-driven remediation loop is wired correctly and fully operational.

---

## MH-OBS Verification

The telemetry and application monitoring engine for the vehicle search layer has been successfully verified:
* **Log-Based Telemetry Pipeline:** The implementation proved that the isolated private subnet restrictions were overcome by deploying a decoupled **Log-Based Custom Metric architecture**. The backend search Lambda successfully emitted rapid JSON metric data directly to `stdout`, allowing a CloudWatch Metric Filter to capture and parse the `$.latency_value` without introducing execution timeouts or requiring a public NAT route.
* **Dashboard and Alarm Visualization:** The aggregated custom performance metrics were successfully mapped onto the centralized **CarSales-Performance-Dashboard**, providing real-time line widget visibility into database query latency (`CarSearchLatencyMs`). System resilience was confirmed by the active deployment of a `Lambda-Errors-Alarm` configured to watch standard execution failure counts and transition cleanly into an active state when threshold parameters are crossed.

---

## MH-SEC Verification

The automated reactive and preventative security posture for the workload has been completely verified:
* **Self-Healing Firewall Remediation:** Active reactive defense was proven by injecting an intentional security violation—introducing an unconfined inbound SSH rule (Port 22 open to `0.0.0.0/0`) on the target testing security group (`sg-0c1085a97f4479ac7`). Upon execution via the EventBridge Scheduler, the `SecurityGuard_SelfHealing` function successfully captured the event, parsed the breach, and programmatically triggered the `RevokeSecurityGroupIngress` API. This completely removed the dangerous rule and restored the firewall to a compliant state, verified by immutable AWS CloudTrail audit logs.
* **Preventative Data Edge Controls:** Implementation of Path B verified robust perimeter data security. All four account-level S3 Block Public Access variables were toggled on. Furthermore, the frontend bucket (`group2-car-frontend-dev`) was locked down via an explicit bucket policy restricting read paths to the CloudFront distribution origin (`EBLC5THV5AEAZ`). The policy's automated `DenyNonTLSRequests` layer was successfully verified using the AWS CLI, throwing a definitive `AccessDenied` response when unencrypted HTTP uploads were attempted over port 80 while allowing secure HTTPS commands to proceed normally.

---

# Section 7 — Lessons Learned

## Operational Challenges

During the design, redeployment, and validation phases of the `AAP-CarApp` architecture, Group G2 encountered several critical operational roadblocks that impacted system runtime, metrics generation, and compliance bounds:

1. **VPC Isolation and Database Connection Deadlocks:** Placing the backend `CarSales-Search-Service` Lambda function and the Amazon DocumentDB cluster within fully isolated Private Subnets introduced critical execution issues. By default, the PyMongo driver attempted to perform an automated replica set topology discovery loop. Because the AWS Lab environment blocks arbitrary cross-subnet multi-AZ topology scans, the driver hit an unresolved routing loop, leading to consistent 30-second execution timeouts and sifting through broken connections.

2. **Metrics Pipeline Blocking & Subnet Architecture Restrictions:** The initial monitoring strategy relied on calling the standard AWS SDK API `cloudwatch.put_metric_data` synchronously from within the Lambda function code to track operational latency. However, since the private subnets lacked an outbound NAT Gateway or a dedicated VPC Endpoint for CloudWatch, the data packets became trapped inside the private network boundary. This network blockage caused the Lambda functions to hang indefinitely, introducing unnatural transaction latency.

3. **Human Configuration Error and Rapid Port Exposure:** During intensive live test intervals, human error led to the accidental addition of insecure Security Group ingress rules, opening up management ports (such as SSH Port 22 and RDP Port 3389) directly to the public internet (`0.0.0.0/0`). This immediately violated enterprise infrastructure compliance rules and highlighted how quickly vulnerable attack paths can appear on the public network edge when relying on manual human administration.

---

## Improvements Made

To resolve these challenges, Group G2 refactored the infrastructure and code logic to incorporate non-blocking, event-driven, and highly resilient design configurations:

1. **Driver Optimization & Timeout Hardening:** The backend database connection script was optimized by injecting the `directConnection=True` configuration parameter directly into the PyMongo `MongoClient` client block. This bypassed the restrictive topology sweep and forced an instant, linear connection path to the DocumentDB primary instance endpoint. Concurrently, network socket wait thresholds (`connectTimeoutMS`, `socketTimeoutMS`, `serverSelectionTimeoutMS`) were lowered to a strict 2000ms, ensuring that any connection faults drop instantly rather than driving up unneeded Lambda billing runtimes.

2. **Decoupled Log-Based Custom Metric Architecture:** To circumvent the lack of public internet routes within the isolated private subnets, the team eliminated all direct synchronous API calls to CloudWatch from the application tier. Instead, a decoupled **Log-Based Custom Metric** pipeline was established. The Lambda script was updated to simply print lightweight, structured JSON strings (e.g., matching the `CarSearchLatencyMs` metric name) directly to standard output (`stdout`), which executes in less than 1ms. A CloudWatch **Metric Filter** was then configured at the Log Group layer to asynchronously scan the incoming streams, extract the variable values, and dynamically generate the dashboard graphics with zero performance overhead.

3. **Automated Budget Guardrails and Self-Healing Security Filters:** To safeguard the workspace from budget breaches and external entry points, automated remediation mechanisms were successfully deployed. Group G2 implemented an active **`SecurityGuard_SelfHealing`** Lambda loop triggered via an **Amazon EventBridge Scheduler**. The moment an unencrypted administrative route appeared on `sg-0c1085a97f4479ac7`, the Lambda function programmatically called `RevokeSecurityGroupIngress` to tear down the rule. Furthermore, financial guardrails were set using an AWS Cost Budget capped tightly at **USD 150.00**, wired via an SNS topic subscription to handle budget overruns automatically.

---

## Production Recommendations

If this car sales infrastructure is transitioned into an enterprise-grade, high-availability production cloud environment, Group G2 recommends deploying the following permanent enhancements:

1. **Establish Dedicated AWS PrivateLink VPC Endpoints:** Instead of utilizing log-parsing workarounds or provisioning expensive NAT Gateways to bypass private subnet boundaries, production environments should deploy native **VPC Interface Endpoints** (AWS PrivateLink) for CloudWatch (`com.amazonaws.us-west-2.logs`), Secrets Manager, and systems management. This ensures that all management and telemetry data packets flow safely across the isolated internal AWS private network backbone, improving transit latency and minimizing data exposure.

2. **Enforce Absolute GitOps Pipelines with Static Security Linters:** All manual mutations through the AWS Management Console must be strictly blocked using IAM permissions. The entire workload configuration must be managed through Declarative **Infrastructure-as-Code (IaC)** templates (such as our verified CloudFormation stack file `infra/w6-self-healing-security-guard.yaml`). Deployment pipelines must run automated static check rules (e.g., `Checkov`, `TFLint`, or `cfn-lint`) to inspect policies and reject any commits that introduce exposed ports or lack mandatory cost allocation tags (`Owner`, `Environment`, `CostCenter`, `Application`).

3. **Implement Database Right-Sizing and Scheduled Compute Suspension:** To manage stateful persistence costs effectively, production DocumentDB configurations should leverage modern clustering rules, such as deploying auto-scaling instance classes or utilizing automated nightly hooks. For development environments, EventBridge automated cron tasks should completely pause DocumentDB instances and auxiliary non-production EC2 servers outside of working hours, ensuring strict compliance with operational cost caps without impacting developer agility.

---

# Section 8 — Stretch Goals (Optional)

## 8.1 CloudFormation template for one W6 resource

### Infrastructure-as-Code (IaC) Justification
To demonstrate structural maturity, infrastructure-as-code discipline, and prepare the environment for Week 7 deployment pipelines, the entire Week 6 Self-Healing Security Guard infrastructure has been fully codified into an AWS CloudFormation template. This ensures that the detection and automated remediation loop can be reliably destroyed, version-controlled, and redeployed across multiple target AWS accounts with zero configuration drift.

* **Template File Path:** `infra/w6-self-healing-security-guard.yaml`
* **Validation Status:** `Successfully validated via AWS CloudFormation CLI linter engine`

### Resources Defined in the Template

The template declares a modular topology consisting of the following core AWS resource blocks:

1. **`AWS::Lambda::Function` (Security Guard Lambda Engine):**
   The core execution runtime running the Python (boto3) code. It acts as the intelligent automation layer that scans inbound security boundaries and intercepts non-compliant changes.
2. **`AWS::IAM::Role` (Least-Privilege Execution Boundary):**
   The dedicated identity role that grants the Lambda function temporary security credentials. It strictly confines rights to `ec2:DescribeSecurityGroups`, `ec2:RevokeSecurityGroupIngress`, and CloudWatch logging paths, enforcing a minimal blast radius.
3. **`AWS::Events::Rule` (EventBridge Event Driven Router):**
   An EventBridge rule configured to capture API mutations. It actively listens to AWS CloudTrail for management events—specifically intercepting the **`AuthorizeSecurityGroupIngress`** API call—enabling the system to trigger the remediation loop instantly whenever a rule is created.
4. **`AWS::Lambda::Permission` (Invocation Trust Policy):**
   An explicit resource-based access policy that grants the Amazon EventBridge service principal authorization to trigger and invoke the Security Guard Lambda function.
5. **`AWS::Logs::LogGroup` (Compliant Log Engine):**
   An explicitly provisioned CloudWatch Log Group dedicated to capturing the Lambda’s standard output. It is configured with an active retention policy to minimize log storage costs while preserving the security audit trail.

### Compliance Filtering Logic Codified

The template injects environment variables and rules that instruct the automated remediation logic to evaluate ingress records against the following explicit security thresholds:

* **Linux Attack Vector:** Protocol `TCP`, Port `22` (SSH) sourced from `0.0.0.0/0` (IPv4 Anywhere) or `::/0` (IPv6 Anywhere).
* **Windows Attack Vector:** Protocol `TCP`, Port `3389` (RDP) sourced from `0.0.0.0/0` (IPv4 Anywhere) or `::/0` (IPv6 Anywhere).

If an administrative firewall opening matches either criteria, the function bypasses manual notifications and immediately invokes the **`RevokeSecurityGroupIngress`** API to forcefully tear down the dangerous entry path, restoring the security group to a secure state.

### Screenshot

![alt text](image-56.png)
![alt text](image-57.png)
 
Repo Link: https://github.com/tuu-ngo/car-showroom-aws/blob/main/infra/w6-self-healing-security-guard.yaml 