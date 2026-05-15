---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 3 Objectives:

* Practice data protection with AWS Backup, including backup plans, notifications, and restore testing.
* Learn hybrid file storage using AWS Storage Gateway (File Gateway).
* Understand Amazon S3 fundamentals and static website hosting, including key features like versioning and replication.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Practice AWS Backup: <br>&emsp; + Deploy lab infrastructure <br>&emsp; + Create backup plan <br>&emsp; + Configure SNS notifications <br>&emsp; + Test restore and clean up | 04/05/2026 | 04/05/2026 | [AWS Backup](https://000013.awsstudygroup.com/) |
| 3 | - Practice AWS Storage Gateway (File Gateway): <br>&emsp; + Prepare environment <br>&emsp; + Create Storage Gateway and file share <br>&emsp; + Mount file share on on-premises machine <br>&emsp; + Clean up resources | 05/05/2026 | 05/05/2026 | [AWS Storage Gateway](https://000024.awsstudygroup.com/) |
| 4 | - Practice Amazon S3 fundamentals and static website hosting: <br>&emsp; + Create bucket and enable static website <br>&emsp; + Configure public access block and public objects <br>&emsp; + Test website access | 06/05/2026 | 06/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |
| 5 | - Continue with Amazon S3: <br>&emsp; + Accelerate static website with CloudFront <br>&emsp; + Enable bucket versioning <br>&emsp; + Move objects | 07/05/2026 | 07/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |
| 6 | - Complete Amazon S3 lab: <br>&emsp; + Configure cross-region replication <br>&emsp; + Review notes and best practices <br>&emsp; + Clean up resources | 08/05/2026 | 08/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |


### Week 3 Achievements:

* Understood the role of **AWS Backup** in centralizing and automating data protection for resources such as EBS, RDS, DynamoDB, and EFS.

* Deployed the lab infrastructure, created a **backup plan**, assigned resources to the plan, and configured backup schedules per the workshop policy.

* Configured **Amazon SNS** to receive notifications when backup or restore jobs complete for centralized status monitoring.

* Performed **restore testing** from backups and verified that recovered data met expectations.

* Completed **cleanup** of AWS Backup lab resources to avoid unnecessary charges.

* Understood the **hybrid storage** model with **AWS Storage Gateway (File Gateway)** — connecting on-premises file storage to object storage in S3.

* Prepared the lab environment (S3 bucket, EC2 gateway host), activated **Storage Gateway**, and created a **file share**.

* **Mounted the file share** on an on-premises machine, tested file read/write operations, and confirmed data synchronization to S3.

* Fully cleaned up Storage Gateway resources after completing the lab.

* Mastered core **Amazon S3** concepts: buckets, objects, durability (11 nines), and common use cases (websites, backup, data lakes).

* Created an **S3 bucket**, enabled **static website hosting**, configured **Block Public Access**, and set public objects as required by the lab.

* Validated website access via the S3 website endpoint and documented the static content delivery flow from object storage.

* Deployed **Amazon CloudFront** to accelerate static website delivery while restricting direct public access to the bucket.

* Enabled **bucket versioning** to protect objects from accidental overwrites or deletions; practiced **moving objects** between prefixes/buckets.

* Configured **cross-region replication (CRR)** to replicate objects to another Region for disaster recovery and data residency needs.

* Summarized **notes and best practices** for S3 cost, security, and operations; completed cleanup of buckets, distributions, and related resources.
