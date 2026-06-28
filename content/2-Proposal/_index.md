---
title: "Proposal"
date: 2026-04-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# DocuFlow AI - Serverless Intelligent Invoice & Receipt Processing Platform on AWS

## An AWS Serverless Solution for Intelligent Invoice and Receipt Processing

### 1. Executive Summary

DocuFlow AI is designed for small and medium businesses that need to process invoices and receipts with less manual work. The platform allows authenticated users to upload PDF, JPG, or PNG documents, then uses AWS managed services to extract financial fields, normalize the result, store metadata, and show the processing status in a simple web interface.

The solution uses a serverless, event-driven architecture deployed with AWS SAM in `ap-southeast-1` (Singapore). Users access the frontend through Amazon CloudFront and AWS Amplify, sign in with Amazon Cognito, and upload documents through presigned URLs. Amazon S3 stores the original files, EventBridge and SQS decouple ingestion from processing, Step Functions orchestrates the workflow, Amazon Textract extracts invoice and receipt data, an AI Proxy Lambda calls an External AI API to normalize the extracted result, a Confidence + Status Lambda determines the final status, and DynamoDB stores document status and metadata.

The workshop is scoped for a five-person team. Each member owns one module: frontend/auth/upload, ingestion/workflow, AI extraction and validation, data/result dashboard, and observability/security/IaC.

### 2. Problem Statement

#### What's the Problem?

Many businesses still process invoices and receipts through email, shared folders, spreadsheets, or manual data entry. This creates several issues:

- Finance and operations teams spend repetitive time entering vendor name, invoice date, tax, total amount, and line items.
- Manual entry can introduce wrong values, missing fields, and inconsistent document formats.
- Users cannot easily know whether a document is uploaded, processing, failed, extracted, or waiting for review.
- Documents are hard to search, audit, and summarize when they are spread across inboxes and shared drives.
- Finance teams have limited visibility into spending by vendor, month, document type, or failed processing reason.
- Running always-on servers for an uneven document workload increases cost and operational work.

#### The Solution

DocuFlow AI centralizes document upload and processing on AWS. The frontend provides login, upload, status tracking, result viewing, and manual correction. The backend uses presigned URLs for secure upload to S3, then processes documents asynchronously with EventBridge, SQS, Lambda, and Step Functions.

Amazon Textract extracts invoice and receipt fields. An AI Proxy Lambda sends only the minimized Textract output to the External AI API, maps inconsistent field names into a consistent schema, classifies the document as invoice or receipt, explains missing fields, and returns structured JSON. Lambda validates the schema, confidence score, and business rules before saving the result to DynamoDB and S3 processed storage. The frontend never calls the external AI API directly, and raw PDF or image files are not sent to the external AI provider unless the team explicitly approves that behavior.

#### Benefits and Return on Investment

DocuFlow AI reduces manual data entry and gives teams a repeatable process for handling financial documents. The system creates a searchable metadata store, provides status visibility, and supports a review loop for low-confidence documents. Because the MVP is serverless, the platform can stay low-cost for a workshop workload and scale with document volume when needed.

The estimated AWS cost for the workshop workload is about **$3.50-$8.00 per month**, or **$42-$96 for 12 months** in `ap-southeast-1` (Singapore), depending on Textract pages, External AI API usage, CloudWatch/X-Ray volume, log retention, and alert testing. The project also provides reusable learning value for AWS serverless architecture, AI document processing, asynchronous workflows, secure secret handling, observability, governance, and cleanup.

### 3. Solution Architecture

DocuFlow AI uses a serverless AWS architecture for secure upload, asynchronous processing, AI-assisted extraction, result storage, and operational monitoring. The high-level architecture is shown below:

![DocuFlow AI high-level serverless architecture with External AI API](/images/2-Proposal/docuflow_high_level_architecture.png)

The diagram reflects the approved architecture scope: CloudFront, Amplify, Cognito, API Gateway, Lambda, S3 Raw/Processed buckets, EventBridge, SQS with DLQ, Step Functions Standard Workflow, Textract, AI Proxy Lambda, External AI API, DynamoDB, CloudWatch, X-Ray, SNS/SES, IAM, KMS, Secrets Manager, CloudTrail, Budgets, and SAM.

