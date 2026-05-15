---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 4 Objectives:

* Strengthen data protection practices with AWS Backup, including backup plans, SNS notifications, and restore testing.
* Explore **Amazon DynamoDB** as a serverless NoSQL service and learn how tables fit into a backup strategy.
* Practice VM migration and portability using VM Import/Export with VMware Workstation, Amazon S3, and EC2.
* Learn hybrid file storage with AWS Storage Gateway (File Gateway) and managed Windows file shares with Amazon FSx.
* Deepen Amazon S3 skills for static website hosting, CloudFront, versioning, and cross-region replication.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Explore **Amazon DynamoDB**: <br>&emsp; + Study table model, partition key, and sort key <br>&emsp; + Compare on-demand and provisioned capacity <br>&emsp; + Perform basic CRUD in the console and review metrics <br> - Practice AWS Backup: <br>&emsp; + Create S3 bucket and deploy lab infrastructure <br>&emsp; + Create a backup plan (including DynamoDB and other resources) <br>&emsp; + Configure SNS notifications <br>&emsp; + Test restore and clean up resources | 11/05/2026 | 11/05/2026 | [AWS Backup](https://000013.awsstudygroup.com/) <br> [DynamoDB Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) |
| 3 | - Practice VM Import/Export: <br>&emsp; + Prepare VMware Workstation environment <br>&emsp; + Export a virtual machine from on-premises <br>&emsp; + Upload the VM image to Amazon S3 and import to AWS <br>&emsp; + Deploy an EC2 instance from AMI <br>&emsp; + Export instance/AMI back to on-premises and clean up | 12/05/2026 | 12/05/2026 | [VM Import/Export](https://000014.awsstudygroup.com/) |
| 4 | - Practice AWS Storage Gateway (File Gateway): <br>&emsp; + Prepare S3 bucket and EC2 for Storage Gateway <br>&emsp; + Create Storage Gateway and file share <br>&emsp; + Mount file share on on-premises machine <br>&emsp; + Clean up resources | 13/05/2026 | 13/05/2026 | [AWS Storage Gateway](https://000024.awsstudygroup.com/) |
| 5 | - Practice Amazon FSx for Windows File Server: <br>&emsp; + Create environment and Multi-AZ file systems (SSD/HDD) <br>&emsp; + Create file shares and test performance <br>&emsp; + Enable deduplication, shadow copies, and user quotas <br>&emsp; + Scale throughput/storage and delete environment | 14/05/2026 | 14/05/2026 | [Amazon FSx](https://000025.awsstudygroup.com/) |
| 6 | - Practice Amazon S3 fundamentals and static website hosting: <br>&emsp; + Create bucket, enable static website, and configure public access <br>&emsp; + Accelerate website with CloudFront and enable versioning <br>&emsp; + Move objects and configure cross-region replication <br>&emsp; + Review notes, best practices, and clean up | 15/05/2026 | 15/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |


### Week 4 Achievements:

* Explored **Amazon DynamoDB** and understood its serverless NoSQL characteristics, table model (partition key, sort key), and capacity modes (on-demand vs provisioned).

* Practiced basic operations in the DynamoDB console: created a table, performed CRUD on items, and reviewed read/write metrics to become familiar with service operations.

* Understood the role of **AWS Backup** in centralizing and automating data protection for resources such as EBS, RDS, **DynamoDB**, and EFS.

* Deployed the lab infrastructure, created a **backup plan**, assigned resources (including DynamoDB tables) to the plan, and configured backup schedules based on defined policies.

* Configured **Amazon SNS** to receive notifications when backup or restore jobs complete, enabling centralized job status monitoring.

* Performed **restore testing** from backups and verified that data could be recovered as expected before applying the approach in production scenarios.

* Completed **cleanup** of lab resources after practice to avoid unnecessary ongoing charges.

* Understood **VM Import/Export** concepts and common use cases: workload migration to the cloud, on-premises VM backup, and disaster recovery.

* Prepared a **VMware Workstation** environment, exported a virtual machine from on-premises, and uploaded the image to **Amazon S3** as the intermediate step for import.

* Imported the VM image into AWS, created an **AMI**, and launched an **EC2 instance** from the imported AMI to validate that the workload runs in the cloud.

* Practiced the reverse flow: configured **S3 bucket ACL**, exported an instance/AMI from AWS back to on-premises, and cleaned up resources after the lab.

* Understood how **AWS Storage Gateway (File Gateway)** connects on-premises file storage with object storage in Amazon S3 using a hybrid model.

* Created an **S3 bucket**, provisioned EC2 as the gateway host, activated Storage Gateway, and created a **file share** following the workshop steps.

* **Mounted the file share** on an on-premises machine, tested file read/write operations, and confirmed that data was synchronized to S3.

* Understood the architecture of **Amazon FSx for Windows File Server**, including file servers, backend storage, VPC, ENIs, and multi-AZ replication.

* Deployed **Multi-AZ** file systems (SSD and HDD), created new **file shares**, and tested access performance from a Windows environment.

* Enabled and observed management features including **data deduplication**, **shadow copies**, user session/open file management, **storage quotas**, and **Continuous Access** shares.

* Practiced **scaling throughput** and **storage capacity**, monitored performance metrics, then deleted the environment and completed cleanup.

* Mastered core **Amazon S3** concepts: buckets, objects, durability, and common use cases (websites, backup, data lakes, analytics).

* Deployed **static website hosting** on S3, configured **Block Public Access**, public objects, and validated website access.

* Accelerated content delivery with **Amazon CloudFront** while restricting direct public access to the bucket for a more secure access pattern.

* Enabled **bucket versioning**, performed **object moves** between prefixes/buckets, and configured **cross-region replication (CRR)**.

* Summarized **notes and best practices** for S3 cost, security, and operations; completed cleanup of buckets, distributions, and related resources.
