---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, you need to summarize the contents of the workshop that you **plan** to conduct. -->

# DocuFlow AI - Serverless Intelligent Invoice & Receipt Processing Platform on AWS

{{% notice info %}}
**Implementation recommendation:** lock the MVP around end-to-end invoice and receipt processing using the serverless core architecture.
{{% /notice %}}

### 1. Executive Summary

DocuFlow AI is an intelligent document processing platform on AWS for Finance, Accounting, Operations, and Reviewer/Admin users in small and medium businesses. The platform allows authenticated users to upload invoices and receipts through presigned URLs, automatically extract important information with Amazon Textract, normalize and enrich the result with Amazon Bedrock, store metadata, track processing status, send alerts when processing fails or confidence is low, and let reviewers correct results when needed.

The project targets a real operational pain point: manual invoice and receipt entry is slow, error-prone, hard to audit, and difficult to summarize. The architecture is suitable for an AWS workshop because it has a clear cloud design, multiple managed/serverless AWS services, an end-to-end workflow, logs, metrics, alerts, security baselines, testing, and cleanup.

| Information | Proposed value |
| --- | --- |
| Project name | DocuFlow AI - Serverless Intelligent Invoice & Receipt Processing Platform on AWS |
| Target users | Finance, Accounting, Operations, and Admin/Reviewer users in small and medium businesses |
| Main goal | Automatically ingest, extract, normalize, store, monitor, and analyze invoices and receipts |
| Architecture principle | Serverless, event-driven, scale-to-zero compute, no EC2 in the MVP |
| Team size | 5 people, each owning a clear AWS module |
| Recommendation status | Ready to implement if the MVP scope and data contract are locked before coding |

### 2. Problem Statement

Many businesses still process invoices, receipts, payment slips, and purchase documents through email, shared folders, or manual data entry. When document volume increases near the end of a month or quarter, the process becomes slow, inconsistent, and difficult to audit.

| Pain point | Business impact | How DocuFlow AI addresses it |
| --- | --- | --- |
| Manual data entry | Wrong vendor, invoice date, total amount, tax, or line items | Textract extracts fields; Lambda normalizes JSON and validates schema |
| No processing status | Users do not know whether a file is processing, failed, or waiting for review | DynamoDB status table and frontend polling |
| Scattered documents | Hard to search, audit, and analyze | S3 raw/processed bucket, documentId, and consistent metadata |
| Low quality documents | OCR may miss or misread fields | Confidence score, `REVIEW_REQUIRED`, SNS alert, and manual review loop |
| Hard-to-control operating cost | Always-on servers are wasteful for uneven workloads | Lambda, SQS, Step Functions, and event-driven processing; no EC2 in the MVP |

{{% notice note %}}
**Scope decision:** the MVP only processes invoices and receipts. The implementation flow focuses on login, presigned URL upload, extraction, storage, result/status tracking, alerts, and manual review.
{{% /notice %}}

### 3. Scope, Objectives and Success Criteria

#### 3.1 MVP Scope

- Users sign in with Amazon Cognito and upload PDF, JPG, or PNG files through the frontend.
- The frontend calls Amazon API Gateway and AWS Lambda to create a presigned URL and processing job.
- Files uploaded to the S3 raw bucket trigger EventBridge, SQS, and a Step Functions Standard Workflow.
- Amazon Textract reads invoices and receipts with AnalyzeExpense or AnalyzeDocument/StartExpenseAnalysis depending on document format and implementation approach.
- Amazon Bedrock receives Textract output to normalize field names, classify invoice/receipt data, enrich contextual fields, and return a consistent JSON structure.
- A Lambda validation layer checks schema, business rules, and confidence score.
- DynamoDB stores metadata and status; the S3 processed bucket stores JSON/CSV output.
- CloudWatch Logs, Metrics, Alarms, and Amazon SNS notify reviewers when processing fails or confidence is low.

#### 3.2 Out of Scope for MVP

