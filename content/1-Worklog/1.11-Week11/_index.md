---
title: "Week 11 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 11 Objectives:

* Finalize the admin-approved **DocuFlow AI** architecture: Textract + AI Proxy Lambda + External AI API, EventBridge + SQS + Step Functions, and complete observability/security/governance.
* Update the five-person work assignment matrix so every service in the architecture has an owner, deliverable, evidence, and cleanup responsibility.
* Complete the implementation documentation: data contract, status model, API contract, workflow, RACI, timeline, test plan, demo script, and Definition of Done.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Review the admin-approved architecture <br>&emsp; + Confirm the main flow from frontend to document-processing workflow <br>&emsp; + Check services still included in the diagram: CloudFront/Amplify, Cognito, API Gateway, Lambda, S3, EventBridge, SQS/DLQ, Step Functions, Textract, AI Proxy, DynamoDB, CloudWatch/X-Ray, SNS/SES, IAM/KMS/Secrets Manager/CloudTrail/Budgets/SAM <br>&emsp; + Remove services that are no longer part of the current architecture | 06/29/2026 | 06/29/2026 | [AWS Architecture Center](https://aws.amazon.com/architecture/) <br> [AWS Serverless](https://aws.amazon.com/serverless/) |
| 3 | - Update the assignment matrix and service ownership map <br>&emsp; + Split the work into five modules: Integration/Workflow, AI, Frontend/Auth, Data, Ops/Security/IaC <br>&emsp; + Assign an owner for each service/component <br>&emsp; + Define deliverables, evidence, and cleanup responsibility for each member | 06/30/2026 | 06/30/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) |
| 4 | - Lock the data contract, status model, and API contract <br>&emsp; + Standardize the JSON schema for document results <br>&emsp; + Finalize statuses: `UPLOADED`, `QUEUED`, `PROCESSING`, `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED`, `CORRECTED`, `APPROVED` <br>&emsp; + Write API contracts for upload-url, list documents, get document detail, and review update | 07/01/2026 | 07/01/2026 | [API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) <br> [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) <br> [S3 Object Key Naming](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html) |
| 5 | - Design the end-to-end processing workflow <br>&emsp; + Document the S3 ObjectCreated -> EventBridge -> SQS/DLQ -> Job Starter Lambda -> Step Functions flow <br>&emsp; + Finalize the state machine: validate, Textract, AI Proxy, validate JSON, confidence/status, save result, alert <br>&emsp; + Add retry/catch behavior for Textract, external AI timeout/rate limit, invalid JSON, and DynamoDB/S3 write failure | 07/02/2026 | 07/02/2026 | [S3 EventBridge Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html) <br> [Amazon SQS DLQ](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) <br> [Step Functions Error Handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html) |
| 6 | - Update security, observability, governance, and cost control <br>&emsp; + Design least-privilege IAM, KMS encryption baseline, and Secrets Manager for the external AI API key <br>&emsp; + Finalize CloudWatch logs/alarms, X-Ray tracing, SNS/SES alerts, CloudTrail audit, and AWS Budgets <br>&emsp; + Write AWS SAM deploy/cleanup checklist | 07/03/2026 | 07/03/2026 | [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) <br> [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) <br> [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) |
| 7 | - Complete the test plan, demo script, and Definition of Done <br>&emsp; + Write test cases: login, clear invoice upload, low-quality receipt, invalid file type, external AI timeout/rate limit, invalid JSON, DLQ path, and cleanup <br>&emsp; + Prepare a 7-10 minute demo script <br>&emsp; + Finalize the evidence checklist for each member | 07/04/2026 | 07/04/2026 | [Amazon Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) <br> [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |


### Week 11 Achievements:

* Finalized the admin-approved **DocuFlow AI** architecture:
  * Users access the frontend through CloudFront/Amplify.
  * Cognito handles login, JWTs, and protected routes.
  * API Gateway + Lambda Presigned URL creates the `documentId`, initial metadata, and upload URL.
  * S3 Raw stores original files; EventBridge routes ObjectCreated events to SQS; DLQ stores repeatedly failed messages.
  * Job Starter Lambda starts a Step Functions Standard Workflow.
  * The workflow calls Validate Lambda, Textract AnalyzeExpense, AI Proxy Lambda, External AI API, and Confidence/Status Lambda.
  * DynamoDB stores metadata/status/job state; S3 Processed stores `result.json`.
  * CloudWatch/X-Ray/SNS/SES provide logs, tracing, alarms, and notifications.
  * IAM/KMS/Secrets Manager/CloudTrail/Budgets/SAM cover security, governance, cost control, and deployment.

* Updated the five-person assignment matrix:
  * **Hoang Trong Tra** - Leader / Integration Owner: S3 raw/processed, EventBridge, SQS/DLQ, Job Starter Lambda, Step Functions, Validate Lambda, and integration tests.
  * **Vu Duy Tai** - AI Owner: Textract AnalyzeExpense, AI Proxy Lambda, External AI API normalization, confidence/status logic, and JSON schema validation.
  * **Nguyen Huu Tinh** - Frontend/Auth Owner: CloudFront/Amplify, Cognito, API Gateway integration, upload UI, document list/detail, and review UI.
  * **Lam Quang Loc** - Data Owner: DynamoDB, S3 processed JSON, metadata schema, result management, review updates, and lightweight reports.
  * **Pham Tung Duong** - Ops/Security/IaC Owner: IAM, KMS, Secrets Manager, CloudTrail, Budgets, SAM, CloudWatch, X-Ray, SNS/SES, and cleanup.

* Locked shared responsibilities:
  * The team will not add or remove services outside the admin-approved architecture unless the change is agreed again.
  * The data contract, status model, API contract, and S3 key naming must be shared across frontend, workflow, AI, and data modules.
  * The external AI API key stays only in Secrets Manager; the frontend never calls external AI directly; AI Proxy Lambda is the only HTTPS caller to the external API.
  * Logs must include `documentId`, `userId`, `status`, `stateName`, and `errorType`, but must not log secrets or sensitive payloads.

* Completed the data contract and status model:
  * Data contract includes `documentId`, `userId`, `fileName`, `documentType`, `status`, `vendorName`, `invoiceDate`, `currency`, `totalAmount`, `taxAmount`, `confidenceScore`, `reviewReasons`, `aiProvider`, `normalizationMethod`, `s3RawPath`, `s3ProcessedPath`, `createdAt`, and `updatedAt`.
  * Status flow includes `UPLOADED`, `QUEUED`, `PROCESSING`, `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED`, `CORRECTED`, and `APPROVED`.
  * S3 key naming is locked as `raw/{userId}/{documentId}/original` and `processed/{userId}/{documentId}/result.json`.

* Completed the initial API contract:
  * `POST /documents/upload-url` creates a presigned URL.
  * `GET /documents` lists documents by user/status.
  * `GET /documents/{documentId}` returns status and extracted fields.
  * `POST /documents/{documentId}/review` corrects or approves reviewed results.

* Finalized the Step Functions workflow:
  * `ValidateInput` -> `UpdateStatusProcessing` -> `RunTextractAnalyzeExpense` -> `ParseTextractOutput` -> `CallAIProxyLambda` -> `ValidateNormalizedJson` -> `CalculateConfidenceAndStatus` -> `SaveMetadataToDynamoDB` -> `SaveProcessedJsonToS3` -> `PublishSNSAlert` when needed.
  * The catch path updates the status to `FAILED`, sends an alert, and keeps log/evidence for debugging.
  * Main handled errors include invalid file type/size, Textract throttling/error, external AI timeout/rate limit/invalid API key, invalid JSON/missing fields, DynamoDB/S3 write failure, and SNS/SES issues.

* Updated the test plan and demo script:
  * Happy path with a clear invoice: successful upload, workflow execution, `EXTRACTED` status, and DynamoDB/S3 Processed result.
  * Low-quality or missing-field receipt: `REVIEW_REQUIRED` status and reviewer/admin alert.
  * Invalid file type: reject or move to `FAILED`.
  * External AI timeout/rate limit and invalid JSON: retry/catch works and logs contain a clear `errorType`.
  * DLQ path and cleanup are validated with SAM delete.

* Locked the Definition of Done:
  * At least 3 sample invoices/receipts can be uploaded from the frontend after login.
  * EventBridge, SQS/DLQ, and Step Functions work for both happy path and failure path.
  * Textract extracts key fields; AI Proxy Lambda calls external AI and returns valid JSON.
  * DynamoDB and S3 Processed store data using the agreed schema/path convention.
  * Frontend shows list/detail/result/review flows.
  * CloudWatch logs, alarms, SNS/SES alerts, Secrets Manager, IAM, CloudTrail, Budgets, and cleanup all have evidence.
