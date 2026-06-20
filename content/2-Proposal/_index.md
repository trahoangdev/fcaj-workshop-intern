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

The solution uses a serverless, event-driven architecture deployed via AWS CloudFormation in `ap-southeast-1` (Singapore). Users sign in with Amazon Cognito and upload documents through presigned URLs. Amazon S3 stores the original files, EventBridge and SQS decouple ingestion from processing, Step Functions orchestrates the workflow, Amazon Textract extracts invoice and receipt data, Amazon Bedrock normalizes and enriches the extracted result, Lambda validates the final JSON, and DynamoDB stores document status and metadata.

The workshop is scoped for a five-person team. Each member owns one module: frontend/auth/upload, ingestion/workflow, AI extraction and validation, data/result dashboard, and observability/security/IaC. This makes the project practical for an internship report while still demonstrating a complete AWS serverless system.

### 2. Problem Statement

#### What's the Problem?

Many businesses still process invoices and receipts through email, shared folders, spreadsheets, or manual data entry. This creates several issues:

- Finance and operations teams spend repetitive time entering vendor name, invoice date, tax, total amount, and line items.
- Manual entry can introduce wrong values, missing fields, and inconsistent document formats.
- Users cannot easily know whether a document is uploaded, processing, failed, extracted, or waiting for review.
- Documents are hard to search, audit, and summarize when they are spread across inboxes and shared drives.
- Running always-on servers for an uneven document workload increases cost and operational work.

#### The Solution

DocuFlow AI centralizes document upload and processing on AWS. The frontend provides login, upload, status tracking, result viewing, and manual correction. The backend uses presigned URLs for secure upload to S3, then processes documents asynchronously with EventBridge, SQS, Lambda, and Step Functions.

Amazon Textract extracts invoice and receipt fields. Amazon Bedrock receives the Textract output, maps inconsistent field names into a consistent schema, classifies the document as invoice or receipt, enriches contextual fields, and returns structured JSON. Lambda validates the schema, confidence score, and business rules before saving the result to DynamoDB and S3 processed storage.

#### Benefits and Return on Investment

DocuFlow AI reduces manual data entry and gives teams a repeatable process for handling financial documents. The system creates a searchable metadata store, provides status visibility, and supports a review loop for low-confidence documents. Because the MVP is serverless, the platform can stay low-cost for a workshop workload and scale with document volume when needed.

The estimated AWS cost for the workshop workload is about **$3.00-$6.00 per month**, or **$36-$72 for 12 months** in `ap-southeast-1` (Singapore), depending on Bedrock model, token usage, and log retention. The project also provides reusable learning value for AWS serverless architecture, AI document processing, asynchronous workflows, security, monitoring, and cleanup.

### 3. Solution Architecture

DocuFlow AI uses a serverless AWS architecture for secure upload, asynchronous processing, AI-assisted extraction, result storage, and operational monitoring. The high-level architecture is shown below:

![DocuFlow AI high-level serverless architecture](/images/2-Proposal/docuflow_high_level_architecture.png)

#### AWS Services Used

The services below are grouped by architectural layer. Each row maps directly to the Budget Estimation in section 6.

**Identity & Security**

- Amazon Cognito: User Pool with groups (end-user, reviewer, admin), used as the Cognito User Pool authorizer for every API Gateway endpoint.
- AWS IAM: Least-privilege role per Lambda function and a dedicated Step Functions execution role; no shared admin role.
- AWS KMS: AWS-managed keys (`aws/s3`, `aws/dynamodb`, `aws/sqs`) for encryption at rest; no customer-managed keys to keep cost at $0.

**Frontend Delivery**

- Amazon S3 + Amazon CloudFront: A private S3 bucket holds the static SPA build (HTML/JS/CSS); CloudFront fronts it with HTTPS, an Origin Access Control to S3, optional custom domain, and global edge caching. Builds run in GitHub Actions, not on AWS.

**API & Compute**

- Amazon API Gateway (REST): Endpoints `POST /uploads`, `GET /documents/{id}`, `GET /documents/{id}/result`, and `PUT /documents/{id}/review`, all protected by the Cognito authorizer.
- AWS Lambda: Five Node.js 20.x functions at 256 MB — `presignUpload`, `startProcessing`, `validateExtraction`, `updateStatus`, and `reviewUpdate`.