#### AWS Services Used

The services below are grouped by architectural layer. Each row maps directly to the Budget Estimation in section 6.

**Identity & Security**

- Amazon Cognito: User Pool with groups (end-user, reviewer, admin), used as the Cognito User Pool authorizer for every API Gateway endpoint.
- AWS IAM: Least-privilege role per Lambda function and a dedicated Step Functions execution role; no shared admin role.
- AWS KMS: AWS-managed keys (`aws/s3`, `aws/dynamodb`, `aws/sqs`) for encryption at rest; no customer-managed keys to keep cost at $0.
- AWS Secrets Manager: Stores the External AI API key outside source code and frontend code. Lambda stores only the secret name in an environment variable.
- AWS CloudTrail: Provides audit visibility for AWS account activity and project resource changes.

**Frontend Delivery**

- Amazon CloudFront + AWS Amplify: CloudFront provides the secure, scalable user entry point, while Amplify hosts and deploys the React/Vite single-page app with Git-based CI/CD, managed HTTPS, preview deployments, and optional custom domain support.

**API & Compute**

- Amazon API Gateway (REST): Endpoints `POST /documents/upload-url`, `GET /documents`, and `GET /documents/{id}`, all protected by the Cognito authorizer.
- AWS Lambda: Seven Node.js 20.x functions at 256 MB — `generateUploadUrl`, `jobStarter`, `validateDocument`, `textractExtraction`, `aiProxyNormalization`, `confidenceStatus`, and `statusApi`. Notification logic can be implemented as a dedicated Lambda or as a workflow task depending on the final SAM template.

**Storage**

- Amazon S3: Two buckets — `docuflow-raw` (Block Public Access, SSE-KMS, 60-day lifecycle expiration) and `docuflow-processed` (versioned, transition to STANDARD-IA after 30 days).
- Amazon DynamoDB: On-demand table `Documents` keyed by `documentId`, with a GSI on `userId`+`status` for the user dashboard; stores metadata, status, confidence score, normalized fields, AI provider/model metadata, S3 paths, and error codes.

**Eventing & Workflow**

- Amazon EventBridge: Default bus receives S3 `Object Created` events and routes them to SQS.
- Amazon SQS: Standard queue plus Dead-Letter Queue (max 3 retries) to absorb spikes and isolate poison messages.
- AWS Step Functions (Standard Workflow): A state machine that validates document metadata and file type, runs Textract `AnalyzeExpense`, invokes the AI Proxy Lambda, runs confidence/status logic, saves metadata to DynamoDB, saves processed JSON to S3, and routes low-confidence or failed documents to `REVIEW_REQUIRED` or `FAILED`.

**AI Services**

- Amazon Textract `AnalyzeExpense`: Purpose-built API for invoices and receipts; returns `SUMMARY_FIELDS` and `LINE_ITEM_FIELDS` consumed by the External AI normalization step.
- AI Proxy Lambda + External AI API: The AI Proxy Lambda secures and controls calls to the external AI service. It receives Textract output, retrieves the API key from Secrets Manager, sends only extracted fields/text to the provider, validates the response, and returns normalized JSON to the workflow.

**Observability & Notification**

- Amazon CloudWatch: Logs, Metrics, Alarms, and Logs Insights for Lambda, Step Functions, SQS DLQ, Textract, AI Proxy timeout/rate-limit, low-confidence documents, and cost-related operational signals.
- AWS X-Ray: Trace visibility across API Gateway, Lambda, and workflow components where enabled.
- Amazon SNS + Amazon SES: SNS topics and SES email notifications alert the team when processing fails or confidence is low.

**Operations & Cost**

- AWS SAM: A single SAM template per environment for reproducible deployment and clean teardown through CloudFormation.
- AWS Budgets: Spending alerts at the $5 and $10 monthly thresholds defined in section 6.

#### End-to-end Flow

The happy path for a single invoice or receipt traverses the system as follows:

