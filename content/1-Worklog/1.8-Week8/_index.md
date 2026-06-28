---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 8 Objectives:

* Study the business problem and MVP scope for **DocuFlow AI** - a serverless intelligent invoice and receipt processing platform on AWS.
* Identify the required AWS services, operating cost, technical risks, and high-level solution architecture.
* Prepare the initial project proposal as the baseline for the workshop and module ownership.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Study the **DocuFlow AI** project overview <br>&emsp; + Identify the pain points of manual invoice and receipt data entry <br>&emsp; + Lock the MVP scope around invoice/receipt processing <br>&emsp; + Identify user roles: EndUser, Reviewer, Admin | 06/08/2026 | 06/08/2026 | [Amazon Textract - Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/invoices-receipts.html) |
| 3 | - Study AWS services required for the project <br>&emsp; + Cognito, API Gateway, Lambda, S3 <br>&emsp; + EventBridge, SQS, Step Functions <br>&emsp; + Textract, Bedrock, DynamoDB <br>&emsp; + CloudWatch, SNS/SES, IAM/KMS | 06/09/2026 | 06/09/2026 | [Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) <br> [API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) <br> [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) <br> [S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) <br> [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) <br> [Textract](https://docs.aws.amazon.com/textract/latest/dg/what-is.html) <br> [Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) |
| 4 | - Analyze project cost <br>&emsp; + Identify services billed by request, page, or token <br>&emsp; + Review cost drivers for Textract, Bedrock, Step Functions, S3, Lambda, and CloudWatch <br>&emsp; + Propose demo file limits, budget alerts, and cleanup after the workshop | 06/10/2026 | 06/10/2026 | [AWS Pricing Calculator](https://calculator.aws/) <br> [Textract Pricing](https://aws.amazon.com/textract/pricing/) <br> [Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/) <br> [Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| 5 | - Analyze project risks <br>&emsp; + Oversized scope if the system targets all business document types <br>&emsp; + Textract extraction errors for low-quality documents <br>&emsp; + Bedrock returning JSON that does not match the schema <br>&emsp; + Overly broad IAM or unexpected AI service cost | 06/11/2026 | 06/11/2026 | [Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) <br> [Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) <br> [IAM Security Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| 6 | - Draw the solution architecture <br>&emsp; + Design the upload flow with presigned URL and S3 raw bucket <br>&emsp; + Design the EventBridge, SQS, Step Functions, Textract, and Bedrock workflow <br>&emsp; + Define the metadata path to DynamoDB and processed output path to S3 | 06/12/2026 | 06/12/2026 | [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [S3 EventBridge Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html) <br> [SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) <br> [Step Functions Workflow Type](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html) <br> [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) |
| 7 | - Prepare the project proposal <br>&emsp; + Write the executive summary, problem statement, MVP scope, and success criteria <br>&emsp; + Document the architecture, service selection, workflow, risks, and cost control <br>&emsp; + Split the project into five modules and define deliverables | 06/13/2026 | 06/13/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [Serverless Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html) <br> [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) |


### Week 8 Achievements:

* Understood the business problem of **DocuFlow AI**: reducing manual data entry for invoices and receipts, improving status tracking, normalizing extracted data, and supporting auditability for Finance/Ops teams.

* Locked the MVP scope to a workshop-friendly end-to-end flow:
  * Users authenticate with Cognito.
  * Users upload PDF/JPG/PNG files through the frontend and a presigned URL.
  * The S3 raw bucket triggers an asynchronous processing workflow.
  * Textract extracts invoice/receipt data.
  * Bedrock normalizes, classifies, and returns output using a shared JSON schema.
  * DynamoDB stores metadata/status, and S3 processed stores JSON/CSV results.
  * CloudWatch and SNS/SES notify failures or low-confidence documents.

* Identified the main AWS services and their roles in the serverless, event-driven architecture:
  * **Cognito** for EndUser, Reviewer, and Admin authentication/authorization.
  * **API Gateway + Lambda** for presigned URL creation, status updates, and lightweight backend logic.
  * **S3** for raw documents and processed results.
  * **EventBridge + SQS + Step Functions** for asynchronous processing, retry, and execution audit.
  * **Textract + Bedrock** for extraction, normalization, classification, and schema-based output.
  * **DynamoDB** for the status table and metadata.
  * **CloudWatch + SNS/SES** for logs, metrics, alarms, and notifications.
  * **IAM/KMS** for least privilege and data encryption.

* Analyzed cost and proposed control measures:
  * Limit the demo to 5-10 small invoice/receipt files.
  * Track services billed by request, page, or token, especially Textract, Bedrock, and Step Functions.
  * Query only the processed demo folder when using Athena to avoid unnecessary scan cost.
  * Configure S3 lifecycle policies for raw/processed data and prepare cleanup/destroy commands after the demo.
  * Set up budget alerts to catch unexpected cost.

* Built the initial risk matrix and mitigation plan:
  * Oversized scope: keep the MVP focused on invoice/receipt processing and move contracts, purchase orders, email ingestion, and advanced QuickSight dashboards to extensions.
  * OCR errors on low-quality documents: use confidence scores, a REVIEW_REQUIRED threshold, and a manual review loop.
  * Bedrock schema mismatch: define strict prompts/schema, validate output with Lambda, and add retry/fallback behavior.
  * Overly broad IAM: create module-specific roles, apply least privilege, and avoid hard-coded access keys.
  * Unexpected AI cost: limit demo files, enable budget alerts, and clean up resources after the workshop.

* Completed the first end-to-end architecture draft for DocuFlow AI, covering client/access, ingestion/workflow, AI processing, storage, analytics, observability, and DevOps/IaC layers.

* Completed the initial project proposal with the following sections:
  * Executive summary and problem statement.
  * MVP scope, out-of-scope items, and success criteria.
  * Solution architecture and service selection.
  * End-to-end workflow.
  * Data contract, status model, and five-person module ownership.
  * Security baseline, observability, analytics, cost control, risk mitigation, and definition of done.
