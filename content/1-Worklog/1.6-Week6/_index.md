---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 6 Objectives:

* Build a foundation in **resource governance** with AWS Tags and Resource Groups for organizing, querying, and automating actions across many resources.
* Strengthen **IAM least privilege** with tag-based access control for EC2 and Permission Boundaries that cap the maximum permissions of users.
* Practice **encryption at rest** for Amazon S3 using AWS KMS, combined with AWS CloudTrail and Amazon Athena to audit key usage.
* Get started with **AWS Security Hub** to centralize security findings and run continuous compliance checks against AWS best practices.
* Optimize **operations and cost** with Lambda-based EC2 start/stop automation.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Practice **Tags & Resource Groups**: <br>&emsp; + Apply tags to EC2/S3 and other resources <br>&emsp; + Create a tag-based Resource Group <br>&emsp; + Use the group to manage and automate actions <br>&emsp; + Clean up resources | 25/05/2026 | 25/05/2026 | [Tags & Resource Groups](https://000027.awsstudygroup.com/) |
| 3 | - Practice **IAM tag-based access control for EC2**: <br>&emsp; + Create IAM policy with `Condition` on resource tags <br>&emsp; + Create IAM role for an EC2 Administrator persona <br>&emsp; + Validate least-privilege behavior <br> - Practice **IAM Permission Boundary**: <br>&emsp; + Create restriction policy as a boundary <br>&emsp; + Create a bounded IAM user <br>&emsp; + Verify the effective permissions and clean up | 26/05/2026 | 26/05/2026 | [IAM with Resource Tags](https://000028.awsstudygroup.com/) <br> [IAM Permission Boundary](https://000030.awsstudygroup.com/) |
| 4 | - Practice **Encrypt at rest with AWS KMS**: <br>&emsp; + Prepare IAM users/roles for the lab <br>&emsp; + Create a KMS Customer Managed Key (CMK) <br>&emsp; + Create an S3 bucket and enable SSE-KMS <br>&emsp; + Configure CloudTrail and query KMS events with Athena <br>&emsp; + Test sharing encrypted objects and clean up | 27/05/2026 | 27/05/2026 | [Encrypt at Rest with AWS KMS](https://000033.awsstudygroup.com/) |
| 5 | - Practice **AWS Security Hub**: <br>&emsp; + Review supported security standards <br>&emsp; + Enable Security Hub and integrations <br>&emsp; + Read the security score and findings by standard <br>&emsp; + Clean up resources | 28/05/2026 | 28/05/2026 | [AWS Security Hub](https://000018.awsstudygroup.com/) |
| 6 | - Practice **Optimize EC2 cost with Lambda**: <br>&emsp; + Tag instances by start/stop schedule <br>&emsp; + Create IAM role for Lambda <br>&emsp; + Author Lambda function and trigger via EventBridge <br>&emsp; + Verify start/stop and clean up | 29/05/2026 | 29/05/2026 | [Optimize EC2 Cost with Lambda](https://000022.awsstudygroup.com/) |


### Week 6 Achievements:

* Understood the role of **Tags** as metadata for organizing AWS resources by purpose, owner, environment, or cost center, and applied a consistent tagging convention across EC2 and S3 resources.

* Created a **Resource Group** based on tag query to manage many resources as a single logical unit and reviewed how Resource Groups support automation across services.

* Applied the **principle of least privilege** in IAM by writing a policy with `Condition` blocks that grant EC2 actions only when the target resource carries a specific tag, and validated the behavior with an EC2 Administrator role.

* Configured an **IAM Permission Boundary** to cap the maximum permissions of a user, observed how the effective permissions are the intersection of the identity policy and the boundary, and used the pattern to mitigate privilege escalation risks.

* Created an **AWS KMS Customer Managed Key (CMK)** with appropriate key policies and used it to enable **SSE-KMS** encryption on an Amazon S3 bucket for data protection at rest.

* Enabled **AWS CloudTrail** to capture KMS and S3 events and queried the trail logs with **Amazon Athena** to audit who used the key, when, and against which objects.

* Practiced **sharing encrypted S3 objects** across IAM principals while keeping key access controlled through KMS grants and key policies, and completed full cleanup of KMS, S3, CloudTrail and Athena artifacts.

* Enabled **AWS Security Hub** and reviewed supported standards (e.g., AWS Foundational Security Best Practices, CIS) to get a centralized view of high-priority findings across services.

* Read the **security score by standard**, drilled into failed controls, and understood how Security Hub aggregates findings from sources such as GuardDuty, Inspector, and Macie into a single dashboard.

* Built an **EC2 cost optimization** workflow with **AWS Lambda**: tagged instances with start/stop schedules, created a Lambda execution role with `ec2:StartInstances` and `ec2:StopInstances`, and authored a function to act on tagged instances.

* Verified the start/stop logic by triggering the Lambda manually and on a schedule via EventBridge, confirming that only tagged instances were affected, and cleaned up the function, role, and policies after the lab.

* Reinforced the **cleanup discipline** at the end of every workshop to keep AWS bills predictable and the practice account tidy.
