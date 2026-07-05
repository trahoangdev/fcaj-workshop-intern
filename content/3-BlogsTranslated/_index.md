---
title: "Translated Blogs"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

<!-- This section will list and introduce the blogs you have translated. For example: -->

###  [Blog 1 - Cyber Resilience on AWS: Recovery from ransomware and destructive events](3.1-blog1/)
This blog summarizes an AWS reference approach for building cyber resilience for critical workloads. It focuses on separating production, the recovery account, and the isolated recovery environment, using AWS Backup logically air-gapped vaults to protect recovery points, validating backups before restore, selecting a safe recovery point, and applying the Rebuild-Restore-Rotate framework.

###  [Blog 2 - Building a Multi Account Patch Compliance Dashboard with Kiro Specs](3.2-blog2/)
This blog summarizes how AWS builds a multi-account patch compliance dashboard using AWS Systems Manager Patch Manager, Resource Data Sync, Amazon S3, AWS Lambda, EventBridge, an internal ALB, and Session Manager. It also focuses on how Kiro Specs, steering files, and MCP servers support a spec-driven development workflow.

###  [Blog 3 - Architecting an AI-Powered Resilience Framework on AWS](3.3-blog3/)
This blog summarizes how AWS combines AWS Resilience Hub, AWS Fault Injection Service, Amazon Bedrock AgentCore, AWS Systems Manager, AWS Config, and CloudWatch to build a continuous resilience testing framework. It focuses on five layers: discovery, test generation, experimentation, gap analysis, and continuous validation in CI/CD.

### [Blog 4 - Building a Scalable User Search Layer on top of Amazon Cognito](3.4-blog4/)
This blog shares how to solve the challenge of fuzzy matching and filtering user data at scale, by combining Cognito, DynamoDB Streams, and OpenSearch Serverless into an event-driven architecture that completely decouples the Command and Query layers.

### [Blog 5 - Create a Serverless Custom Retry Mechanism for Stateless Queue Consumers](3.5-blog5/)
This blog presents a solution for handling rate-limiting from fragile downstream systems by using Amazon EventBridge Scheduler instead of heavy Step Functions, offering flexible control and cost optimization through serverless architecture.

### [Blog 6 - Revolutionizing Core Logging with Machine Learning and Amazon EKS](3.6-blog6/)
This blog analyzes a hybrid architecture that combines the flexibility of AWS Lambda with the massive compute power of Amazon EKS, solving the problem of unpredictable industrial workloads and maximizing the "scale to zero" feature to save costs in the mining industry.
