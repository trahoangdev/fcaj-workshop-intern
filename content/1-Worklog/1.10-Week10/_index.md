---
title: "Week 10 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 10 Objectives:

* Revise the **DocuFlow AI** architecture after evaluating that Bedrock should not be a mandatory dependency for the Free Tier/workshop environment.
* Replace the Bedrock normalization step with an **external AI provider/API** called through a Lambda adapter while keeping the post-Textract JSON normalization flow.
* Rewrite the proposal and workshop so the architecture, cost model, risks, and implementation steps match the new design.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Review Free Tier and AI access constraints <br>&emsp; + Check Bedrock pricing and model access requirements <br>&emsp; + Decide that Bedrock should not be mandatory in the MVP <br>&emsp; + Move to an external AI approach to reduce account/policy risk | 06/22/2026 | 06/22/2026 | [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/) <br> [AWS Free Tier](https://aws.amazon.com/free/) |
| 3 | - Revise the system architecture <br>&emsp; + Remove the Bedrock state from the Step Functions workflow <br>&emsp; + Add an `externalAiNormalize` Lambda that calls an external AI API <br>&emsp; + Store the API key in Secrets Manager instead of code or frontend config <br>&emsp; + Update the diagram and data flow | 06/23/2026 | 06/23/2026 | [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) <br> [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) <br> [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) |
| 4 | - Update the data contract, status model, and error handling <br>&emsp; + Keep the shared JSON schema for invoices/receipts <br>&emsp; + Add `EXTERNAL_AI_FAILED` handling or map it to `REVIEW_REQUIRED`/`FAILED` <br>&emsp; + Update workflow retry/catch behavior <br>&emsp; + Adjust the IAM role for the Lambda adapter | 06/24/2026 | 06/24/2026 | [Step Functions Error Handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html) <br> [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 5 | - Update cost and risk analysis <br>&emsp; + Remove Bedrock from the AWS baseline cost table <br>&emsp; + Add external AI provider cost by request/token <br>&emsp; + Add risks for API keys, quota, latency, network timeout, and sensitive data leaving AWS <br>&emsp; + Update cost control and cleanup checklists | 06/25/2026 | 06/25/2026 | [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) <br> [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) <br> [Secrets Manager Pricing](https://aws.amazon.com/secrets-manager/pricing/) |
| 6 | - Rewrite the proposal based on the new architecture <br>&emsp; + Update the executive summary, architecture, service selection, and workflow <br>&emsp; + Replace Bedrock with the External AI Lambda Adapter <br>&emsp; + Update the timeline, cost estimation, risk mitigation, and definition of done | 06/26/2026 | 06/26/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [Serverless Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html) |
| 7 | - Rewrite the workshop based on the updated proposal <br>&emsp; + Update overview, prerequisites, and the AI extraction module <br>&emsp; + Rewrite the external AI secret configuration steps <br>&emsp; + Update deploy steps, test cases, evidence, and cleanup <br>&emsp; + Re-check workshop internal links | 06/27/2026 | 06/27/2026 | [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) <br> [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [Amazon Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) |


### Week 10 Achievements:

* Reconfirmed the **DocuFlow AI** architecture direction:
  * Bedrock is no longer a mandatory MVP component because of policy/model access/cost risk in Free Tier or workshop accounts.
  * Textract remains responsible for invoice/receipt data extraction.
  * Normalization, classification, and JSON schema generation now move to an external AI provider behind a dedicated Lambda adapter.

* Updated the processing flow:
  * The user uploads an invoice/receipt to the S3 raw bucket using a presigned URL.
  * The S3 Object Created event flows through EventBridge and SQS.
  * Step Functions orchestrates file validation, Textract, `externalAiNormalize`, JSON validation, result persistence, and status updates.
  * DynamoDB stores metadata/status; S3 processed stores result JSON; CloudWatch and SNS handle logs, alarms, and notifications.

* Redesigned the external AI integration:
  * A Lambda adapter isolates external AI API calls from the main workflow.
  * The API key is stored in AWS Secrets Manager, not in frontend code, source code, or public configuration files.
  * The adapter handles sensitive-data masking when needed, input length limits, timeout, retry, and response normalization into the internal schema.

* Updated the data contract and status model:
  * Kept the shared schema with `documentId`, `userId`, `fileName`, `documentType`, `vendorName`, `invoiceDate`, `currency`, `totalAmount`, `taxAmount`, `lineItems`, `confidenceScore`, `status`, `s3RawPath`, and `s3ProcessedPath`.
  * Added handling for external AI failures such as timeout, quota exceeded, invalid JSON, and schema mismatch.
  * Failures can move to `REVIEW_REQUIRED` when human review is useful, or `FAILED` when the workflow cannot continue.

* Updated the cost analysis:
  * Removed Bedrock from the AWS baseline cost.
  * Kept the main AWS services: Cognito, S3, CloudFront, API Gateway, Lambda, EventBridge, SQS, Step Functions, Textract, DynamoDB, CloudWatch, SNS, Secrets Manager, and SAM/CloudFormation.
  * Added external AI provider cost as a separate item based on request/token usage and outside AWS Free Tier.
  * Added cost controls: demo file limit, page-per-file limit, input/output token limits, budget alerts, and post-workshop cleanup.

* Updated the risk matrix:
  * External AI API key exposure is mitigated with Secrets Manager and least-privilege IAM.
  * Quota/rate-limit risk is mitigated with controlled retry, timeout, and fallback to `REVIEW_REQUIRED`.
  * Sensitive data leaving AWS is mitigated by sending only fields needed for normalization, masking when required, and documenting the boundary in the workshop.
  * Latency/network timeout risk is mitigated with Step Functions retry/catch and explicit statuses.

* Reworked the proposal:
  * Updated the high-level architecture and service selection.
  * Replaced Bedrock with the External AI Lambda Adapter.
  * Updated workflow, cost estimation, risk mitigation, security baseline, and definition of done.
  * Synchronized the bilingual Vietnamese and English content.

* Reworked the workshop:
  * Updated overview and prerequisites for the new architecture.
  * Rewrote the AI extraction module to use Textract + external AI adapter instead of Textract + Bedrock.
  * Added secret configuration, deployment, happy-path test, low-confidence/failure-path test, and cleanup steps.
  * Rechecked the evidence checklist for the final report.
