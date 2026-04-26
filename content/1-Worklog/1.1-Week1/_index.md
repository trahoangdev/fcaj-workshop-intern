---
title: "Week 1 Worklog"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 1 Objectives:

* Complete 5 foundational AWS Study Group workshops for cloud onboarding.
* Build a strong AWS foundation in account setup, IAM, networking, compute, and database.
* Practice hands-on deployment and cleanup to avoid unnecessary AWS costs.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Complete workshop **AWS Free Tier 2025** <br> - Study the key topics: <br>&emsp;+ Free Plan vs Paid Plan <br>&emsp;+ Strategy to get full $200 credit <br>&emsp;+ Credit-killer services to avoid <br> - Create a personal cost-control checklist for Week 1 | 20/04/2026 | 20/04/2026 | [Workshop](https://000001.awsstudygroup.com/) <br> [AWS Free Tier Docs](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier.html) |
| 3 | - Complete workshop **IAM Access Control** <br> - **Practice:** <br>&emsp;+ Create IAM Groups and IAM Users <br>&emsp;+ Attach policies and verify permissions <br>&emsp;+ Create IAM Roles and perform role switching <br> - Summarize least-privilege implementation notes | 21/04/2026 | 21/04/2026 | [Workshop](https://000002.awsstudygroup.com/) <br> [IAM Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) |
| 4 | - Complete workshop **Amazon VPC and Site-to-Site VPN** <br> - **Practice:** <br>&emsp;+ Create a VPC and core subnets <br>&emsp;+ Configure baseline Security Group and NACL rules <br>&emsp;+ Follow Site-to-Site VPN setup flow <br> - Validate connectivity and perform resource cleanup | 22/04/2026 | 22/04/2026 | [Workshop](https://000003.awsstudygroup.com/) <br> [VPC Docs](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), [Site-to-Site VPN Docs](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html) |
| 5 | - Complete workshop **Introduction to Amazon EC2** <br> - **Practice:** <br>&emsp;+ Launch Windows Server and Amazon Linux EC2 instances <br>&emsp;+ Connect to instances and perform basic operations <br>&emsp;+ Deploy the sample Node.js application on EC2 <br> - Record EC2 security and cost governance notes | 23/04/2026 | 23/04/2026 | [Workshop](https://000004.awsstudygroup.com/) <br> [EC2 Docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) |
| 6 | - Complete workshop **Amazon RDS** <br> - **Practice:** <br>&emsp;+ Create an Amazon RDS database instance <br>&emsp;+ Connect application workflow to the database <br>&emsp;+ Run backup and restore validation <br> - Clean up all resources and re-check cost impact | 24/04/2026 | 24/04/2026 | [Workshop](https://000005.awsstudygroup.com/) <br> [RDS Docs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |


### Week 1 Achievements:

* Completed all 5 planned foundational workshops in Week 1.

* Built practical understanding across key AWS domains:
  * Account and cost management (Free Tier 2025)
  * Identity and permissions (IAM users, groups, roles)
  * Networking and hybrid connectivity (VPC + Site-to-Site VPN)
  * Compute and application deployment (EC2 on Windows/Linux)
  * Managed relational database services (Amazon RDS)

* Practiced end-to-end lab execution from environment setup to cleanup, reducing the risk of unnecessary charges.

* Strengthened security-first thinking through least privilege, role-based access, and secure network segmentation.

* Improved confidence in reading technical workshop documentation and translating it into hands-on implementation steps.

### Challenges and How I Resolved Them:

* Challenge: The amount of new AWS concepts in the first week was large and easy to confuse.
  Resolution: I organized notes by domain (Account, IAM, Network, Compute, Database) and reviewed each workshop summary after finishing the lab.

* Challenge: IAM permissions and role assumptions were initially difficult to visualize.
  Resolution: I repeated the IAM lab steps and compared permission boundaries between users, groups, and roles.

* Challenge: Risk of unexpected charges when creating cloud resources.
  Resolution: I followed cleanup steps at the end of each lab and cross-checked resources in the AWS console.

### Lessons Learned:

* Good cloud operations start with cost awareness and security fundamentals.
* Clear IAM design and least-privilege access are essential from day one.
* Practicing complete workflows (create, test, clean up) builds reliable habits.

### Plan for Week 2:

* Continue hands-on practice with core AWS services in deeper scenarios.
* Document lab outputs with screenshots and key commands for better evidence.
* Improve speed and accuracy when troubleshooting deployment and configuration issues.
