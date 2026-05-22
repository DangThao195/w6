# MH-COST-A — Cost Control & Action

## 1. Objective

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

## 2. Automated Cost Guard Lambda

### 2.1 Lambda function

| Item | Value |
|---|---|
| Lambda name | `CostGuard_Stop_Compute` |
| Runtime | Python |
| AWS SDK | `boto3` |
| Target resource | EC2 instances |
| Action | `StopInstances` |
| Protection rule | Skip EC2 instances with tag `keep=true` |
| Environment | `dev` |
| Application | `AAP-CarApp` |
| CostCenter | `G2` |

The Lambda function scans all EC2 instances in the `running` state. For each instance, it checks whether the instance has the tag:

```text
keep=true
```

If the tag is missing, the Lambda treats the instance as a cost-risk resource and stops it using the EC2 `StopInstances` API.

If the tag exists, the Lambda logs the instance as protected and skips it. This prevents important infrastructure such as the bastion host from being stopped accidentally.

### 2.2 Lambda code summary

```python
import boto3
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')

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

TODO: add image

![MH-COST-A-01 Lambda code for CostGuard_Stop_Compute](./images/mh-cost-a-01-lambda-code.png)

TODO: add image

![MH-COST-A-02 Lambda manual test succeeded](./images/mh-cost-a-02-lambda-test-success.png)

TODO: add image

![MH-COST-A-03 Lambda CloudWatch logs showing scanned and stopped EC2](./images/mh-cost-a-03-lambda-logs-stop-ec2.png)

---

## 3. IAM Least-Privilege Role

The Lambda function uses a dedicated IAM role with only the permissions required for this cost guard.

Required permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DescribeEC2Instances",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    },
    {
      "Sid": "StopEC2Instances",
      "Effect": "Allow",
      "Action": [
        "ec2:StopInstances"
      ],
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

TODO: add image

![MH-COST-A-04 Lambda IAM role least privilege policy](./images/mh-cost-a-04-iam-least-privilege-policy.png)

TODO: add image

![MH-COST-A-05 Lambda execution role attached to CostGuard_Stop_Compute](./images/mh-cost-a-05-lambda-execution-role.png)

---

## 4. Component (b) — Daily EventBridge Schedule

To make the cost guard automatic, we configured an EventBridge Scheduler rule to invoke the Lambda every day.

| Item | Value |
|---|---|
| Scheduler name | `cost-guard-daily-schedule` |
| Target | `CostGuard_Stop_Compute` |
| Schedule type | Recurring |
| Schedule expression | Daily cron |
| Purpose | Automatically stop unprotected dev compute resources |
| State | Enabled |

Example production behavior:

```text
Every day at the scheduled time
→ EventBridge invokes CostGuard_Stop_Compute
→ Lambda scans EC2 running instances
→ Lambda stops EC2 instances without keep=true
```

This prevents development EC2 instances from running idle overnight and helps keep the weekly account cost under the $150 cap.

### Evidence

TODO: add image

![MH-COST-A-06 EventBridge daily schedule overview](./images/mh-cost-a-06-eventbridge-daily-schedule.png)

TODO: add image

![MH-COST-A-07 EventBridge schedule target Lambda CostGuard_Stop_Compute](./images/mh-cost-a-07-eventbridge-target-lambda.png)

---

## 5. Component (c) — Demonstrated Action

### 5.1 Before state

Before invoking the automation, at least one EC2 instance was running and did not have the protection tag:

```text
keep=true
```

Target examples:

| Instance name | Instance ID | Initial state | `keep=true` |
|---|---|---|---|
| `car-backend-huutai-a` | `i-01669f35cc53b5899` | Running | Missing |
| Additional unprotected EC2 | `i-04caa3aea23b844ed` | Running | Missing |

Protected instance example:

| Instance name | Instance ID | State | Reason |
|---|---|---|---|
| `bastion-backend-huutai-a` | `i-0b11cacc05b1a432e` | Running | Protected by `keep=true` |

### Evidence — Before

TODO: add image

![MH-COST-A-08 EC2 before automation running state](./images/mh-cost-a-08-ec2-before-running.png)

TODO: add image

![MH-COST-A-09 EC2 tag view showing missing keep tag or protected keep=true tag](./images/mh-cost-a-09-ec2-tags-keep-true.png)

### 5.2 Lambda execution

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

### 5.3 After state

After the Lambda executed, the unprotected EC2 instances moved from `running` to `stopped`.

| Instance name | Instance ID | Final state |
|---|---|---|
| `car-backend-huutai-a` | `i-01669f35cc53b5899` | Stopped |
| Additional unprotected EC2 | `i-04caa3aea23b844ed` | Stopped |
| `bastion-backend-huutai-a` | `i-0b11cacc05b1a432e` | Running |

The protected bastion instance remained running, proving that the `keep=true` safeguard worked correctly.

### Evidence — After

TODO: add image

![MH-COST-A-11 EC2 after automation stopped state](./images/mh-cost-a-11-ec2-after-stopped.png)

---

## 6. CloudTrail Evidence — StopInstances

CloudTrail recorded the EC2 `StopInstances` API call.

This proves that the EC2 stop action was performed by AWS automation through the Lambda execution role, not manually by a human user.

Important fields to capture:

| Field | Expected value |
|---|---|
| Event name | `StopInstances` |
| Event source | `ec2.amazonaws.com` |
| User identity type | `AssumedRole` |
| Role / session | Lambda execution role for `CostGuard_Stop_Compute` |
| Resource | EC2 instance ID that was stopped |
| AWS Region | `us-west-2` |

Example CloudTrail lookup command:

```bash
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=StopInstances \
  --region us-west-2 \
  --max-results 5