- Processing every enterprise document type; the MVP only accepts invoice and receipt.
- EC2 for core processing compute.
- Ingestion from external systems. Direct presigned URL upload is enough for the workshop.

#### 3.3 Success Criteria

| Criterion | Required level |
| --- | --- |
| End-to-end demo | Login, upload, processing, extraction, status/result display, alert, and cleanup work successfully |
| Extraction quality | At least vendor, invoice date, total amount, currency, tax, and line items when available |
| Reliability | Retry, DLQ or failure path exists; document status does not remain stuck forever |
| Observability | Logs, metrics, alarms, Step Functions execution history, and test evidence are available |
| Security | No public bucket, no hard-coded access keys, IAM roles follow least privilege |
| Cost control | Sample file limits, lifecycle policy, cleanup script, and basic budget alert are included |

### 4. Solution Architecture

The architecture groups the solution into Frontend & Access, API, Storage & Ingestion, Processing Workflow, Data & Results, Manual Review & Notifications, Observability/Operations, and Security/Governance. Files are uploaded directly to S3 through presigned URLs, then processed asynchronously through EventBridge, SQS, Lambda, Step Functions, Amazon Textract, Amazon Bedrock, and Lambda validation.

![DocuFlow AI high-level serverless architecture](/images/2-Proposal/docuflow_high_level_architecture.png)

```text
[User] -> [Route 53 / CloudFront + WAF] -> [Amplify + Cognito]
       -> [API Gateway] -> [Lambda Business APIs]
       -> [S3 Raw via presigned URL] -> [EventBridge] -> [SQS]
       -> [Lambda Job Starter] -> [Step Functions]
       -> [Textract] -> [Amazon Bedrock] -> [Validation]
       -> [DynamoDB + S3 Processed]
       -> [SNS / Manual Review if needed]
```

### 5. AWS Service Selection and Rationale

| Layer | AWS services | Rationale |
| --- | --- | --- |
| Global Edge & Frontend/Auth | Route 53, CloudFront, AWS WAF, Amplify Hosting, Cognito | Separate frontend from backend, provide CDN/protection, managed login, and a clear UI/API boundary |
| API Layer | API Gateway REST API, Lambda Business APIs | API creates `documentId`, writes initial metadata, and returns a presigned URL so the browser uploads directly to S3 |
| Ingestion & Orchestration | S3 raw, EventBridge, SQS, Lambda Job Starter | S3 stores originals; EventBridge/SQS decouple events from processing; Lambda starts the workflow reliably |
| Processing Workflow | Step Functions, Textract, Amazon Bedrock, Lambda validation | Workflow validates files, calls Textract, uses Bedrock to normalize/enrich/classify output, then validates schema/confidence |
| Data Stores | DynamoDB, S3 raw/processed/archive | DynamoDB stores status/metadata for the frontend; S3 stores raw, processed, and audit data |
| Manual Review & Notifications | Reviewer UI, SNS, Review Update Lambda | Low-confidence items enter the review loop; reviewer corrections update status/result |
| Observability | CloudWatch Metrics/Logs/Alarms | Track executions, Lambda errors, SQS backlog, workflow failures, and operational alerts |
| Security & Governance | IAM, KMS, S3 Block Public Access, bucket policies | Apply least privilege, encryption, and bucket protection for financial documents |

### 6. End-to-End Workflow

1. User signs in with Cognito and the frontend receives a JWT token.
2. User selects an invoice or receipt file; the frontend calls API Gateway for a presigned URL.
3. Lambda creates a `documentId`, writes a DynamoDB item with `UPLOADED` status, and returns the presigned URL.
4. The frontend uploads the file directly to the S3 raw bucket.
5. S3 Object Created event is sent through EventBridge and placed into the SQS processing queue.
6. Lambda Job Starter reads the queue and starts a Step Functions execution with `documentId`, bucket, key, and `userId`.
7. Step Functions validates file type, size, metadata, and updates status to `PROCESSING`.
8. Textract extracts text, key-value fields, summary fields, and line items.
9. Amazon Bedrock receives Textract output, normalizes field names, classifies invoice/receipt data, enriches missing or contextual fields, and returns JSON using the agreed schema.
10. Lambda validates JSON schema, business rules, and `confidenceScore`, then decides whether the document is `EXTRACTED` or `REVIEW_REQUIRED`.
11. Results are stored in DynamoDB and S3 processed; the frontend displays status and result.
12. If processing fails or confidence is low, CloudWatch alarms and SNS send an alert to reviewer/admin.
13. Reviewer corrects fields in the admin review page; update Lambda stores the corrected result and changes status to `CORRECTED` or `APPROVED`.