**Storage**

- Amazon S3: Two buckets — `docuflow-raw` (Block Public Access, SSE-KMS, 60-day lifecycle expiration) and `docuflow-processed` (versioned, transition to STANDARD-IA after 30 days).
- Amazon DynamoDB: On-demand table `Documents` keyed by `documentId`, with a GSI on `userId`+`status` for the user dashboard; stores metadata, status, confidence score, S3 paths, and review state.

**Eventing & Workflow**

- Amazon EventBridge: Default bus receives S3 `Object Created` events and routes them to SQS.
- Amazon SQS: Standard queue plus Dead-Letter Queue (max 3 retries) to absorb spikes and isolate poison messages.
- AWS Step Functions (Standard Workflow): An 8-state machine — `Validate` → `Textract` → `Bedrock` → `ValidateJSON` → `SaveResult` → `UpdateStatus` on the happy path, plus two terminal Catch states `MarkReviewRequired` and `MarkFailed` that update the DynamoDB item to `REVIEW_REQUIRED` or `FAILED` respectively.

**AI Services**

- Amazon Textract `AnalyzeExpense`: Purpose-built API for invoices and receipts; returns `SUMMARY_FIELDS` and `LINE_ITEM_FIELDS` consumed by Bedrock downstream.
- Amazon Bedrock: Small text model (Amazon Nova Lite or Claude 3 Haiku) called with a strict system prompt, capped `maxTokens`, and a JSON-only output contract.

**Observability & Notification**

- Amazon CloudWatch: Logs with 7-day retention, Metrics, Alarms (Lambda error rate, Step Functions failed executions, SQS DLQ depth, Bedrock throttling), and Logs Insights for ad-hoc queries.
- Amazon SNS: Topic `docuflow-alerts` for reviewer/admin email notifications on `FAILED` or `REVIEW_REQUIRED` documents.

**Operations & Cost**

- AWS CloudFormation: A single stack per environment for reproducible deployment and clean teardown.
- AWS Budgets: Email alerts at the $5 and $10 monthly thresholds defined in section 6.

#### End-to-end Flow

The happy path for a single invoice or receipt traverses the system as follows:

1. **Sign in** — User authenticates against Cognito and receives a JWT for downstream API calls.
2. **Request upload slot** — Frontend calls `POST /uploads`. The `presignUpload` Lambda generates a `documentId`, writes a DynamoDB item with status `UPLOADED`, and returns a short-lived S3 presigned URL scoped to `docuflow-raw/{userId}/{documentId}.{ext}`.
3. **Direct upload to S3** — Browser PUTs the file with the presigned URL; no document bytes ever pass through API Gateway or Lambda.
4. **Event ingestion** — S3 emits `Object Created` to EventBridge, which routes it to SQS. The `startProcessing` Lambda consumes the message and starts a Step Functions execution; status moves to `PROCESSING`.
5. **AI extraction** — Step Functions calls Textract `AnalyzeExpense`, then Bedrock for normalization, then the `validateExtraction` Lambda for schema and confidence checks.
6. **Persist result** — The `SaveResult` state writes the structured JSON to `docuflow-processed` and updates the DynamoDB item to `EXTRACTED` or `REVIEW_REQUIRED` depending on confidence and validation outcome.
7. **Notify reviewer** — On `REVIEW_REQUIRED` or `FAILED`, SNS publishes to the `docuflow-alerts` topic and sends an email to subscribers.
8. **Review correction** — Reviewer opens the document and calls `PUT /documents/{id}/review`. The `reviewUpdate` Lambda writes corrected fields and moves status to `REVIEWED`.

Failure paths are handled by Step Functions Catch branches and SQS retry/DLQ rather than ad-hoc Lambda try/catch. When any state catches an error, the workflow transitions to `MarkFailed` (or `MarkReviewRequired` for low-confidence or schema issues) and the DynamoDB status is updated accordingly. Ingestion failures occurring before Step Functions starts are absorbed by SQS retry and end up in the DLQ.

### 4. Technical Implementation

