---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Cyber Resilience on AWS: A Reference Approach for Recovery from Ransomware and Destructive Events

## References

- AWS Architecture Blog: [Cyber resilience on AWS: A reference approach for recovery from ransomware and destructive events](https://aws.amazon.com/vi/blogs/architecture/cyber-resilience-on-aws-a-reference-approach-for-recovery-from-ransomware-and-destructive-events/)
- Authors: Ashish Panwar, Kanniah Vagathupatti Jaikumar, and Rakesh Singh
- Published: May 20, 2026

## Overview

When discussing security, it is common to focus on prevention and detection: keeping attackers out and finding suspicious activity quickly. The AWS blog highlights another important area: **cyber resilience**, which is the ability to recover workloads to a trustworthy state after ransomware, data extortion, or destructive cyber events.

The main idea I learned from this post is that recovery cannot assume everything is still trusted. After a serious incident, the production environment, credentials, backups, and even the recovery path may have been targeted. Therefore, a good recovery strategy is not only about having backups. It also needs protected backup storage, an isolated recovery environment, validation before restore, and a clear decision process for choosing a safe recovery point.

## Problem Context

For critical workloads on AWS, ransomware can create more risk than encrypted data alone. An attacker may attempt to delete backups, change configurations, steal credentials, or leave malicious changes in the environment before the incident is detected.

If a team restores the latest backup without validating it, the restored environment may still contain the same issue that caused the incident. This is especially risky when the attacker was present before detection and recent backups were created during that compromise window.

A cyber recovery plan should answer several practical questions:

- Are backups protected from deletion or shortened retention?
- Is the recovery environment isolated from the compromised production environment?
- How can the team identify which recovery point is safe to use?
- Which components should be rebuilt from code, restored from backup, or generated fresh?

## Reference Recovery Architecture

The AWS blog describes a three-account pattern inside AWS Organizations.

![Isolated Recovery Environment](../../images/3-BlogsTranslated/Blog1/IsolatedRecoveryEnvironment(IRE).png)

*Source: AWS Architecture Blog - Cyber resilience on AWS*

### Production Account

The production account is where the workload normally runs. After a confirmed cyber event, this account should be isolated for investigation. Recovery should not happen directly in the old production environment because its identities, network paths, or configurations may no longer be fully trusted.

### Recovery Account

The recovery account manages critical backups, especially the **AWS Backup logically air-gapped vault**. This vault protects recovery points from deletion during the retention period, even if a root user or administrator account is compromised.

This account should be restricted with Service Control Policies (SCPs) and focused on backup and restore operations. By separating backup controls from production, a compromised production identity cannot easily change or delete protected recovery points.

One important detail is that the logically air-gapped vault is designed around the idea that recovery points must remain available long enough for investigation and recovery. It uses strict protection so recovery points cannot be deleted or have their retention shortened during the configured retention period. For Amazon S3 data, the article also mentions a similar protection pattern using **S3 Versioning** and **S3 Object Lock in Compliance mode** to protect object versions from unwanted deletion or overwrite.

### Isolated Recovery Environment (IRE)

The IRE is where backups are restored, validated, and used to rebuild the new environment before cutover. It has no trust relationship with the production account, no VPC peering to production, and no public internet-facing resources.

This design limits blast radius. If a restored backup is still unsafe, the issue remains contained inside the IRE instead of spreading back to production or outside the AWS environment.

## AWS Services Involved

- **AWS Backup**: manages backup and restore across supported AWS resources.
- **AWS Backup logically air-gapped vault**: protects recovery points from deletion during retention.
- **AWS Resource Access Manager (AWS RAM)**: shares recovery points across accounts.
- **Amazon S3 Versioning and S3 Object Lock**: protect S3 object versions from deletion or overwrite when strict retention is required.
- **IAM Identity Center and Multi-party approval (MPA)**: require approval from multiple predefined approvers before restore.
- **Amazon GuardDuty Malware Protection**: scans restored volumes or backup contents for malware.
- **AWS CloudTrail, VPC Flow Logs, and AWS Security Hub**: help build an investigation timeline and identify suspicious activity.
- **AWS Config and IAM Access Analyzer**: help identify cross-account dependencies, policies, and trust relationships before cutover.
- **AWS PrivateLink / VPC endpoints**: allow the IRE to call AWS service APIs without opening internet connectivity.

## End-to-End Recovery Workflow

The original article does not only describe architecture components. It also shows how recovery should be operated in stages. I summarize the workflow into five main steps:

![Recovery workflow](../../images/3-BlogsTranslated/Blog1/Recovery_workflow.jpg)

*Source: AWS Architecture Blog - Cyber resilience on AWS*

1. **Establish the timeline**: reconstruct the incident timeline from logs, alerts, and indicators of compromise.
2. **Validate candidates**: choose potentially safe recovery points and validate them inside the IRE.
3. **Approval**: require approval before using a recovery point, especially for critical workloads.
4. **Rebuild and restore**: rebuild clean infrastructure, restore validated data, and rotate all secrets.
5. **Cutover**: check dependencies, move traffic to the new environment, and continue monitoring.

What I like about this workflow is that it avoids the rushed approach of restoring the latest backup and reopening the system immediately. Each stage includes validation or approval to reduce the chance of bringing the threat back into production.

## Validation Pipeline: Restore Does Not Mean Safe

One useful point from the article is that a successful restore only proves the backup can be read. It does not prove the backup is safe.

AWS recommends combining multiple validation layers:

- Restore testing to verify backups are recoverable.
- Malware scanning to detect known malicious tools or indicators.
- Workload-specific checks such as database consistency checks or configuration comparison against a known-good baseline.
- Log and audit review to identify unexpected identity, network, or configuration changes.

These checks should run inside the IRE. A recovery point should only be approved after it passes both technical validation and the organization’s approval process.

## Selecting a Safe Recovery Point

For normal operational recovery, the most recent backup is often the best option. For cyber events, the most recent backup might not be safe because it may have been created after the attacker entered the environment.

![Selecting a safe recovery point](../../images/3-BlogsTranslated/Blog1/Selecting_a_safe_recovery_point.png)

*Source: AWS Architecture Blog - Cyber resilience on AWS*

A safer process is:

1. Build an investigation timeline using CloudTrail, VPC Flow Logs, GuardDuty, Security Hub, and workload logs.
2. Identify the earliest plausible indicator of the event.
3. Consider recovery points created before that event boundary.
4. Validate candidates in reverse chronological order, starting with the newest candidate before the boundary.
5. If validation fails, move to an older recovery point.
6. Document the selected recovery point, validation result, approver, and reason.

This process reduces the chance of restoring compromised data or configuration.

## Checks Before Cutover

After rebuild and restore are complete, traffic should not be moved immediately. The new environment still needs careful checks for dependencies across accounts, services, and identities.

Some items to review before cutover include:

- IAM role trust policies and resource-based policies.
- AWS KMS key policies, grants, and encrypt/decrypt permissions.
- Cross-account references or service integrations that still point to the old production environment.
- Security groups, route tables, DNS records, and endpoint configuration.
- Log forwarding, monitoring, alerting, and backup plans for the new environment.

In my view, this is one of the easiest areas to miss during recovery. Even if the data is restored successfully, the system can still fail or keep security risk if KMS permissions are missing, DNS cutover is incorrect, or IAM trust policies still reference the old account.

## Rebuild-Restore-Rotate Framework

The blog summarizes cyber recovery with a simple framework:

| Category | Examples | Recovery action |
| --- | --- | --- |
| Rebuild | VPC, security groups, IAM roles, Lambda functions, CI/CD pipelines | Rebuild from reviewed Infrastructure as Code or a trusted source repository |
| Restore | RDS, Aurora, EBS, EFS, FSx, business data | Restore from validated backups |
| Rotate | Passwords, API keys, access keys, certificates, SSH keys | Rotate or re-issue instead of reusing old secrets |

The key lesson is that not everything should be restored from backup. Infrastructure and configuration should come from trusted code. Business data should come from validated backups. Credentials should be replaced because it is difficult to prove they were not exposed during the event.

## What I Learned

Before reading this article, I mostly thought of backup as the core of disaster recovery. After studying the post, I realized that ransomware recovery requires a stricter mindset. A backup is only useful if it is protected, validated, and restored into an environment that does not inherit trust from compromised production.

I also learned why AWS account separation is valuable. AWS Organizations, SCPs, AWS RAM, AWS Backup vaults, and IAM controls can work together to create clear trust boundaries. This is stronger than keeping all recovery resources inside the same production account.

## Conclusion

Cyber resilience on AWS is not only about defending against attacks. It is also about preparing to recover when parts of the environment can no longer be trusted. The reference approach with a Recovery Account, IRE, logically air-gapped vault, validation pipeline, and Rebuild-Restore-Rotate framework gives organizations a clearer recovery process for ransomware and destructive events.

For a real system, I think the first practical steps should be:

- Create a Recovery Account and logically air-gapped vault for critical workloads.
- Prepare the IRE in advance instead of building it only after an incident.
- Enable AWS Backup Restore Testing and run restore tests regularly.
- Enable GuardDuty Malware Protection as part of the validation process.
- Define workload-specific integrity checks such as database checks, file hashes, or baseline configuration comparison.
- Write a runbook for Rebuild-Restore-Rotate.
- Practice credential rotation and cutover regularly.

Waiting until an incident occurs would make recovery slower, riskier, and more error-prone. This article helped me understand that cyber recovery must be designed, tested, and practiced in advance, just like a required part of the system architecture instead of an optional document created after deployment.