1. **Open frontend** — User accesses the web app through CloudFront and Amplify.
2. **Sign in** — User authenticates through Cognito and receives a JWT for downstream API calls.
3. **Request upload slot** — Frontend sends an authenticated API request to API Gateway. The `generateUploadUrl` Lambda creates a `documentId`, writes an initial DynamoDB record, and returns a short-lived S3 presigned URL.
4. **Direct upload to S3 Raw Bucket** — Browser uploads the invoice or receipt directly to S3; document bytes do not pass through API Gateway or Lambda.
5. **Queue processing job** — S3 ObjectCreated event is routed through EventBridge into SQS; status moves from `UPLOADED` to `QUEUED`.
6. **Start workflow** — Job Starter Lambda polls SQS and starts a Step Functions Standard Workflow; status moves to `PROCESSING`.
7. **Validate and extract** — Step Functions invokes Validate Lambda, then runs Textract `AnalyzeExpense` to extract invoice/receipt fields.
8. **Normalize through AI Proxy** — AI Proxy Lambda retrieves the External AI API key from Secrets Manager, sends minimized Textract fields to the external AI provider, validates the response, and returns normalized JSON.
9. **Calculate confidence and status** — Confidence + Status Lambda determines whether the document is `EXTRACTED`, `REVIEW_REQUIRED`, or `FAILED`.
10. **Persist result** — Metadata and status are saved to DynamoDB, and processed `result.json` is saved to the S3 Processed Bucket.
11. **Alert and observe** — SNS/SES sends alerts for failed or low-confidence documents. CloudWatch and X-Ray provide logs, metrics, alarms, execution visibility, and traces.
12. **Review and approval** — Frontend displays document list, status, result, and review flow; users can move corrected results to `CORRECTED` and final results to `APPROVED`.

Failure paths are handled by Step Functions Catch branches and SQS retry/DLQ rather than ad-hoc Lambda try/catch. When any state catches an error, the workflow transitions to `MarkFailed` (or `MarkReviewRequired` for low-confidence or schema issues) and the DynamoDB status is updated accordingly. Ingestion failures occurring before Step Functions starts are absorbed by SQS retry and end up in the DLQ.

### 4. Technical Implementation

#### Implementation Phases

The project runs in four short phases. Section 5 breaks each phase into specific weeks.

1. **Design** — Lock the MVP scope, draw the architecture, and define the data contract.
2. **Estimate** — Run AWS Pricing Calculator and confirm the workshop cost ceiling.
3. **Refine** — Tune workflow boundaries, confidence threshold, and log retention to fit the five-person team.
4. **Build, test, deploy** — Implement, test end-to-end, capture evidence, and verify cleanup.

#### Technical Requirements

- Frontend: Single-page web app delivered through CloudFront and deployed by AWS Amplify, with login, upload, document list, status/result pages, review/correction flow, managed HTTPS, and branch-based deployments.
- Authentication: Cognito user pool with three groups — `end-user`, `reviewer`, `admin` — wired into API Gateway as a Cognito authorizer.
- Document Input: PDF, JPG, or PNG invoices and receipts uploaded through 5-minute presigned URLs, with frontend pre-validation on file type, size (≤10 MB), and page count.
- Processing: Step Functions Standard Workflow with Node.js 20.x Lambda tasks at 256 MB, Validate Lambda, Textract `AnalyzeExpense`, AI Proxy Lambda, Confidence + Status Lambda, JSON schema validation, Retry/Catch on each state, and explicit status transitions.
- Status Model: `UPLOADED` → `QUEUED` → `PROCESSING` → `EXTRACTED` / `REVIEW_REQUIRED` / `FAILED` → `CORRECTED` → `APPROVED`.
- Data Model: DynamoDB table `Documents` with PK `documentId` and a GSI on `userId`+`status`. Item attributes: `documentId`, `userId`, `fileName`, `documentType`, `status`, normalized fields, `confidenceScore`, `reviewReasons`, `aiProvider`, `normalizationMethod`, `s3RawPath`, `s3ProcessedPath`, `errorCode`, `createdAt`, and `updatedAt`.
- Observability: CloudWatch Logs (7-day retention), Metrics, and Alarms on Lambda errors, Step Functions failed executions, SQS DLQ depth, Textract errors, AI Proxy timeout/rate-limit count, invalid AI response count, and low-confidence document count; X-Ray traces across API, Lambda, and workflow components where enabled.
- Security: No public S3 buckets (Block Public Access), no hard-coded keys, least-privilege IAM role per Lambda, AWS KMS managed keys for encryption at rest, short-lived presigned URLs, External AI API key stored in Secrets Manager, no raw PDF/image sent to the external AI provider by default, CloudTrail enabled for audit visibility, AWS Budgets for spending governance, and a SAM cleanup script.
- Infrastructure as Code: A single AWS SAM stack per environment provisions every resource above and supports clean teardown.
- Reporting Deliverables: Test cases, screenshots covering each status transition, demo recording, and bilingual workshop instructions.

