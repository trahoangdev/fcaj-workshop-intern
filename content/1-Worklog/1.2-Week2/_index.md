---
title: "Week 2 Worklog"
date: 2026-04-27
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

* Understood how Amazon Route 53 Resolver supports hybrid DNS between on-premises DNS systems and AWS-hosted private DNS zones.

* Installed, configured, and practiced using AWS CLI to interact with AWS services such as EC2, S3, SNS, IAM, and VPC.

* Learned how IAM Identity Center helps manage users, groups, permission sets, and access to multiple AWS accounts from a centralized place.

* Understood AWS Backup concepts, including backup plans, restore testing, notifications, and cleanup to avoid unnecessary costs.

* Studied VM Import/Export and its use cases for migration, backup, and disaster recovery scenarios.

* Practiced container application deployment with Docker, Docker Compose, Docker Hub, Amazon ECR, Amazon RDS, and Amazon EC2.

* Learned the main components of Amazon ECS:
  * ECS cluster
  * Task definition
  * ECS service
  * Application Load Balancer
  * Target group
  * Container Insights

* Understood CI/CD options for ECS deployments using GitLab CI/CD, GitHub Actions, and AWS CodeBuild.

* Learned how AWS Security Hub aggregates security findings and evaluates compliance against standards such as AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, and PCI DSS.

* Practiced network connectivity patterns with VPC Peering and AWS Transit Gateway, including route tables, attachments, and cleanup steps.