#### Implementation Phases

The project runs in four short phases. Section 5 breaks each phase into specific weeks.

1. **Design** — Lock the MVP scope, draw the architecture, and define the data contract.
2. **Estimate** — Run AWS Pricing Calculator and confirm the workshop cost ceiling.
3. **Refine** — Tune workflow boundaries, confidence threshold, and log retention to fit the five-person team.
4. **Build, test, deploy** — Implement, test end-to-end, capture evidence, and verify cleanup.

#### Technical Requirements

- Frontend: Single-page web app stored in a private Amazon S3 bucket and served through Amazon CloudFront with Origin Access Control, with login, upload, status/result pages, and reviewer correction UI.
- Authentication: Cognito user pool with three groups — `end-user`, `reviewer`, `admin` — wired into API Gateway as a Cognito authorizer.
- Document Input: PDF, JPG, or PNG invoices and receipts uploaded through 5-minute presigned URLs, with frontend pre-validation on file type, size (≤10 MB), and page count.
- Processing: Step Functions Standard Workflow (8 states) with Node.js 20.x Lambda tasks at 256 MB, Textract `AnalyzeExpense`, Bedrock normalization with capped `maxTokens`, JSON schema validation, Retry/Catch on each state, and explicit `REVIEW_REQUIRED` and `FAILED` statuses.
- Data Model: DynamoDB table `Documents` with PK `documentId` and a GSI on `userId`+`status`. Item attributes: `documentId`, `userId`, `fileName`, `documentType`, `status`, extracted fields, `confidenceScore`, `rawS3Key`, `processedS3Key`, `createdAt`, `updatedAt`, and review fields (`reviewerId`, `reviewedAt`).
- Observability: CloudWatch Logs (7-day retention), Metrics, and Alarms on Lambda errors, Step Functions failed executions, SQS DLQ depth, and Bedrock throttling, plus Logs Insights for ad-hoc queries.
- Security: No public S3 buckets (Block Public Access), no hard-coded keys, least-privilege IAM role per Lambda, AWS KMS managed keys for encryption at rest, short-lived presigned URLs, and a CloudFormation teardown script.
- Infrastructure as Code: A single AWS CloudFormation stack per environment provisions every resource above and supports clean teardown.
- Reporting Deliverables: Test cases, screenshots covering each status transition, demo recording, and bilingual workshop instructions.

#### Technology Stack

The stack below complements the AWS services in section 3. It is the concrete code-level toolset the team uses to build, test, deploy, and document the platform.

**Frontend**

| Layer | Choice | Notes |
|---|---|---|
| Language | TypeScript 5 | Shared types with backend via a workspace package |
| Framework | React 18 | Single-page app served via S3 + CloudFront |
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
| AWS SDK | AWS SDK v3 modular packages | `client-s3`, `s3-request-presigner`, `client-dynamodb`, `lib-dynamodb`, `client-textract`, `client-bedrock-runtime`, `client-sns`, `client-sfn` |
| Validation | Ajv (JSON Schema) + Zod | Ajv enforces the Bedrock JSON contract; Zod validates HTTP payloads |
| Observability | AWS Lambda Powertools for TypeScript | Structured logs, custom metrics, tracing |
| Testing | Vitest + `aws-sdk-client-mock` | |

**Infrastructure & DevOps**

| Concern | Choice | Notes |
|---|---|---|
| IaC | AWS SAM (CloudFormation transform) | Compiles to CloudFormation; teardown via `sam delete` or `aws cloudformation delete-stack` per the section 7 contingency |
| Local Lambda | AWS SAM CLI (`sam local invoke`, `sam local start-api`) | |
| Bedrock model | Amazon Nova Lite (primary), Claude 3 Haiku (fallback) | Both GA in `ap-southeast-1`; Nova Lite matches the $0.10–$1.00 line in section 6 |
| Monorepo | pnpm workspaces | |
| Linting | ESLint + Prettier | One shared config across `apps/*`, `services/*`, `packages/*` |
| Secrets scan | gitleaks pre-commit hook | Blocks accidental key commits |
| CI/CD | GitHub Actions with OIDC → IAM role | Jobs: `lint-test` on PR, `deploy-dev` on `main`, `deploy-prod` on a `v*` tag |
| Documentation | Hugo + Learn theme (this repo) | Bilingual EN/VI, diagrams in draw.io |

