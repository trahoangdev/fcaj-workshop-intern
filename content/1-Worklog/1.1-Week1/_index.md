---
title: "Week 1 Worklog"
date: 2024-01-01
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

* Completed all **5 foundational AWS Study Group workshops** during the internship onboarding week.

* Understood the **AWS Free Tier 2025** structure, the difference between Free Plan and Paid Plan, and requirements to receive the full **$200 credit**.

* Identified **credit-consuming services** (EC2, NAT Gateway, Load Balancer, etc.) and created a personal **cost-control checklist** for Week 1.

* Understood the **IAM** model: users, groups, roles, policies, and the **least privilege** principle for AWS access control.

* Created **IAM Groups** and **IAM Users**, attached policies, and **verified permissions** in the AWS Management Console.

* Created an **IAM Role**, performed **role switching**, and documented the difference between direct user permissions and permissions via roles.

* Understood **Amazon VPC** concepts: CIDR blocks, public/private subnets, route tables, and resource isolation in a virtual network.

* Configured baseline **Security Groups** and **NACLs** to control inbound/outbound traffic at different security layers.

* Followed the **Site-to-Site VPN** setup flow between a simulated on-premises environment and an AWS VPC; **validated connectivity** and completed lab cleanup.

* Mastered core **EC2** concepts: instance types, AMIs, key pairs, security groups, and instance connectivity methods.

* Launched **EC2 Windows Server** and **Amazon Linux** instances, connected to each OS, and performed basic operational tasks.

* **Deployed the sample Node.js application** on EC2 and recorded security notes (open ports, SSH/RDP) and cost considerations for continuously running instances.

* Understood **Amazon RDS** as a managed relational database service, including provisioning, automated backups, and basic high availability.

* Created an **RDS DB instance**, **connected an application** to the database, and **validated backup and restore** per the workshop workflow.

* Practiced **end-to-end lab workflows**: environment setup → deployment → validation → **cleanup** and cost review in the Billing Console after each lab day.

* Built a habit of reading workshop/AWS documentation and translating it into verifiable implementation steps on the console.
