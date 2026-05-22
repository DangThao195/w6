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
    * **Filter applied:** Tag: `Application = AAP-CarApp`.

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

## 2.5 Baseline Cost Breakdown

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

### Dashboard Overview

The **CarSales-Performance-Dashboard** is designed to provide a comprehensive view of the health and operational efficiency of the vehicle search system. It integrates both core Infrastructure Metrics and custom Application Performance Metrics to enable rapid anomaly detection and proactive troubleshooting.

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

CarSales-Latency-Deep-Analysis

---

### Query Purpose

This advanced analytical query scans system execution logs to filter and parse the latency_value attribute from the structured JSON stream. The extracted data points are aggregated into 5-minute time buckets (bin(5m)) to compute: Total Search Volume (TotalSearches), Average Latency (AvgLatencyMs), and Peak Latency (MaxLatencyMs). This allows engineers to easily isolate database connection degradation or spike periods during high traffic.

---

### Logs Insights Query

```sql
fields @timestamp, @message
| filter @message like /START|END|REPORT|ERROR|Exception|Task timed out|Runtime/
| stats count(*) as log_count by bin(5m)
| sort @timestamp desc
```

---

### Query Results Screenshot

![alt text](image-16.png)
![alt text](image-17.png)

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

| Setting          | Value |
| ---------------- | ----- |
| Key Alias        |       |
| Rotation Enabled |       |
| Applied Service  |       |

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

| Item         | Details |
| ------------ | ------- |
| Finding      |         |
| Action Taken |         |
| Result       |         |

---

### Finding 2

| Item         | Details |
| ------------ | ------- |
| Finding      |         |
| Action Taken |         |
| Result       |         |

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
