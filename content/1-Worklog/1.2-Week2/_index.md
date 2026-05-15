---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 2 Objectives:

* Strengthen hands-on practice with AWS networking, identity, backup, migration, container, security, and CI/CD services.
* Understand how to configure AWS resources through both the AWS Management Console and AWS CLI.
* Practice deploying applications with Docker, Amazon ECS, and automated CI/CD workflows.
* Learn how to design secure and scalable network connectivity with VPC Peering and AWS Transit Gateway.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Study hybrid DNS architecture with Amazon Route 53 Resolver: <br>&emsp; + Review inbound endpoint, outbound endpoint, and resolver rules <br>&emsp; + Deploy the lab infrastructure with CloudFormation <br>&emsp; + Configure Microsoft AD and DNS forwarding between on-premises simulation and AWS <br>&emsp; + Test DNS resolution results <br> - Practice AWS CLI: <br>&emsp; + Install and configure AWS CLI profile <br>&emsp; + View EC2, S3, IAM, VPC, and SNS resources via CLI <br>&emsp; + Create EC2 resources using CLI commands <br> - Practice IAM Identity Center: <br>&emsp; + Create AWS accounts in AWS Organizations <br>&emsp; + Set up organizational units, users, groups, and permission sets <br>&emsp; + Test console and CLI access with assigned permissions | 27/04/2026 | 27/04/2026 | [Route 53 Resolver](https://000010.awsstudygroup.com/) <br> [AWS CLI](https://000011.awsstudygroup.com/) <br> [IAM Identity Center](https://000012.awsstudygroup.com/) |
| 3 | - Practice AWS Backup: <br>&emsp; + Create an S3 bucket and deploy lab resources <br>&emsp; + Create a backup plan for AWS resources <br>&emsp; + Configure backup notifications <br>&emsp; + Test restore from backup and clean up resources <br> - Practice VM Import/Export: <br>&emsp; + Prepare VMware Workstation environment <br>&emsp; + Export a virtual machine from on-premises simulation <br>&emsp; + Upload the VM image to Amazon S3 <br>&emsp; + Import the VM to AWS and deploy an EC2 instance from the generated AMI | 28/04/2026 | 28/04/2026 | [AWS Backup](https://000013.awsstudygroup.com/) <br> [VM Import/Export](https://000014.awsstudygroup.com/) |
| 4 | - Practice deploying applications with Docker: <br>&emsp; + Install dependencies and run the application locally <br>&emsp; + Configure VPC, security group, and IAM role for ECR access <br>&emsp; + Launch and configure Amazon RDS <br>&emsp; + Configure EC2 instance for container deployment <br>&emsp; + Deploy the application using Docker image and Docker Compose <br>&emsp; + Push container image to Amazon ECR or Docker Hub | 29/04/2026 | 29/04/2026 | [Docker Deployment](https://000015.awsstudygroup.com/) |
| 5 | - Practice deploying applications on Amazon ECS: <br>&emsp; + Create ECS cluster with AWS Fargate <br>&emsp; + Create backend and frontend task definitions <br>&emsp; + Configure target groups and Application Load Balancer <br>&emsp; + Create backend ECS service with blue/green deployment <br>&emsp; + Create frontend ECS service with rolling deployment <br>&emsp; + Test application access and verify service status | 30/04/2026 | 30/04/2026 | [Amazon ECS](https://000016.awsstudygroup.com/) |
| 6 | - Practice CI/CD deployment for ECS: <br>&emsp; + Configure GitLab pipeline and GitLab Runner <br>&emsp; + Configure GitHub Actions workflow for ECS deployment <br>&emsp; + Create frontend and backend CodeBuild projects <br>&emsp; + Verify deployment results and monitor application logs <br> - Practice AWS Security Hub: <br>&emsp; + Enable Security Hub <br>&emsp; + Review security standards and security score <br>&emsp; + Check findings and clean up resources <br> - Practice AWS networking: <br>&emsp; + Create VPC Peering connection and update route tables <br>&emsp; + Configure cross-peer DNS resolution <br>&emsp; + Create Transit Gateway, attachments, and route tables <br>&emsp; + Add Transit Gateway routes to VPC route tables and test connectivity | 01/05/2026 | 01/05/2026 | [ECS CI/CD](https://000017.awsstudygroup.com/) <br> [Security Hub](https://000018.awsstudygroup.com/) <br> [VPC Peering](https://000019.awsstudygroup.com/) <br> [Transit Gateway](https://000020.awsstudygroup.com/) |


### Week 2 Achievements:

* Understood **hybrid DNS** architecture with **Amazon Route 53 Resolver**: inbound endpoints, outbound endpoints, and resolver rules.

* Deployed lab infrastructure with **CloudFormation**, configured **Microsoft AD**, and set up DNS forwarding between simulated on-premises and AWS.

* Validated **DNS resolution** in both directions and documented query flows between on-premises DNS and AWS private hosted zones.

* Installed the **AWS CLI**, configured profiles (access keys, region, output format), and practiced foundational commands.

* Used the CLI to **list and manage** EC2, S3, IAM, VPC, and SNS resources; created EC2 resources from the command line.

* Understood **IAM Identity Center** (SSO) in the context of **AWS Organizations** for centralized multi-account access.

* Created **AWS accounts**, **organizational units (OUs)**, users, groups, and **permission sets**; assigned role-based access.

* Verified **console** and **CLI** sign-in through Identity Center with configured permission sets.

* Understood **AWS Backup** for centralized protection of EBS, RDS, DynamoDB, and EFS; created a **backup plan** and assigned resources.

* Configured **backup notifications** (SNS), performed **restore testing**, and completed Backup lab **cleanup**.

* Mastered **VM Import/Export**: exported a VM from on-premises, uploaded the image to **S3**, imported an **AMI**, and launched **EC2**.

* Prepared **VMware Workstation** and completed the export → upload → import migration workflow from the workshop.

* Ran the application **locally** with dependencies; understood Dockerfile basics and image packaging workflows.

* Configured **VPC**, security groups, and an **IAM role** for EC2 access to **Amazon ECR**.

* Provisioned **Amazon RDS** and integrated the database into the container application flow on EC2.

* **Deployed** the application with Docker images and **Docker Compose**; **pushed images** to ECR or Docker Hub.

* Created an **ECS cluster** with **AWS Fargate** (serverless containers without managing EC2 hosts).

* Defined **task definitions** for backend and frontend, including container ports, images, and environment variables.

* Set up an **Application Load Balancer**, **target groups**, and traffic routing to ECS services.

* Deployed a backend service with **blue/green deployment** and a frontend service with **rolling deployment**; verified application access and **Container Insights**.

* Configured a **GitLab pipeline** and **GitLab Runner** for automated build and deploy workflows.

* Set up a **GitHub Actions workflow** for ECS deployment and created **CodeBuild projects** for frontend and backend.

* Monitored **deployment results** and **application logs** after each pipeline run.

* Enabled **AWS Security Hub**, reviewed **security standards**, security scores, and **findings**.

* Evaluated findings against **AWS Foundational Security Best Practices**, **CIS**, and **PCI DSS**; completed Security Hub lab cleanup.

* Created a **VPC Peering connection**, updated **route tables**, and configured **cross-peer DNS resolution**.

* Deployed **AWS Transit Gateway** with attachments and route tables, added routes to VPCs, and **tested multi-VPC connectivity**.

* Completed **cleanup** of networking, Security Hub, and CI/CD resources to avoid unnecessary charges.