**Repository Layout**

```text
docuflow-ai/
├── apps/
│   └── web/                  # React + Vite frontend
├── packages/
│   ├── shared-types/         # Zod schemas and DTOs
│   └── shared-config/        # eslint, tsconfig, prettier
├── services/
│   ├── functions/            # 5 Lambda handlers from section 3
│   │   ├── presignUpload/
│   │   ├── startProcessing/
│   │   ├── validateExtraction/
│   │   ├── updateStatus/
│   │   └── reviewUpdate/
│   └── statemachines/        # ASL JSON for the 8-state Step Functions
├── infrastructure/
│   ├── template.yaml         # SAM root
│   └── parameters/{dev,prod}.json
├── samconfig.toml
├── pnpm-workspace.yaml
└── .github/workflows/
```

The five Lambda function folders mirror the names declared in section 3, and the state machine in `services/statemachines/` matches the 8-state workflow under "Eventing & Workflow".

### 5. Timeline & Milestones

#### Project Timeline

- Week 1: Study FCAJ requirements, lock the MVP scope, define the data contract, and draw the architecture.
- Week 2-3: Build foundation services: Cognito, frontend hosting, API Gateway, Lambda upload API, S3 buckets, and DynamoDB table.
- Week 4-5: Build ingestion and workflow: EventBridge, SQS, Step Functions, retry/catch handling, and status transitions.
- Week 6-7: Build AI extraction: Textract processing, Bedrock enrichment, JSON schema validation, confidence score, and result storage.
- Week 8: Build result and review UI: document status page, extracted-field display, reviewer correction flow, and update API.
- Week 9-10: Add observability, security, and IaC: CloudWatch logs/metrics/alarms, SNS alerts, IAM review, KMS encryption, AWS Budgets, and CloudFormation stack consolidation per environment.
- Week 11: Run test cases, capture screenshots, prepare workshop instructions, and verify cleanup.
- Week 12: Polish bilingual report content, final demo flow, budget estimate, and submission materials.

### 6. Budget Estimation