```

### Evidence

TODO: add image

![MH-COST-A-12 CloudTrail Event History filtered by StopInstances](./images/mh-cost-a-12-cloudtrail-stopinstances-list.png)

TODO: add image

![MH-COST-A-13 CloudTrail StopInstances event detail](./images/mh-cost-a-13-cloudtrail-stopinstances-detail.png)

TODO: add image

![MH-COST-A-14 CloudTrail event record JSON showing Lambda assumed role](./images/mh-cost-a-14-cloudtrail-event-json-assumed-role.png)

---

## 7. Component (d) — Cost-Driven Path: AWS Budget → SNS → Lambda

### 7.1 AWS Budget

We configured an AWS Budget with a hard workshop cost cap of:

```text
$150
```

The budget is connected to SNS so that cost-related events can trigger the same Lambda cost guard.

| Item | Value |
|---|---|
| Budget name | `group2-budget` or `W6-150-Cost-Cap` |
| Budget amount | `$150` |
| Budget type | Cost budget |
| Notification type | Actual cost |
| Notification target | SNS topic |
| SNS topic | `group2-cost-guard-budget-topic` |

### Evidence — Budget

TODO: add image

![MH-COST-A-15 AWS Budget overview 150 USD](./images/mh-cost-a-15-budget-overview-150.png)

TODO: add image

![MH-COST-A-16 AWS Budget alert notification to SNS topic](./images/mh-cost-a-16-budget-sns-notification.png)

---

### 7.2 SNS topic and Lambda subscription

The SNS topic is used as the event bridge between AWS Budgets and the Lambda cost guard.

```text
AWS Budget
→ SNS topic
→ CostGuard_Stop_Compute Lambda
```

| Item | Value |
|---|---|
| SNS topic name | `group2-cost-guard-budget-topic` |
| Topic type | Standard |
| Region | `us-west-2` |
| Purpose | Trigger cost guard when budget alert is published |
| Subscription protocol | AWS Lambda |
| Subscription endpoint | `CostGuard_Stop_Compute` |
| Subscription status | Confirmed |

The screenshot for this section includes both the SNS topic overview and the Lambda subscription, so a separate SNS subscription screenshot is not required.

### Evidence — SNS topic and Lambda subscription

TODO: add image

![MH-COST-A-17 SNS topic overview and Lambda subscription confirmed](./images/mh-cost-a-17-sns-topic-overview.png)

---

### 7.3 Lambda permission for SNS invoke

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

TODO: add image

![MH-COST-A-19 Lambda resource policy allows SNS invoke](./images/mh-cost-a-19-lambda-sns-invoke-permission.png)

---

## 8. Manual SNS Publish Demonstration

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

TODO: add image

![MH-COST-A-20 Terminal SNS publish message ID](./images/mh-cost-a-20-terminal-sns-publish.png)

TODO: add image

![MH-COST-A-21 Lambda invocation after SNS publish](./images/mh-cost-a-21-lambda-invocation-after-sns.png)

TODO: add image

![MH-COST-A-22 Lambda logs after SNS publish](./images/mh-cost-a-22-lambda-logs-after-sns-publish.png)

---

## 9. ADR — AWS Budgets Cost Data Latency

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

## 10. Final Summary

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

## 11. Evidence Image Checklist

| Image file | Purpose | Required |
|---|---|---|
| `mh-cost-a-01-lambda-code.png` | Lambda code showing cost guard logic | Yes |
| `mh-cost-a-02-lambda-test-success.png` | Lambda manual test succeeded | Yes |
| `mh-cost-a-03-lambda-logs-stop-ec2.png` | Logs showing scan, stop, and skip behavior | Yes |
| `mh-cost-a-04-iam-least-privilege-policy.png` | IAM policy with least-privilege permissions | Yes |
| `mh-cost-a-05-lambda-execution-role.png` | Lambda execution role attached | Yes |
| `mh-cost-a-06-eventbridge-daily-schedule.png` | EventBridge daily schedule overview | Yes |
| `mh-cost-a-07-eventbridge-target-lambda.png` | EventBridge target is CostGuard Lambda | Yes |
| `mh-cost-a-08-ec2-before-running.png` | EC2 before automation is running | Yes |
| `mh-cost-a-09-ec2-tags-keep-true.png` | EC2 tag evidence for keep=true rule | Yes |
| `mh-cost-a-11-ec2-after-stopped.png` | EC2 after automation is stopped | Yes |
| `mh-cost-a-12-cloudtrail-stopinstances-list.png` | CloudTrail list filtered by StopInstances | Yes |
| `mh-cost-a-13-cloudtrail-stopinstances-detail.png` | StopInstances event detail | Yes |
| `mh-cost-a-14-cloudtrail-event-json-assumed-role.png` | CloudTrail JSON showing Lambda assumed role | Yes |
| `mh-cost-a-15-budget-overview-150.png` | AWS Budget overview with $150 cap | Yes |
| `mh-cost-a-16-budget-sns-notification.png` | Budget notification to SNS topic | Yes |
| `mh-cost-a-17-sns-topic-overview.png` | SNS topic overview and Lambda subscription | Yes |
| `mh-cost-a-19-lambda-sns-invoke-permission.png` | Lambda resource policy allows SNS invoke | Yes |
| `mh-cost-a-20-terminal-sns-publish.png` | Manual SNS publish output | Yes |
| `mh-cost-a-21-lambda-invocation-after-sns.png` | Lambda invocation after SNS publish | Yes |
| `mh-cost-a-22-lambda-logs-after-sns-publish.png` | Lambda logs after SNS publish | Yes |
