---
title: "Workshop Overview"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### What You Will Build

In this workshop you build **DocuFlow AI**, a serverless platform that turns uploaded invoices and receipts into structured, searchable data with minimal manual work. An authenticated user uploads a PDF, JPG, or PNG; the platform extracts vendor, date, tax, totals, and line items; and the result is stored, tracked by status, and shown in a web dashboard with a manual review path for low-confidence documents.

The system is fully serverless and event-driven. There are no servers to manage, you pay only for what you process, and the whole platform is defined as a single AWS SAM stack deployed in `ap-southeast-1` (Singapore).

#### Architecture

![DocuFlow AI high-level serverless architecture](/images/2-Proposal/docuflow_high_level_architecture.png)

The platform is organized into layers:

- **Identity & Frontend** - Amazon Cognito authenticates users; a React single-page app is delivered through Amazon CloudFront and deployed with AWS Amplify Hosting.
- **API & Compute** - Amazon API Gateway exposes REST endpoints backed by AWS Lambda functions.
- **Storage** - Amazon S3 holds raw uploads and processed output; Amazon DynamoDB stores document metadata and status.
- **Ingestion & Workflow** - Amazon EventBridge and Amazon SQS decouple upload from processing; AWS Step Functions orchestrates the pipeline.
- **AI Extraction** - Amazon Textract reads the document; an AI Proxy Lambda calls an External AI API to normalize the output into a consistent JSON schema.
- **Observability & Notification** - Amazon CloudWatch and AWS X-Ray provide logs, metrics, alarms, and traces; Amazon SNS triggers Amazon SES email notifications.

#### End-to-end Flow

A single document moves through the platform as follows:

1. The user signs in with Cognito and requests an upload slot.
2. A Lambda returns a short-lived S3 presigned URL and writes an initial DynamoDB item with status `UPLOADED`.
3. The browser uploads the file directly to S3; no document bytes pass through the API.
4. S3 emits an event to EventBridge, which routes it through SQS; status moves to `QUEUED`.
5. Job Starter Lambda starts a Step Functions execution; the workflow validates input, calls Textract, then calls the AI Proxy Lambda for External AI normalization.
6. Confidence + Status Lambda saves metadata to DynamoDB and processed JSON to S3 with status `EXTRACTED`, `REVIEW_REQUIRED`, or `FAILED`.
7. On low confidence or failure, SNS and SES notify reviewers, who can correct fields and move the status to `CORRECTED` or `APPROVED`.

#### Workshop Modules

You build the platform incrementally. Each module adds resources to the same SAM stack.

| Module | Focus | Key services added |
|---|---|---|
| 5.1 | Overview (this page) | — |
| 5.2 | Prerequisites | Tooling, AWS profile, region, budget guardrails |
| 5.3 | Frontend, Auth, Upload | CloudFront, Amplify Hosting, Cognito, API Gateway, presign Lambda |
| 5.4 | Storage, Ingestion, Workflow | S3 buckets, EventBridge, SQS + DLQ, Step Functions |
| 5.5 | AI Extraction | Textract, AI Proxy Lambda, External AI API, confidence/status Lambda |
| 5.6 | Data, Result, Review | DynamoDB, API Gateway, review Lambdas |
| 5.7 | Observability & Security | CloudWatch, X-Ray, SNS/SES, IAM, KMS, Secrets Manager, CloudTrail, Budgets |
| 5.8 | Cleanup | Stack teardown and verification |

#### Who This Is For

This workshop suits a five-person team, where each member can own one or two modules. It assumes basic familiarity with the AWS console, the command line, and JavaScript or TypeScript. Deep prior experience with serverless services is not required, each module explains the services it introduces.

#### Cost and Time

The workshop workload costs about **$3.50 - $8.00 per month** in `ap-southeast-1`, and far less on a new account where the AWS Free Tier covers most services. Plan roughly 10 to 12 weeks for a team building and documenting all modules end to end. Remember to run the cleanup in module 5.8 to avoid ongoing charges.

When you are ready, continue to **5.2 Prerequisites** to set up your environment.