#### Data Contract and External AI Payload

All modules share one JSON contract so frontend, workflow, storage, API response, and analytics stay consistent:

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
  "confidenceScore": 0.91,
  "reviewReasons": [],
  "aiProvider": "external-ai-api",
  "normalizationMethod": "TEXTRACT_PLUS_AI_PROXY_EXTERNAL_API",
  "s3RawPath": "s3://docuflow-dev-raw-bucket/raw/user-123/doc-001/original.pdf",
  "s3ProcessedPath": "s3://docuflow-dev-processed-bucket/processed/user-123/doc-001/result.json",
  "createdAt": "2026-06-08T10:00:00Z",
  "updatedAt": "2026-06-08T10:01:00Z"
}
```

The External AI API receives a minimized payload only:

```json
{
  "documentId": "doc-001",
  "fileName": "invoice-001.pdf",
  "textractSummaryFields": [
    {
      "type": "VENDOR_NAME",
      "text": "ABC Company",
      "confidence": 0.96
    },
    {
      "type": "TOTAL",
      "text": "2,500,000 VND",
      "confidence": 0.93
    }
  ],
  "textractLineItems": [],
  "rawTextPreview": "Invoice ABC Company total 2,500,000 VND..."
}
```

The payload must not include raw PDF files, raw images, API keys, AWS credentials, full documents when not necessary, or unrelated sensitive information. The AI Proxy Lambda validates that the response is valid JSON, `documentType` is `INVOICE`, `RECEIPT`, or `UNKNOWN`, `totalAmount` is numeric, `invoiceDate` is ISO-8601 when available, `confidenceScore` is between 0 and 1, and low-confidence or missing required fields are represented in `reviewReasons`.

#### Technology Stack

The stack below complements the AWS services in section 3. It is the concrete code-level toolset the team uses to build, test, deploy, and document the platform.

**Frontend**

| Layer | Choice | Notes |
|---|---|---|
| Language | TypeScript 5 | Shared types with backend via a workspace package |
| Framework | React 18 | Single-page app delivered through CloudFront and deployed by Amplify |
| Build tool | Vite | Fast HMR, zero-config TypeScript |
| Auth | `amazon-cognito-identity-js` | Official lightweight Cognito SDK; talks to the User Pool from section 3 |
| API client | `axios` + `@tanstack/react-query` | Axios interceptor injects the Cognito JWT; TanStack Query caches server state |
| UI | shadcn/ui + Tailwind CSS | Component primitives, no vendor lock |
| Forms | React Hook Form + Zod | Zod schemas reused server-side |
| Routing | React Router v6 | |
| Testing | Vitest (unit) | E2E is out of scope for the workshop |

**Backend Lambda**

| Layer | Choice | Notes |
|---|---|---|
| Runtime | Node.js 20.x at 256 MB | As declared in sections 3 and 4 |
| Language | TypeScript 5 | Compiled with esbuild via SAM |
| AWS SDK | AWS SDK v3 modular packages | `client-s3`, `s3-request-presigner`, `client-dynamodb`, `lib-dynamodb`, `client-textract`, `client-secrets-manager`, `client-sns`, `client-sfn` |
| External API client | `fetch` / `undici` with timeout and retry limits | Calls the External AI API only from AI Proxy Lambda |
| Validation | Ajv (JSON Schema) + Zod | Ajv enforces the AI Proxy JSON contract; Zod validates HTTP payloads |
| Observability | AWS Lambda Powertools for TypeScript + X-Ray SDK | Structured logs, custom metrics, tracing |
| Testing | Vitest + `aws-sdk-client-mock` | |

**Infrastructure & DevOps**

| Concern | Choice | Notes |
|---|---|---|
| IaC | AWS SAM | Uses the CloudFormation transform; teardown via `sam delete` or `aws cloudformation delete-stack` per the section 7 contingency |
| Local Lambda | AWS SAM CLI (`sam local invoke`, `sam local start-api`) | |
| External AI API | Configured provider/model through environment variables | API key stored in Secrets Manager; raw files are not sent by default |
| Monorepo | pnpm workspaces | |
| Linting | ESLint + Prettier | One shared config across `apps/*`, `services/*`, `packages/*` |
| Secrets scan | gitleaks pre-commit hook | Blocks accidental key commits |
| CI/CD | Amplify Hosting for frontend; GitHub Actions with OIDC → IAM role for backend | Jobs: `lint-test` on PR, `deploy-dev` on `main`, `deploy-prod` on a `v*` tag |
| Documentation | Hugo + Learn theme (this repo) | Bilingual EN/VI, diagrams in draw.io |

**Repository Layout**

```text
docuflow-ai/
├── frontend/
│   └── src/                  # React + Vite frontend deployed by Amplify
├── backend/
│   ├── functions/
│   │   ├── upload-url/
│   │   ├── job-starter/
│   │   ├── textract-extraction/
│   │   ├── ai-proxy-normalization/
│   │   ├── confidence-status/
│   │   ├── status-api/
│   │   └── notification/
│   └── shared/
│       ├── schema/           # JSON schema and Zod DTOs
│       └── utils/
├── infrastructure/
│   ├── template.yaml         # SAM root template
│   └── parameters/{dev,demo}.json
├── docs/
│   ├── architecture/
│   ├── test-evidence/
│   └── demo-script.md
└── samples/
    ├── invoices/
    └── receipts/
```

The Lambda function folders mirror the responsibilities declared in section 3, and the shared schema package enforces the same data contract across upload, normalization, storage, API response, and analytics modules.

#### Team Ownership

| Member | Role | Main responsibility |
|---|---|---|
| Hoàng Trọng Trà | Leader / Integration Owner | Ingestion, EventBridge, SQS, Job Starter Lambda, Step Functions, integration flow |
| Vũ Duy Tài | AI Owner | Textract, AI Proxy Lambda, External AI API normalization, confidence/status logic |
| Nguyễn Hữu Tịnh | Frontend/Auth Owner | CloudFront, Amplify, Cognito, API Gateway integration, upload/result/review UI |
| Lâm Quang Lộc | Data Owner | DynamoDB, S3 processed JSON, metadata schema, document result management |
| Phạm Tùng Dương | Ops/Security/IaC Owner | IAM, KMS, Secrets Manager, CloudTrail, Budgets, SAM, CloudWatch, X-Ray, SNS/SES |

### 5. Timeline & Milestones

#### Project Timeline

- Week 1: Study FCAJ requirements, lock the MVP scope, define the data contract, and draw the architecture.
- Week 2-3: Build foundation services: Cognito, frontend hosting, API Gateway, Lambda upload API, S3 buckets, and DynamoDB table.
- Week 4-5: Build ingestion and workflow: EventBridge, SQS, Step Functions, retry/catch handling, and status transitions.
- Week 6-7: Build AI extraction and normalization: Textract processing, AI Proxy Lambda, External AI API integration, Secrets Manager secret retrieval, JSON schema validation, confidence/status logic, and result storage.
- Week 8: Build result and review UI: document list, document detail page, extracted-field display, correction flow, approval flow, and status tracking.
- Week 9-10: Add observability, security, governance, and IaC: CloudWatch logs/metrics/alarms, X-Ray tracing, SNS/SES alerts, IAM review, KMS encryption, CloudTrail, External AI API retry limits, AWS Budgets, and SAM stack consolidation per environment.
- Week 11: Run test cases, capture screenshots, prepare workshop instructions, and verify cleanup.
- Week 12: Polish bilingual report content, final demo flow, budget estimate, and submission materials.

### 6. Budget Estimation

The budget targets a workshop-scale workload that a five-person team can run end-to-end while staying close to the AWS Free Tier. The official estimate can be created and updated with the [AWS Pricing Calculator](https://calculator.aws/#/).

#### Assumptions

The estimate uses the following baseline so each cost line is reproducible:

- Region: `ap-southeast-1` (Singapore — closest AWS region to Vietnam, lowest latency for local end users; pricing is moderately higher than `us-east-1` for CloudWatch, DynamoDB, API Gateway, and Step Functions, but still well within a workshop budget).
- Document volume: 100 invoice/receipt files per month, average 2 pages per file (about 200 pages per month).
- External AI API usage: 100 normalization requests per month through AI Proxy Lambda, sending only minimized Textract text/fields and line items. The team sets provider-side usage caps where available and validates actual pricing before running demos.
- API traffic: about 5,000 REST API calls per month across upload-url, list-documents, and document-status endpoints.
- Storage: 1 GB raw documents and 0.5 GB processed JSON/CSV.
- Logs: about 500 MB CloudWatch log ingestion per month with a 7-day retention.
- Users: 5–10 monthly active Cognito users (well within the always-free 50,000 MAU).
- Step Functions: 100 Standard-workflow executions per month, about 9 states per document.

#### Monthly Cost Breakdown

| Service | Usage assumption | Monthly cost (USD) |
|---|---|---|
| Amazon Textract `AnalyzeExpense` | 200 pages | $2.00 |
| External AI API | 100 normalization requests, minimized payload | $0.00 – $2.00 provider-dependent |
| AWS Secrets Manager | 1 external API secret | $0.40 |
| AWS Lambda | ~1,200 invocations, 256 MB, ~500 ms | $0.00 – $0.10 |
| Amazon API Gateway (REST) | ~5,000 calls | $0.02 – $0.03 |
| Amazon S3 | 1.5 GB Standard + small request volume | $0.03 – $0.10 |
| Amazon DynamoDB (on-demand) | 1,000 writes + 5,000 reads, < 1 GB storage | $0.05 – $0.30 |
| Amazon EventBridge + Amazon SQS | ~100 events, low queue volume | $0.00 – $0.10 |
| AWS Step Functions Standard | 100 executions × ~9 states | $0.00 – $0.10 |
| Amazon CloudWatch + AWS X-Ray | ~500 MB logs, 7-day retention, traces, ~5 alarms | $0.60 – $1.40 |
| Amazon SNS/SES | ~100 alerts | $0.00 – $0.10 |
| Amazon Cognito | 5–10 MAU (free tier covers 50k) | $0.00 |
| Amazon CloudFront + AWS Amplify Hosting | static React build, low demo traffic, occasional builds | $0.00 – $1.00 |
| AWS CloudTrail + AWS Budgets | management events and spending alerts | $0.00 |
| **Total** | | **$3.50 – $8.00** |

Annualized: about **$36 – $84 for 12 months**. Numbers vary with region, Textract pages, External AI API provider pricing, retry volume, token/payload size, and CloudWatch retention. AWS SAM/CloudFormation (deployment) and AWS Budgets (cost alerts) are also used and stay at **$0.00** within free usage limits.

#### Free Tier Impact

Free Tier coverage depends on the account creation date and selected account plan. Accounts created before July 15, 2025 can still follow the legacy 12-month Free Tier model, while newer accounts may use the current AWS Free Tier plan and credits. Before running the workshop, the team will verify the actual account's Free Tier status in AWS Billing and keep AWS Budgets alerts at $5 and $10.

For this workload, several services may still be free or near-free at workshop volume, such as Lambda, DynamoDB, SNS, Cognito, CloudFront, and Amplify static hosting. API Gateway, S3 storage, CloudWatch logs/alarms, X-Ray traces, Step Functions, and Amplify build minutes/data transfer must be checked against the active account's Free Tier and pricing page rather than assumed to be free.

Textract and the External AI API are **not** covered by AWS Free Tier assumptions in this proposal, so the team must validate both charges directly and keep the monthly workshop budget at or below **$10**.

#### Scaling Scenario

If the workload grows 10× to 1,000 documents and 2,000 pages per month, Textract scales linearly to roughly $20/month and the External AI API scales according to provider pricing, retry count, and payload size. The other services stay near current levels, putting the AWS-side platform around **$25 – $40/month** before provider-specific AI charges.

#### Cost Controls

- AWS Budgets alerts set at $5 and $10 thresholds with email notification.
- CloudWatch log retention set to 7 days; metric filters limited to essential workflow metrics.
- External AI API requests capped by payload size, retry count, timeout, and provider-side spending or usage limits where available.
- S3 lifecycle policy: expire raw uploads after 60 days, move processed JSON to STANDARD-IA after 30 days.
- Textract sample test files limited to ≤ 5 pages each during development.
- Step Functions per-task timeouts set to 30 seconds to avoid runaway executions.
- Final cleanup runs `aws cloudformation delete-stack` (or `sam delete`) on the backend workshop stack, then deletes the Amplify app/branches from the Amplify console or CLI. Together this removes the Cognito User Pool, S3 buckets, DynamoDB table, Step Functions state machine, API Gateway, Lambda functions, EventBridge rules, SQS queues, SNS topic, IAM roles, CloudWatch log groups, and frontend hosting resources.
- CloudTrail, Budgets, X-Ray, SNS, and SES are kept within narrow workshop usage so they provide governance and evidence without becoming open-ended cost drivers.

### 7. Risk Assessment

#### Risk Matrix

Severity is impact × probability on a 1–3 scale (max 9). Owner refers to the workshop module accountable for the mitigation.

| ID | Risk | Impact | Probability | Severity | Owner |
|---|---|---|---|---|---|
| R-01 | Scope creep beyond invoice/receipt MVP | High (3) | Medium (2) | 6 | Tech lead |
| R-02 | Low Textract accuracy on poor scans | High (3) | Medium (2) | 6 | AI module |
| R-03 | External AI API returns malformed JSON / wrong schema | Medium (2) | Medium (2) | 4 | AI module |
| R-04 | Asynchronous workflow failure (Step Functions / SQS) | High (3) | Low (1) | 3 | Workflow module |
| R-05 | Cost overrun beyond $10/month budget | Medium (2) | Low (1) | 2 | IaC/Ops module |
| R-06 | IAM misconfiguration exposes data | High (3) | Low (1) | 3 | Security module |
| R-07 | External AI API timeout, rate limit, or outage | Medium (2) | Medium (2) | 4 | AI module |
| R-08 | Presigned URL leaked from frontend or logs | High (3) | Low (1) | 3 | Frontend / Security |
| R-09 | Document edge cases (low-DPI scan, encrypted, oversized) | Medium (2) | Medium (2) | 4 | AI module |
| R-10 | Sensitive document data sent unnecessarily to external provider | High (3) | Low (1) | 3 | Security / AI module |
| R-11 | External AI API key leaked or missing | High (3) | Low (1) | 3 | Security / IaC module |

#### Mitigation Strategies

- **R-01 (Scope)**: Lock the MVP to invoice and receipt only; any new document type goes through a change-request review before entering the backlog.
- **R-02 (Extraction)**: Use clear sample documents during dev, set a confidence threshold of 0.7, and route low-confidence results to `REVIEW_REQUIRED` for the human review path.
- **R-03 (External AI JSON)**: Strict prompt/instruction with a JSON-only contract, JSON schema validation in the normalization Lambda, automatic retry once, then mark `REVIEW_REQUIRED` if the structure is still invalid.
- **R-04 (Workflow)**: Step Functions Catch states on each task, SQS Standard queue with DLQ at 3 retries, CloudWatch alarm on DLQ depth, and clear DynamoDB status transitions.
- **R-05 (Cost)**: AWS Budgets alerts at $5 and $10, CloudWatch log retention capped at 7 days, External AI API request/retry caps, provider-side spending limit where available, S3 lifecycle policies, and a Textract sample limit of ≤5 pages per file during development.
- **R-06 (IAM)**: One IAM role per Lambda function, Step Functions execution role separated from task roles, AWS KMS managed keys for storage, S3 Block Public Access on every bucket, and a periodic IAM Access Analyzer review.
- **R-07 (External API availability)**: Retry with exponential backoff on timeout/rate-limit, cap retries, emit CloudWatch metrics, then mark `REVIEW_REQUIRED` or `FAILED` instead of blocking the whole workflow.
- **R-08 (Presigned URL)**: TTL of 5 minutes per URL, scope each URL to a single object key, never log the full URL, and filter URLs out of client-side error reports.
- **R-09 (Edge cases)**: Frontend pre-validates file type, size (≤10 MB), and page count; backend returns a structured error and writes the file to a `quarantine/` S3 prefix for manual triage.
- **R-10 (Sensitive data minimization)**: Send only extracted text/fields and a short raw text preview when necessary; do not send raw PDF/image files, AWS credentials, API keys, or unrelated sensitive fields to the external provider.
- **R-11 (Secret handling)**: Store the External AI API key in Secrets Manager, give only the AI Proxy Lambda permission to read that specific secret, never log the key, and rotate it if exposure is suspected.

#### Contingency Plans

- **R-02 / R-03 demo failure**: Use a prepared sample extraction result to demonstrate the status/result UI while showing the failed Step Functions execution history for transparency.
- **R-04 backlog growth**: Pause uploads, inspect SQS and DLQ, then replay only a small sample batch after fixing the root cause.
- **R-05 cost spike**: Stop test uploads, disable the External AI API key or lower provider usage limits, drop CloudWatch retention to 1 day, empty raw and processed buckets, and run the CloudFormation `delete-stack` script if needed.
- **R-06 IAM exposure**: Rotate any leaked credential, disable the affected role, and re-deploy the stack from a clean commit.
- **R-07 External AI API outage**: Route affected documents to `REVIEW_REQUIRED`, show the Step Functions execution history, and use prepared normalized JSON only for the demo UI if the provider remains unavailable.
- **R-08 URL leak**: Invalidate the user's active credentials, re-issue presigned URLs, and audit Amplify access logs / API Gateway logs for the leaked URL pattern.
- **R-10 / R-11 data or key exposure**: Stop external API calls, rotate the provider key, inspect CloudWatch logs for leakage, and re-enable only after payload minimization and log redaction are verified.

### 8. Expected Outcomes

#### Technical Improvements

DocuFlow AI replaces manual invoice and receipt entry with a serverless document-processing workflow. It provides secure upload, automated Textract extraction, External AI API normalization through AI Proxy Lambda, schema validation, metadata storage, status tracking, notifications, operational logs, audit visibility, and cost governance.

#### Success Criteria

The workshop is considered complete when each of the following has evidence:

- Field-level extraction accuracy ≥ 90% for required summary fields on a curated test set of 20 sample invoices and receipts. Required fields are `vendorName`, `invoiceDate`, `totalAmount`, `taxAmount`, and `currency`; line-item extraction is treated as bonus evidence because it varies more across document formats.
- End-to-end processing latency ≤ 60 seconds at the 95th percentile.
- Workshop monthly AWS cost stays at or below **$10**, verified via AWS Budgets and Cost Explorer.
- Every Lambda function has its own least-privilege IAM role; no role is shared across functions.
- All primary DynamoDB status paths exercised with screenshots: `UPLOADED`, `QUEUED`, `PROCESSING`, `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED`, `CORRECTED`, and `APPROVED`.
- Step Functions runs validate, Textract, AI Proxy, confidence/status, and save-result steps.
- External AI API key is stored in Secrets Manager, not source code, frontend code, logs, or API responses.
- External AI payloads contain minimized Textract output only; raw PDF/image files are not sent to the external provider by default.
- CloudWatch logs/alarms, X-Ray trace evidence, SNS/SES alert evidence, CloudTrail audit evidence, and Budgets screenshot are available.
- The SAM/CloudFormation stack deploys cleanly from a fresh account and tears down without manual cleanup.
- Bilingual report (EN/VI) covers each of the eight workshop modules in `5-Workshop`.

#### Long-term Value

The project creates a reusable foundation for financial document processing on AWS. Future teams can extend the same pattern to more document types, stronger validation rules, richer reporting, and deeper finance-system integration. For FCAJ, the workshop demonstrates practical use of AWS serverless services, AI-assisted document processing, external API security, audit visibility, cost control, observability, and cleanup in one coherent solution.