### 7. Five-Person Module Ownership

| Person | Module | AWS services owned | Required deliverable |
| --- | --- | --- | --- |
| 1 | Frontend, Auth & Upload | Route 53/CloudFront/WAF, Amplify, Cognito, API Gateway | Login, protected route, upload UI, status/result page, presigned URL flow |
| 2 | Ingestion & Workflow | S3 raw/processed, EventBridge, SQS, Lambda, Step Functions | Workflow trigger, retry/DLQ, status transition, failure handling, sample execution |
| 3 | Extraction, Bedrock Enrichment & Validation | Textract, Amazon Bedrock, Lambda | Extraction JSON, invoice/receipt field mapping, Bedrock prompt/schema, confidence score, and validation logic |
| 4 | Data Storage & Dashboard | DynamoDB, S3 processed/archive/reports, frontend summary | Metadata table, processed JSON, status query, and simple frontend dashboard/report |
| 5 | Observability, Security & IaC | CloudWatch, SNS, IAM, KMS, S3 policies, CDK/SAM | Alarms, logs, metrics, SNS notification, least privilege policy, deploy/cleanup script |

{{% notice tip %}}
**Integration rule:** every member owns a separate module, but all modules must share one `documentId`, status model, JSON schema, and naming convention. Do not let each module define its own fields independently.
{{% /notice %}}

### 8. Data Contract and Status Model

The data contract must be finalized before work is split across the team. It is the contract between frontend, workflow, extraction, DynamoDB, summary dashboard, and reviewer UI.

```json
{
  "documentId": "doc-001",
  "userId": "user-123",
  "fileName": "invoice-001.pdf",
  "documentType": "INVOICE",
  "status": "EXTRACTED",
  "vendorName": "ABC Company",
  "invoiceDate": "2026-06-01",
  "currency": "VND",
  "totalAmount": 2500000,
  "taxAmount": 250000,
  "lineItems": [{"description": "Service fee", "amount": 2500000}],
  "confidenceScore": 0.91,
  "s3RawPath": "s3://docuflow-raw/invoice-001.pdf",
  "s3ProcessedPath": "s3://docuflow-processed/doc-001.json",
  "createdAt": "2026-06-08T10:00:00Z",
  "updatedAt": "2026-06-08T10:02:00Z"
}
```

| Status | Meaning | Updated by |
| --- | --- | --- |
| `UPLOADED` | File has been uploaded to S3 raw or a job has been created | Presigned URL Lambda / upload callback |
| `PROCESSING` | Workflow is validating, extracting, or normalizing | Step Functions / Lambda |
| `EXTRACTED` | Extraction succeeded and confidence is above threshold | Schema validation Lambda |
| `REVIEW_REQUIRED` | Required field is missing or confidence is low | Schema validation Lambda |
| `CORRECTED` | Reviewer corrected the result | Admin review page / update Lambda |
| `FAILED` | Workflow failed, file format is invalid, or an external service failed after retry | Step Functions catch handler |

### 9. Security, IAM and Compliance Baseline

- S3 raw and processed buckets enable Block Public Access; the frontend never performs public writes.
- Upload uses short-lived presigned URLs with content type and user/document prefix restrictions.
- Cognito groups separate `EndUser`, `Reviewer`, and `Admin`; API Gateway uses an authorizer.
- Each Lambda function and Step Functions state uses least-privilege IAM roles for specific buckets, tables, and queues.
- Data in S3 and DynamoDB should be encrypted with SSE-S3 or KMS; logs must not store full sensitive invoice or receipt content.
- No access keys are hard-coded in code, repository, or frontend; access uses IAM roles and least-privilege policies.
- CloudFront can be combined with WAF; S3 static hosting should use OAC/OAI when applicable.