The budget targets a workshop-scale workload that a five-person team can run end-to-end while staying close to the AWS Free Tier. The official estimate can be created and updated with the [AWS Pricing Calculator](https://calculator.aws/#/).

#### Assumptions

The estimate uses the following baseline so each cost line is reproducible:

- Region: `ap-southeast-1` (Singapore — closest AWS region to Vietnam, lowest latency for local end users; pricing is moderately higher than `us-east-1` for CloudWatch, DynamoDB, API Gateway, and Step Functions, but still well within a workshop budget).
- Document volume: 100 invoice/receipt files per month, average 2 pages per file (about 200 pages per month).
- Bedrock usage: 100 enrichment requests per month, capped at about 5,000 input tokens and 2,000 output tokens per request, using a small text model (Amazon Nova Lite or Claude 3 Haiku).
- API traffic: about 5,000 REST API calls per month across upload, status, result, and review endpoints.
- Storage: 1 GB raw documents and 0.5 GB processed JSON/CSV.
- Logs: about 500 MB CloudWatch log ingestion per month with a 7-day retention.
- Users: 5–10 monthly active Cognito users (well within the always-free 50,000 MAU).
- Step Functions: 100 Standard-workflow executions per month, about 8 states per document.

#### Monthly Cost Breakdown

| Service | Usage assumption | Monthly cost (USD) |
|---|---|---|
| Amazon Textract `AnalyzeExpense` | 200 pages | $2.00 |
| Amazon Bedrock (small text model) | 100 requests, ~5k in / 2k out tokens | $0.10 – $1.00 |
| AWS Lambda | ~1,000 invocations, 256 MB, ~500 ms | $0.00 – $0.10 |
| Amazon API Gateway (REST) | ~5,000 calls | $0.02 – $0.03 |
| Amazon S3 | 1.5 GB Standard + small request volume | $0.03 – $0.10 |
| Amazon DynamoDB (on-demand) | 1,000 writes + 5,000 reads, < 1 GB storage | $0.05 – $0.30 |
| Amazon EventBridge + Amazon SQS | ~100 events, low queue volume | $0.00 – $0.10 |
| AWS Step Functions Standard | 100 executions × 8 states | $0.00 – $0.10 |
| Amazon CloudWatch | ~500 MB logs, 7-day retention, ~5 alarms | $0.55 – $1.20 |
| Amazon SNS | ~100 publishes | $0.00 – $0.10 |
| Amazon Cognito | 5–10 MAU (free tier covers 50k) | $0.00 |
| Amazon S3 (static SPA) + Amazon CloudFront | small build + demo traffic | $0.00 – $1.00 |
| **Total** | | **$3.00 – $6.00** |

Annualized: about **$36 – $72 for 12 months**. Numbers vary with region, Bedrock model choice, token volume, and CloudWatch retention. AWS CloudFormation (deployment) and AWS Budgets (cost alerts) are also used and stay at **$0.00** within free usage limits.

#### Free Tier Impact

On a new AWS account (less than 12 months old) most line items above are absorbed by the AWS Free Tier: Lambda (1M requests + 400k GB-s/month always free), API Gateway REST (1M calls/month for 12 months), DynamoDB (25 GB storage + 25 RCU/WCU always free), S3 (5 GB for 12 months), CloudWatch (5 GB log ingestion + 10 metrics/alarms always free), SNS (1M publishes/month), Cognito (50,000 MAU always free), and Amazon CloudFront (1 TB data transfer + 10M HTTPS requests/month always free).

Textract and Bedrock are **not** in the free tier, so a new-account workshop typically pays only **about $2 – $3 per month** out of pocket.

#### Scaling Scenario

If the workload grows 10× to 1,000 documents and 2,000 pages per month, Textract scales linearly to roughly $20/month and Bedrock to about $1 – $10/month. The other services stay near current levels, putting the platform around **$25 – $40/month** at that volume.

#### Cost Controls

- AWS Budgets alerts set at $5 and $10 thresholds with email notification.
- CloudWatch log retention set to 7 days; metric filters limited to essential workflow metrics.
- Bedrock requests capped via `maxTokens` and input length validation in Lambda.
- S3 lifecycle policy: expire raw uploads after 60 days, move processed JSON to STANDARD-IA after 30 days.
- Textract sample test files limited to ≤ 5 pages each during development.
- Step Functions per-task timeouts set to 30 seconds to avoid runaway executions.
- Final cleanup runs `aws cloudformation delete-stack` (or `sam delete`) on the workshop stack, which removes the Cognito User Pool, S3 buckets, DynamoDB table, Step Functions state machine, CloudFront distribution, API Gateway, Lambda functions, EventBridge rules, SQS queues, SNS topic, IAM roles, and CloudWatch log groups in one operation.

### 7. Risk Assessment

#### Risk Matrix

Severity is impact × probability on a 1–3 scale (max 9). Owner refers to the workshop module accountable for the mitigation.

| ID | Risk | Impact | Probability | Severity | Owner |
|---|---|---|---|---|---|
| R-01 | Scope creep beyond invoice/receipt MVP | High (3) | Medium (2) | 6 | Tech lead |
| R-02 | Low Textract accuracy on poor scans | High (3) | Medium (2) | 6 | AI module |
| R-03 | Bedrock returns malformed JSON / wrong schema | Medium (2) | Medium (2) | 4 | AI module |
| R-04 | Asynchronous workflow failure (Step Functions / SQS) | High (3) | Low (1) | 3 | Workflow module |
| R-05 | Cost overrun beyond $10/month budget | Medium (2) | Low (1) | 2 | IaC/Ops module |
| R-06 | IAM misconfiguration exposes data | High (3) | Low (1) | 3 | Security module |
| R-07 | Selected Bedrock model not GA in `ap-southeast-1` | Medium (2) | Low (1) | 2 | AI module |
| R-08 | Presigned URL leaked from frontend or logs | High (3) | Low (1) | 3 | Frontend / Security |
| R-09 | Document edge cases (low-DPI scan, encrypted, oversized) | Medium (2) | Medium (2) | 4 | AI module |

#### Mitigation Strategies

- **R-01 (Scope)**: Lock the MVP to invoice and receipt only; any new document type goes through a change-request review before entering the backlog.
- **R-02 (Extraction)**: Use clear sample documents during dev, set a confidence threshold of 0.7, and route low-confidence results to `REVIEW_REQUIRED` for the human review path.
- **R-03 (Bedrock JSON)**: Strict system prompt with a JSON-only contract, JSON schema validation in `validateExtraction` Lambda, automatic retry once, then mark `REVIEW_REQUIRED` if the structure is still invalid.
- **R-04 (Workflow)**: Step Functions Catch states on each task, SQS Standard queue with DLQ at 3 retries, CloudWatch alarm on DLQ depth, and clear DynamoDB status transitions.
- **R-05 (Cost)**: AWS Budgets alerts at $5 and $10, CloudWatch log retention capped at 7 days, Bedrock `maxTokens` cap, S3 lifecycle policies, and a Textract sample limit of ≤5 pages per file during development.
- **R-06 (IAM)**: One IAM role per Lambda function, Step Functions execution role separated from task roles, AWS KMS managed keys for storage, S3 Block Public Access on every bucket, and a periodic IAM Access Analyzer review.
- **R-07 (Bedrock region)**: Verify the chosen model is GA in `ap-southeast-1` before development; if not, fall back to a cross-region invoke against `us-east-1` for Bedrock only, isolated behind one Lambda with a `BEDROCK_REGION` env var.
- **R-08 (Presigned URL)**: TTL of 5 minutes per URL, scope each URL to a single object key, never log the full URL, and filter URLs out of client-side error reports.
- **R-09 (Edge cases)**: Frontend pre-validates file type, size (≤10 MB), and page count; backend returns a structured error and writes the file to a `quarantine/` S3 prefix for manual triage.

#### Contingency Plans

- **R-02 / R-03 demo failure**: Use a prepared sample extraction result to demonstrate the status/result UI while showing the failed Step Functions execution history for transparency.
- **R-04 backlog growth**: Pause uploads, inspect SQS and DLQ, then replay only a small sample batch after fixing the root cause.
- **R-05 cost spike**: Stop test uploads, drop CloudWatch retention to 1 day, empty raw and processed buckets, and run the CloudFormation `delete-stack` script if needed.
- **R-06 IAM exposure**: Rotate any leaked credential, disable the affected role, and re-deploy the stack from a clean commit.
- **R-07 Bedrock outage**: Switch the Lambda env var `BEDROCK_REGION` to a fallback region for the duration of the demo.
- **R-08 URL leak**: Invalidate the user's active credentials, re-issue presigned URLs, and audit CloudFront / API Gateway logs for the leaked URL pattern.

### 8. Expected Outcomes

#### Technical Improvements

DocuFlow AI replaces manual invoice and receipt entry with a serverless document-processing workflow. It provides secure upload, automated Textract extraction, Bedrock normalization and enrichment, schema validation, metadata storage, status tracking, review handling, notifications, and operational logs.

#### Success Criteria

The workshop is considered complete when each of the following has evidence:

- Extraction accuracy ≥ 90% on a curated test set of 20 sample invoices and receipts.
- End-to-end processing latency ≤ 60 seconds at the 95th percentile.
- Workshop monthly AWS cost stays at or below **$10**, verified via AWS Budgets and Cost Explorer.
- Every Lambda function has its own least-privilege IAM role; no role is shared across functions.
- All four DynamoDB terminal status paths exercised with screenshots: `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED` (via Step Functions Catch or SQS DLQ), and `REVIEWED`.
- The CloudFormation stack deploys cleanly from a fresh account and tears down without manual cleanup.
- Bilingual report (EN/VI) covers each of the eight workshop modules in `5-Workshop`.

#### Long-term Value

The project creates a reusable foundation for financial document processing on AWS. Future teams can extend the same pattern to more document types, stronger validation rules, richer reporting, and deeper finance-system integration. For FCAJ, the workshop demonstrates practical use of AWS serverless services, AI document processing, cost control, security, observability, and cleanup in one coherent solution.
