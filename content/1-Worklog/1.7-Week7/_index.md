---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Deep-dive into **advanced AWS IAM** — roles, conditions, Permission Boundaries, Identity Center SSO — to apply least-privilege at enterprise scale.
* Understand and practice **network-level security** with AWS WAF and VPC Endpoints (PrivateLink) to protect workloads from the public internet.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Practice **IAM Role & Condition**: <br>&emsp; + Review IAM concepts (User, Group, Role, Policy) <br>&emsp; + Create IAM Group and IAM User for EC2 and RDS administrators <br>&emsp; + Create IAM Role with `Condition` (IP address and time restrictions) <br>&emsp; + Test assume-role behavior and clean up | 01/06/2026 | 01/06/2026 | [IAM Role & Condition](https://000044.awsstudygroup.com/) |
| 3 | - Practice **IAM Permission Boundary**: <br>&emsp; + Understand Permission Boundary vs. Identity-based Policy <br>&emsp; + Create a restriction policy and apply it as a Permission Boundary <br>&emsp; + Create a bounded IAM user and verify effective permissions <br>&emsp; + Observe privilege-escalation prevention in practice <br>&emsp; + Clean up resources | 02/06/2026 | 02/06/2026 | [IAM Permission Boundary](https://000030.awsstudygroup.com/) |
| 4 | - Practice **AWS IAM Identity Center (SSO)**: <br>&emsp; + Enable IAM Identity Center and set up the Identity Store <br>&emsp; + Create groups and users in the Identity Store <br>&emsp; + Assign permission sets to groups across AWS accounts <br>&emsp; + Configure AWS CLI access via Identity Center <br>&emsp; + Apply time-based access control and Customer Managed Policies <br>&emsp; + Use Identity Store APIs to manage identities programmatically <br>&emsp; + Clean up resources | 03/06/2026 | 03/06/2026 | [IAM Identity Center](https://000012.awsstudygroup.com/) |
| 5 | - Practice **AWS Web Application Firewall (WAF)**: <br>&emsp; + Review WAF concepts: WebACL, Rule, Rule Group, IP Set <br>&emsp; + Prepare a target resource (ALB / CloudFront) <br>&emsp; + Create and attach a WebACL with managed and custom rules <br>&emsp; + Test WAF block/allow behavior <br>&emsp; + Clean up resources | 04/06/2026 | 04/06/2026 | [AWS WAF](https://000026.awsstudygroup.com/) |
| 6 | - Practice **Secure Hybrid Access to S3 using VPC Endpoints**: <br>&emsp; + Understand Gateway vs. Interface VPC Endpoints <br>&emsp; + Create a Gateway VPC Endpoint for S3 and update route tables <br>&emsp; + Create an Interface VPC Endpoint for S3 (PrivateLink) <br>&emsp; + Access S3 from on-premises via the Interface Endpoint <br>&emsp; + Configure VPC Endpoint Policies to restrict access (Bonus) <br>&emsp; + Clean up resources | 05/06/2026 | 05/06/2026 | [VPC Endpoints for S3](https://000111.awsstudygroup.com/) |
| 7 | - Practice **AWS Security Hub**: <br>&emsp; + Review security standards (AWS FSBP, CIS, PCI DSS) <br>&emsp; + Enable Security Hub and linked integrations <br>&emsp; + Analyze security score and findings by standard <br>&emsp; + Understand aggregation from GuardDuty, Inspector, Macie <br>&emsp; + Clean up resources | 06/06/2026 | 06/06/2026 | [AWS Security Hub](https://000018.awsstudygroup.com/) |

### Week 7 Achievements:

* Reinforced the **IAM trust model** by distinguishing Identity-based Policies, Resource-based Policies, and Trust Policies for roles, then applied `Condition` blocks (IP CIDR and `DateGreaterThan`/`DateLessThan`) to restrict when and from where a role can be assumed.

* Created IAM Groups and scoped IAM Users for EC2 and RDS administrator personas, validated that each user can only act within their assigned service boundary, and practiced the full assume-role flow with condition evaluation.

* Understood and applied **IAM Permission Boundaries** as a guardrail layer that caps the maximum effective permissions regardless of the identity policy attached — a key control to prevent privilege escalation in multi-team environments.

* Observed the intersection logic: *effective permissions = Identity Policy ∩ Permission Boundary*, and used this pattern to safely delegate permission management to developers without risking over-privilege.

* Set up **AWS IAM Identity Center** (formerly AWS SSO) to provide centralized, federated access to multiple AWS accounts from a single identity source, eliminating per-account IAM user management.

* Configured permission sets mapped to Identity Store groups and assigned them to AWS accounts, then accessed the accounts via AWS CLI using Identity Center credentials — practicing Zero Trust principles at the organizational level.

* Applied **time-based access control** in Identity Center and managed identities programmatically through the **Identity Store APIs**, understanding how these capabilities support automated on-boarding and off-boarding workflows.

* Learned the core **AWS WAF** components — WebACL, Rules, Rule Groups, IP Sets, and Managed Rule Groups — and understood how WAF integrates with CloudFront, ALB, API Gateway, and AppSync as a Layer 7 defense.

* Created a WebACL with both AWS Managed Rules and a custom rate-limiting rule, attached it to a target resource, and confirmed block/allow behavior by sending test requests.

* Understood the two types of **VPC Endpoints**: Gateway Endpoints (route-table-based, for S3 and DynamoDB, no extra cost) and Interface Endpoints (ENI-based, DNS-resolved, for PrivateLink services).

* Created a **Gateway VPC Endpoint** for S3, updated the associated route table, and verified that traffic from EC2 instances no longer traverses the public internet when accessing S3.

* Created an **Interface VPC Endpoint** (PrivateLink) for S3 and configured it to accept connections from an on-premises environment, understanding how private DNS and security groups control access.

* Configured **VPC Endpoint Policies** to grant fine-grained, resource-level access control on top of IAM — a defense-in-depth layer that limits which S3 buckets can be accessed through a given endpoint.

* Enabled **AWS Security Hub** and reviewed the aggregated findings dashboard, understanding how it pulls findings from GuardDuty, Inspector, Macie, and other sources into a single prioritized view aligned to standards such as AWS FSBP and CIS Benchmarks.

* Consistently applied **cleanup discipline** across all labs to prevent unexpected charges and maintain a tidy practice account.