### 10. Observability, Notification and Operations

Operations is not a secondary module. It is proof that the project can run, be debugged, and control failures during the workshop.

| Item | Suggested metric/log/alert | Purpose |
| --- | --- | --- |
| Step Functions | `ExecutionStarted`, `ExecutionFailed`, `ExecutionSucceeded`, duration | Audit each document and debug failed states |
| Lambda | Errors, Throttles, Duration, IteratorAge if queue-based | Detect function errors, timeouts, and abnormal retries |
| SQS | ApproximateNumberOfMessagesVisible, DLQ depth | Detect backlog or repeated failing messages |
| Textract + Amazon Bedrock | Request count, error count, latency, token usage, confidence distribution | Control extraction quality and AI cost |
| DynamoDB | Consumed capacity, throttled requests, item status distribution | Monitor metadata/status store |
| SNS | Notification delivery status | Ensure reviewers receive alerts for errors or low confidence |

Recommended alarms:

- Step Functions failed execution > 0 within 5 minutes.
- DLQ visible messages > 0.
- Lambda error rate exceeds the accepted threshold.
- `REVIEW_REQUIRED` rate exceeds 30% in a demo batch.

### 11. Analytics and Reporting

Analytics in the MVP uses DynamoDB, S3 processed JSON, and a simple frontend summary/dashboard.

| Level | Components | Demo output |
| --- | --- | --- |
| MVP | DynamoDB + S3 processed JSON + frontend summary/dashboard | Total documents, total amount, number of errors, top vendor, items requiring review |

### 12. Implementation Roadmap

| Phase | Duration | Goal | Output |
| --- | --- | --- | --- |
| Phase 0 - Design lock | Week 1 | Lock MVP, data contract, service boundaries, naming convention | Architecture diagram, JSON schema, module owner list |
| Phase 1 - Foundation | Week 2-3 | Create IaC base, S3, Cognito, API Gateway, Lambda upload | User login and file upload to S3 raw |
| Phase 2 - Workflow | Week 4-5 | EventBridge, SQS, Step Functions, DynamoDB status | Workflow runs from upload to `PROCESSING`/`FAILED` |
| Phase 3 - Extraction & enrichment | Week 6-7 | Textract + Bedrock enrichment + schema validation | Extracted JSON and confidence score |
| Phase 4 - Result UI & review | Week 8 | Result page, review page, correction flow | User views result; reviewer corrects fields |
| Phase 5 - Observability & MVP dashboard | Week 9-10 | CloudWatch alarms, SNS, DynamoDB/S3 processed summary | Operations dashboard and simple dashboard/summary |
| Phase 6 - Testing & documentation | Week 11 | Test cases, screenshots, workshop steps | Lab guide, expected results, failure test |
| Phase 7 - Final polish | Week 12 | Cleanup, cost review, bilingual report | Final demo and report website |

### 13. Workshop Structure

| Section | Content to write | Evidence required |
| --- | --- | --- |
| 5.1 Introduction | Document processing problem for SME Finance/Ops and high-level architecture | Goals and architecture diagram |
| 5.2 Prerequisite | AWS account, region, IAM permissions, CLI, CDK/SAM, sample invoice PDF/JPG | Environment checklist |
| 5.3 Frontend & Auth | Deploy Amplify/S3 static, Cognito, API Gateway authorizer | Login and protected page screenshot |
| 5.4 Ingestion & Workflow | S3 raw, EventBridge, SQS, Step Functions, DynamoDB status | Execution history and queue/DLQ setup |
| 5.5 AI Extraction | Textract, Bedrock enrichment, schema mapping, confidence calculation | Sample extracted JSON |
| 5.6 Data & Analytics | DynamoDB, S3 processed, frontend summary/dashboard | Metadata/status query, processed JSON, simple dashboard |
| 5.7 Observability & Cleanup | CloudWatch logs, metrics, alarms, SNS, cleanup script | Alarm test and cleanup commands |

### 14. Test Plan and Validation

| Test case | Input | Expected result |
| --- | --- | --- |
| Happy path invoice | Clear invoice PDF | Status `EXTRACTED`; vendor/date/total/currency/line items are available |
| Low quality receipt | Blurry receipt or missing fields | Status `REVIEW_REQUIRED`; SNS alert is sent to reviewer |
| Invalid file type | `.exe` file or oversized file | File is rejected or status is `FAILED`; Textract is not called |
| Unauthenticated upload | User is not signed in | Presigned URL cannot be generated |
| Batch upload | 5-10 files in a row | SQS/Step Functions process jobs without loss; status updates correctly |
| Textract + Bedrock failure | Simulated error, quota issue, or service failure | Retry/catch works; DLQ or `FAILED` path contains logs |
| Analytics query | Processed JSON sample + DynamoDB metadata | Frontend summary/dashboard returns total amount by vendor/month or status |
| Cleanup | Run destroy/delete script | Stack, bucket objects, alarms, and demo data are removed after the workshop |

### 15. Budget Estimation

You can create and update the official estimate with the [AWS Pricing Calculator](https://calculator.aws/#/). The numbers below are a working estimate for the workshop MVP and should be rechecked for the selected AWS Region before submission.

#### Budget Assumptions

- Region: `ap-southeast-1` (Singapore), unless the team chooses another region during implementation.
- Workload for MVP demo: 100 invoice/receipt files per month.
- Average document size: 2 pages per file, or about 200 Textract pages per month.
- Bedrock enrichment: 100 requests/month, about 2,000 input tokens and 500 output tokens per document.
- File storage: up to 1 GB raw documents and processed JSON/CSV output during the workshop.
- Users: 5-10 workshop/demo users authenticated with Cognito.

#### Infrastructure Costs

| AWS service | MVP usage assumption | Estimated monthly cost |
| --- | --- | --- |
| Amazon Textract | AnalyzeExpense for about 200 pages/month | About $2.00 |
| Amazon Bedrock | About 100 enrichment requests/month with a small text model and capped token usage | About $0.10-$1.00 |
| AWS Lambda | Upload API, workflow starter, validation, review update; low request volume | About $0.00-$0.10 |
| Amazon API Gateway | A few thousand REST API calls/month | About $0.01 |
| Amazon S3 | About 1 GB raw + processed data, small request volume | About $0.03-$0.10 |
| Amazon DynamoDB | On-demand metadata/status table, small read/write volume | About $0.05-$0.25 |
| Amazon EventBridge + Amazon SQS | ObjectCreated events and processing queue for demo volume | About $0.00-$0.10 |
| AWS Step Functions Standard | About 100 executions/month, several states per document | About $0.00-$0.10 |
| Amazon CloudWatch | Logs, metrics, and basic alarms for demo workload | About $0.50-$1.00 |
| Amazon SNS | Reviewer/admin alert notifications, low volume | About $0.00-$0.10 |
| Amazon Cognito | 5-10 monthly active users | About $0.00 |
| AWS Amplify Hosting + Amazon CloudFront | Small frontend build/hosting traffic for workshop demo | About $0.50-$2.00 |

**Estimated MVP total:** about **$3.50-$7.00/month**, or **$42.00-$84.00 for 12 months**, depending on selected region, selected Bedrock model, token usage, frontend hosting choice, and log retention.

### 16. Cost Control and Cleanup

#### 16.1 Cost-Control MVP Decision

{{% notice info %}}
**Cost-control decision:** the MVP only processes invoices and receipts. The team only implements the core services needed for login -> upload -> extraction -> storage -> result/status -> alert -> manual review.
{{% /notice %}}

| Category | MVP decision | Cost control note |
| --- | --- | --- |
| Document types | Clear invoice and receipt samples only | Keep sample count small during demo |
| AI services | Textract + Amazon Bedrock + Lambda schema validation | Limit processed pages, Bedrock requests, and token usage |
| Analytics | DynamoDB + S3 processed + simple frontend summary/dashboard | Avoid extra BI services and query only processed demo data |
| Operations | CloudWatch Logs/Metrics/Alarms + SNS | Keep log retention short and alert volume low |

- Limit the demo to 5-10 small invoice/receipt files; do not upload large batches before budget alert is enabled.
- Apply lifecycle policies to S3 raw/processed buckets so demo data is transitioned or deleted after a short period.
- Textract is charged by request/page; Bedrock is charged by model/token usage. Limit request count, page count, and token usage, then document the selected Textract API and Bedrock model.
- Do not run large batches outside demo data; all test files should use a dedicated prefix for easy cleanup.
- MVP dashboard reads directly from DynamoDB/S3 processed through the API and does not require a separate BI layer.
- CDK/SAM must include deploy and destroy commands; S3 bucket objects must be removed before stack destruction.

### 17. Risks and Mitigations

| Risk | Impact | Mitigation |
| --- | --- | --- |
| Scope is too broad | End-to-end demo is not completed | Lock invoice/receipt MVP and avoid adding non-core services |
| Textract misreads unusual formats | Financial data is wrong | Use clear samples, confidence threshold, and manual review loop |
| Bedrock or schema mapping returns invalid JSON | Frontend or analytics fails | Define a strict prompt/schema, validate output in Lambda, retry/fallback |
| Analytics service exceeds MVP | Cost increases and demo is delayed | Use DynamoDB/S3 processed + simple frontend dashboard |
| IAM policies are too broad | Security risk and lower evaluation score | Use separate least-privilege roles for each module and no hard-coded keys |
| Async workflow is hard to demo | Users cannot see results immediately | Use status polling, execution history, and stable sample dataset |
| AI cost exceeds expectation | Budget impact | Limit pages/files, enable budget alert, and clean up after the demo |

### 18. Definition of Done

- Clear architecture diagram with AWS services and data flow.
- Five module owners with measurable deliverables.
- Real upload to S3 raw and real workflow trigger.
- At least one invoice or receipt processed by Textract, Bedrock, and Lambda validation into JSON.
- DynamoDB status table and a frontend/result view or API to inspect results.
- Failure path for invalid file or low confidence producing `REVIEW_REQUIRED`/`FAILED`.
- CloudWatch logs, metric/alarm, and SNS notification.
- Test cases with expected results and screenshot/log evidence.
- Cost controls, cleanup steps, and no expensive resources left after demo.
- Main report/workshop content available in English and Vietnamese when submitting with the FCAJ template.

### 19. References

- [FCAJ project rules](https://hcm-rules.awsfcaj.com/3-project/)
- [FCAJ proposal sample](https://workshop-sample.awsfcaj.com/2-proposal/)
- [FCAJ workshop sample](https://workshop-sample.awsfcaj.com/5-workshop/)
- [Amazon Textract - Analyzing Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html)
- [Amazon Bedrock user guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
- [AWS Step Functions - Choosing workflow type](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html)
- [Amazon S3 EventBridge notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html)
- [Amazon Textract pricing](https://aws.amazon.com/textract/pricing/)
- [Amazon Bedrock pricing](https://aws.amazon.com/bedrock/pricing/)
- [AWS Lambda pricing](https://aws.amazon.com/lambda/pricing/)
- [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)
- [AWS Step Functions pricing](https://aws.amazon.com/step-functions/pricing/)
- [Amazon API Gateway pricing](https://aws.amazon.com/api-gateway/pricing/)
- [Amazon DynamoDB pricing](https://aws.amazon.com/dynamodb/pricing/)
- [Amazon SNS pricing](https://aws.amazon.com/sns/pricing/)
- [AWS Amplify pricing](https://aws.amazon.com/amplify/pricing/)
