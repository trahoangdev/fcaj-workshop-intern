---
title: "Bản đề xuất"
date: 2026-04-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# DocuFlow AI - Nền tảng xử lý invoice & receipt thông minh bằng AWS Serverless

## Giải pháp AWS Serverless cho xử lý invoice và receipt thông minh

### 1. Tóm tắt điều hành

DocuFlow AI được thiết kế cho các doanh nghiệp vừa và nhỏ cần xử lý invoice và receipt với ít thao tác thủ công hơn. Nền tảng cho phép người dùng đã đăng nhập upload tài liệu PDF, JPG hoặc PNG, sau đó dùng các dịch vụ managed của AWS để trích xuất dữ liệu tài chính, chuẩn hóa kết quả, lưu metadata và hiển thị trạng thái xử lý trên giao diện web đơn giản.

Giải pháp sử dụng kiến trúc serverless, event-driven, deploy bằng AWS SAM tại region `ap-southeast-1` (Singapore). Người dùng truy cập frontend qua Amazon CloudFront và AWS Amplify, đăng nhập bằng Amazon Cognito, và upload tài liệu qua presigned URL. Amazon S3 lưu file gốc, EventBridge và SQS tách lớp ingestion khỏi processing, Step Functions điều phối workflow, Amazon Textract trích xuất dữ liệu invoice/receipt, AI Proxy Lambda gọi External AI API để chuẩn hóa kết quả trích xuất, Confidence + Status Lambda quyết định trạng thái cuối cùng, và DynamoDB lưu trạng thái cùng metadata của tài liệu.

Workshop được giới hạn cho nhóm 5 người. Mỗi người phụ trách một module: frontend/auth/upload, ingestion/workflow, AI extraction và validation, data/result dashboard, observability/security/IaC.

### 2. Tuyên bố vấn đề

#### Vấn đề là gì?

Nhiều doanh nghiệp vẫn xử lý invoice và receipt qua email, folder chia sẻ, spreadsheet hoặc nhập liệu thủ công. Cách làm này tạo ra nhiều vấn đề:

- Đội Finance và Operations mất thời gian lặp lại để nhập vendor name, invoice date, tax, total amount và line items.
- Nhập liệu thủ công dễ gây sai giá trị, thiếu field và format dữ liệu không nhất quán.
- Người dùng khó biết tài liệu đang uploaded, processing, failed, extracted hay waiting for review.
- Tài liệu khó tìm kiếm, kiểm toán và tổng hợp khi bị phân tán qua email hoặc shared drive.
- Đội Finance thiếu góc nhìn tổng hợp theo vendor, tháng, loại chứng từ hoặc lý do xử lý thất bại.
- Chạy server thường trực cho workload tài liệu không đều làm tăng chi phí và công vận hành.

#### Giải pháp

DocuFlow AI tập trung upload và xử lý tài liệu trên AWS. Frontend cung cấp login, upload, theo dõi trạng thái, xem kết quả và chỉnh sửa thủ công khi cần. Backend dùng presigned URL để upload an toàn lên S3, sau đó xử lý bất đồng bộ bằng EventBridge, SQS, Lambda và Step Functions.

Amazon Textract trích xuất field từ invoice và receipt. AI Proxy Lambda chỉ gửi payload Textract đã được rút gọn sang External AI API, map các field name không nhất quán về một schema thống nhất, phân loại tài liệu là invoice hoặc receipt, giải thích field thiếu và trả structured JSON. Lambda validate schema, confidence score và business rules trước khi lưu kết quả vào DynamoDB và S3 processed storage. Frontend không bao giờ gọi external AI API trực tiếp, và raw PDF/image không được gửi sang external AI provider trừ khi team phê duyệt rõ hành vi đó.

#### Lợi ích và giá trị đầu tư

DocuFlow AI giảm công nhập liệu thủ công và tạo quy trình lặp lại được cho xử lý chứng từ tài chính. Hệ thống tạo metadata store có thể tìm kiếm, hiển thị trạng thái rõ ràng và hỗ trợ review loop cho tài liệu có confidence thấp. Vì MVP dùng serverless, nền tảng giữ chi phí thấp cho workload workshop và có thể mở rộng theo số lượng tài liệu khi cần.

Chi phí AWS ước tính cho workload workshop khoảng **$3.50-$8.00 mỗi tháng**, tương đương **$42-$96 cho 12 tháng** tại region `ap-southeast-1` (Singapore), tùy số trang Textract, usage của External AI API, CloudWatch/X-Ray volume, log retention và số lần test alert. Đề tài cũng có giá trị học tập rõ ràng về AWS serverless architecture, AI document processing, asynchronous workflow, secure secret handling, observability, governance và cleanup.

### 3. Kiến trúc giải pháp

DocuFlow AI dùng kiến trúc AWS serverless cho secure upload, asynchronous processing, AI-assisted extraction, result storage và operational monitoring. Kiến trúc tổng quan như sau:

![Kiến trúc high-level của DocuFlow AI với External AI API](/images/2-Proposal/docuflow_high_level_architecture.png)

Diagram thể hiện approved architecture scope: CloudFront, Amplify, Cognito, API Gateway, Lambda, S3 Raw/Processed buckets, EventBridge, SQS with DLQ, Step Functions Standard Workflow, Textract, AI Proxy Lambda, External AI API, DynamoDB, CloudWatch, X-Ray, SNS/SES, IAM, KMS, Secrets Manager, CloudTrail, Budgets và SAM.

#### Dịch vụ AWS sử dụng

Các dịch vụ dưới đây được nhóm theo layer kiến trúc. Mỗi dòng map trực tiếp với bảng Ước tính ngân sách ở Section 6.

**Identity & Security**

- Amazon Cognito: User Pool có groups (end-user, reviewer, admin), dùng làm Cognito User Pool authorizer cho mọi endpoint API Gateway.
- AWS IAM: Role least-privilege riêng cho từng Lambda function và một execution role riêng cho Step Functions; không dùng role admin chia sẻ.
- AWS KMS: AWS-managed keys (`aws/s3`, `aws/dynamodb`, `aws/sqs`) cho encryption at rest; không dùng customer-managed key để giữ chi phí $0.
- AWS Secrets Manager: Lưu External AI API key bên ngoài source code và frontend code. Lambda chỉ lưu secret name trong environment variable.
- AWS CloudTrail: Cung cấp audit visibility cho AWS account activity và các thay đổi resource của project.

**Frontend Delivery**

- Amazon CloudFront + AWS Amplify: CloudFront cung cấp secure/scalable entry point cho người dùng, còn Amplify host và deploy React/Vite single-page app với Git-based CI/CD, managed HTTPS, preview deployments và custom domain tùy chọn.

**API & Compute**

- Amazon API Gateway (REST): Các endpoint `POST /documents/upload-url`, `GET /documents`, `GET /documents/{id}`, đều được Cognito authorizer bảo vệ.
- AWS Lambda: Bảy function Node.js 20.x ở mức 256 MB — `generateUploadUrl`, `jobStarter`, `validateDocument`, `textractExtraction`, `aiProxyNormalization`, `confidenceStatus`, `statusApi`. Notification logic có thể tách thành Lambda riêng hoặc là task trong workflow tùy SAM template cuối cùng.

**Storage**

- Amazon S3: Hai bucket — `docuflow-raw` (Block Public Access, SSE-KMS, lifecycle expire sau 60 ngày) và `docuflow-processed` (bật versioning, chuyển STANDARD-IA sau 30 ngày).
- Amazon DynamoDB: Bảng on-demand `Documents` với PK `documentId` và GSI trên `userId`+`status` phục vụ dashboard người dùng; lưu metadata, status, confidence score, normalized fields, AI provider/model metadata, S3 paths và error codes.

**Eventing & Workflow**

- Amazon EventBridge: Default bus nhận S3 `Object Created` events và route sang SQS.
- Amazon SQS: Standard queue + Dead-Letter Queue (tối đa 3 retry) để đệm spike và cô lập poison message.
- AWS Step Functions (Standard Workflow): State machine validate document metadata và file type, chạy Textract `AnalyzeExpense`, gọi AI Proxy Lambda, chạy confidence/status logic, lưu metadata vào DynamoDB, lưu processed JSON vào S3, và route tài liệu confidence thấp hoặc lỗi sang `REVIEW_REQUIRED` hoặc `FAILED`.

**AI Services**

- Amazon Textract `AnalyzeExpense`: API chuyên cho invoice và receipt; trả về `SUMMARY_FIELDS` và `LINE_ITEM_FIELDS` để bước External AI normalization xử lý tiếp.
- AI Proxy Lambda + External AI API: AI Proxy Lambda bảo vệ và kiểm soát call sang external AI service. Lambda nhận output Textract, đọc API key từ Secrets Manager, chỉ gửi extracted fields/text sang provider, validate response, và trả normalized JSON về workflow.

**Observability & Notification**

- Amazon CloudWatch: Logs, Metrics, Alarms và Logs Insights cho Lambda, Step Functions, SQS DLQ, Textract, AI Proxy timeout/rate-limit, low-confidence documents và cost-related operational signals.
- AWS X-Ray: Trace visibility qua API Gateway, Lambda và workflow components khi được enable.
- Amazon SNS + Amazon SES: SNS topics và SES email notifications cảnh báo team khi processing fail hoặc confidence thấp.

**Operations & Cost**

- AWS SAM: Một SAM template duy nhất cho mỗi environment để deploy có thể tái lập và teardown sạch thông qua CloudFormation.
- AWS Budgets: Email alert ở ngưỡng $5 và $10 hàng tháng đã định nghĩa ở Section 6.

#### Luồng xử lý end-to-end

Happy path cho một invoice hoặc receipt đi qua hệ thống như sau:

1. **Mở frontend** — Người dùng truy cập web app qua CloudFront và Amplify.
2. **Đăng nhập** — Người dùng xác thực qua Cognito và nhận JWT cho các API call sau.
3. **Xin upload slot** — Frontend gửi authenticated API request tới API Gateway. Lambda `generateUploadUrl` sinh `documentId`, ghi DynamoDB record ban đầu, và trả presigned URL S3 ngắn hạn.
4. **Upload trực tiếp lên S3 Raw Bucket** — Browser upload invoice hoặc receipt trực tiếp lên S3; document bytes không đi qua API Gateway hay Lambda.
5. **Queue processing job** — S3 ObjectCreated event được route qua EventBridge vào SQS; status chuyển từ `UPLOADED` sang `QUEUED`.
6. **Start workflow** — Job Starter Lambda poll SQS và start Step Functions Standard Workflow; status chuyển sang `PROCESSING`.
7. **Validate và extract** — Step Functions gọi Validate Lambda, sau đó chạy Textract `AnalyzeExpense` để trích xuất invoice/receipt fields.
8. **Normalize qua AI Proxy** — AI Proxy Lambda đọc External AI API key từ Secrets Manager, gửi minimized Textract fields sang external AI provider, validate response, và trả normalized JSON.
9. **Tính confidence và status** — Confidence + Status Lambda quyết định tài liệu là `EXTRACTED`, `REVIEW_REQUIRED` hay `FAILED`.
10. **Lưu kết quả** — Metadata và status được lưu vào DynamoDB, processed `result.json` được lưu vào S3 Processed Bucket.
11. **Alert và observe** — SNS/SES gửi alert cho tài liệu fail hoặc confidence thấp. CloudWatch và X-Ray cung cấp logs, metrics, alarms, execution visibility và traces.
12. **Review và approve** — Frontend hiển thị document list, status, result và review flow; user có thể chuyển kết quả đã sửa sang `CORRECTED` và kết quả cuối sang `APPROVED`.

Các đường lỗi được xử lý qua Catch branches của Step Functions và retry/DLQ của SQS, không dùng try/catch rải rác trong Lambda. Khi state bất kỳ catch lỗi, workflow chuyển sang `MarkFailed` (hoặc `MarkReviewRequired` cho trường hợp confidence thấp / schema lỗi) và DynamoDB status được cập nhật tương ứng. Các lỗi ingestion xảy ra trước khi Step Functions chạy được hấp thụ bởi SQS retry và rơi vào DLQ.

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án chạy theo bốn giai đoạn ngắn. Section 5 chia mỗi giai đoạn thành các tuần cụ thể.

1. **Thiết kế** — Khóa MVP scope, vẽ kiến trúc và định nghĩa data contract.
2. **Ước tính** — Chạy AWS Pricing Calculator và chốt mức chi phí workshop.
3. **Tinh chỉnh** — Điều chỉnh workflow boundaries, confidence threshold và log retention cho nhóm 5 người.
4. **Build, test, deploy** — Triển khai, test end-to-end, thu thập evidence và verify cleanup.

#### Yêu cầu kỹ thuật

- Frontend: Single-page web app delivered qua CloudFront và deploy bằng AWS Amplify, có login, upload, document list, status/result pages, review/correction flow, managed HTTPS và branch-based deployments.
- Authentication: Cognito user pool với ba groups — `end-user`, `reviewer`, `admin` — gắn vào API Gateway dưới dạng Cognito authorizer.
- Document Input: Invoice/receipt dạng PDF, JPG hoặc PNG được upload qua presigned URL TTL 5 phút, với pre-validate ở frontend cho file type, size (≤10 MB) và page count.
- Processing: Step Functions Standard Workflow với Lambda task chạy Node.js 20.x ở mức 256 MB, Validate Lambda, Textract `AnalyzeExpense`, AI Proxy Lambda, Confidence + Status Lambda, JSON schema validation, Retry/Catch trên mỗi state, và status transitions rõ ràng.
- Status Model: `UPLOADED` → `QUEUED` → `PROCESSING` → `EXTRACTED` / `REVIEW_REQUIRED` / `FAILED` → `CORRECTED` → `APPROVED`.
- Data Model: DynamoDB table `Documents` với PK `documentId` và GSI trên `userId`+`status`. Item attributes: `documentId`, `userId`, `fileName`, `documentType`, `status`, normalized fields, `confidenceScore`, `reviewReasons`, `aiProvider`, `normalizationMethod`, `s3RawPath`, `s3ProcessedPath`, `errorCode`, `createdAt`, `updatedAt`.
- Observability: CloudWatch Logs (retention 7 ngày), Metrics và Alarms trên Lambda errors, Step Functions failed executions, SQS DLQ depth, Textract errors, AI Proxy timeout/rate-limit count, invalid AI response count và low-confidence document count; X-Ray traces qua API, Lambda và workflow components khi được enable.
- Security: Không public S3 bucket (Block Public Access), không hard-code key, IAM role theo least privilege cho mỗi Lambda, AWS KMS managed keys cho encryption at rest, presigned URL TTL ngắn, External AI API key lưu trong Secrets Manager, mặc định không gửi raw PDF/image sang external AI provider, CloudTrail enabled cho audit visibility, AWS Budgets cho spending governance, và có SAM cleanup script.
- Infrastructure as Code: Một AWS SAM stack duy nhất cho mỗi environment provision toàn bộ resource ở trên và hỗ trợ teardown sạch.
- Reporting Deliverables: Test cases, screenshots cho mỗi status transition, demo recording và workshop instructions song ngữ.

#### Data Contract và External AI Payload

Tất cả module dùng chung một JSON contract để frontend, workflow, storage, API response và analytics không lệch nhau:

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

External AI API chỉ nhận payload đã rút gọn:

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

Payload không được chứa raw PDF files, raw images, API keys, AWS credentials, full documents khi không cần thiết hoặc thông tin nhạy cảm không liên quan. AI Proxy Lambda validate response phải là JSON hợp lệ, `documentType` thuộc `INVOICE`, `RECEIPT` hoặc `UNKNOWN`, `totalAmount` là number, `invoiceDate` theo ISO-8601 khi có, `confidenceScore` nằm trong khoảng 0 đến 1, và các field confidence thấp hoặc thiếu phải được thể hiện trong `reviewReasons`.

#### Technology Stack

Stack dưới đây bổ trợ cho danh sách AWS services ở section 3. Đây là toolset code-level cụ thể mà team dùng để build, test, deploy và document nền tảng.

**Frontend**

| Layer | Choice | Notes |
|---|---|---|
| Language | TypeScript 5 | Share type với backend qua workspace package |
| Framework | React 18 | SPA delivered qua CloudFront và deploy bằng Amplify |
| Build tool | Vite | HMR nhanh, zero-config TypeScript |
| Auth | `amazon-cognito-identity-js` | Cognito SDK chính thức, gọn nhẹ; giao tiếp với User Pool ở section 3 |
| API client | `axios` + `@tanstack/react-query` | Axios interceptor gắn JWT của Cognito; TanStack Query cache server state |
| UI | shadcn/ui + Tailwind CSS | Component primitive, không vendor lock |
| Forms | React Hook Form + Zod | Schema Zod reuse được ở backend |
| Routing | React Router v6 | |
| Testing | Vitest (unit) | E2E nằm ngoài scope workshop |

**Backend Lambda**

| Layer | Choice | Notes |
|---|---|---|
| Runtime | Node.js 20.x ở 256 MB | Như đã chốt ở section 3 và 4 |
| Language | TypeScript 5 | Compile bằng esbuild qua SAM |
| AWS SDK | AWS SDK v3 modular | `client-s3`, `s3-request-presigner`, `client-dynamodb`, `lib-dynamodb`, `client-textract`, `client-secrets-manager`, `client-sns`, `client-sfn` |
| External API client | `fetch` / `undici` với timeout và retry limits | Chỉ gọi External AI API từ AI Proxy Lambda |
| Validation | Ajv (JSON Schema) + Zod | Ajv enforce JSON contract của AI Proxy; Zod validate HTTP payload |
| Observability | AWS Lambda Powertools for TypeScript + X-Ray SDK | Structured logs, custom metrics, tracing |
| Testing | Vitest + `aws-sdk-client-mock` | |

**Infrastructure & DevOps**

| Concern | Choice | Notes |
|---|---|---|
| IaC | AWS SAM | Dùng CloudFormation transform; teardown qua `sam delete` hoặc `aws cloudformation delete-stack` theo contingency ở section 7 |
| Local Lambda | AWS SAM CLI (`sam local invoke`, `sam local start-api`) | |
| External AI API | Provider/model cấu hình qua environment variables | API key lưu trong Secrets Manager; mặc định không gửi raw files |
| Monorepo | pnpm workspaces | |
| Linting | ESLint + Prettier | Một config chung cho `apps/*`, `services/*`, `packages/*` |
| Secrets scan | gitleaks pre-commit hook | Chặn commit nhầm key |
| CI/CD | Amplify Hosting cho frontend; GitHub Actions với OIDC → IAM role cho backend | Jobs: `lint-test` khi PR, `deploy-dev` khi push `main`, `deploy-prod` khi tag `v*` |
| Documentation | Hugo + Learn theme (repo hiện tại) | Song ngữ EN/VI, diagrams bằng draw.io |

**Repository Layout**

```text
docuflow-ai/
├── frontend/
│   └── src/                  # Frontend React + Vite deploy bằng Amplify
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
│       ├── schema/           # JSON schema và Zod DTOs
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

Các folder Lambda function khớp với trách nhiệm đã khai báo ở section 3, và shared schema giúp cùng một data contract được áp dụng cho upload, normalization, storage, API response và analytics modules.

#### Phân công trách nhiệm nhóm

| Thành viên | Vai trò | Trách nhiệm chính |
|---|---|---|
| Hoàng Trọng Trà | Leader / Integration Owner | Ingestion, EventBridge, SQS, Job Starter Lambda, Step Functions, integration flow |
| Vũ Duy Tài | AI Owner | Textract, AI Proxy Lambda, External AI API normalization, confidence/status logic |
| Nguyễn Hữu Tịnh | Frontend/Auth Owner | CloudFront, Amplify, Cognito, API Gateway integration, upload/result/review UI |
| Lâm Quang Lộc | Data Owner | DynamoDB, S3 processed JSON, metadata schema, document result management |
| Phạm Tùng Dương | Ops/Security/IaC Owner | IAM, KMS, Secrets Manager, CloudTrail, Budgets, SAM, CloudWatch, X-Ray, SNS/SES |

### 5. Tiến độ & Cột mốc

#### Tiến độ dự án

- Tuần 1: Nghiên cứu yêu cầu FCAJ, chốt MVP scope, định nghĩa data contract và vẽ architecture.
- Tuần 2-3: Xây nền tảng: Cognito, frontend hosting, API Gateway, Lambda upload API, S3 buckets và DynamoDB table.
- Tuần 4-5: Xây ingestion và workflow: EventBridge, SQS, Step Functions, retry/catch handling và status transitions.
- Tuần 6-7: Xây AI extraction và normalization: Textract processing, AI Proxy Lambda, External AI API integration, Secrets Manager secret retrieval, JSON schema validation, confidence/status logic và result storage.
- Tuần 8: Xây result và review UI: document list, document detail page, extracted-field display, correction flow, approval flow và status tracking.
- Tuần 9-10: Bổ sung observability, security, governance và IaC: CloudWatch logs/metrics/alarms, X-Ray tracing, SNS/SES alerts, IAM review, KMS encryption, CloudTrail, External AI API retry limits, AWS Budgets và hợp nhất stack SAM theo environment.
- Tuần 11: Chạy test cases, chụp screenshots, chuẩn bị workshop instructions và verify cleanup.
- Tuần 12: Hoàn thiện nội dung song ngữ, final demo flow, budget estimate và submission materials.

### 6. Ước tính ngân sách

Ngân sách hướng đến workload quy mô workshop mà nhóm 5 người có thể chạy end-to-end và phần lớn vẫn nằm trong AWS Free Tier. Bạn có thể tạo và cập nhật estimate chính thức bằng [AWS Pricing Calculator](https://calculator.aws/#/).

#### Giả định

Ước tính dùng baseline dưới đây để mỗi dòng chi phí đều có thể tái lập:

- Region: `ap-southeast-1` (Singapore — region AWS gần Việt Nam nhất, độ trễ thấp cho người dùng cuối tại VN; giá nhỉnh hơn `us-east-1` ở CloudWatch, DynamoDB, API Gateway và Step Functions, nhưng vẫn nằm trong ngân sách workshop).
- Document volume: 100 file invoice/receipt mỗi tháng, trung bình 2 trang/file (khoảng 200 trang/tháng).
- External AI API usage: 100 normalization requests/tháng qua AI Proxy Lambda, chỉ gửi text/fields và line items đã rút gọn từ Textract. Team đặt provider-side usage cap nếu có và kiểm tra pricing thật trước khi chạy demo.
- API traffic: khoảng 5,000 REST API calls/tháng cho các endpoint upload-url, list-documents và document-status.
- Storage: 1 GB raw documents và 0.5 GB processed JSON/CSV.
- Logs: khoảng 500 MB CloudWatch log ingestion/tháng với retention 7 ngày.
- Users: 5–10 monthly active Cognito users (nằm sâu trong always-free 50,000 MAU).
- Step Functions: 100 Standard-workflow executions/tháng, khoảng 9 states cho mỗi document.

#### Bảng chi phí hàng tháng

| Dịch vụ | Giả định usage | Chi phí/tháng (USD) |
|---|---|---|
| Amazon Textract `AnalyzeExpense` | 200 trang | $2.00 |
| External AI API | 100 normalization requests, payload rút gọn | $0.00 – $2.00 tùy provider |
| AWS Secrets Manager | 1 external API secret | $0.40 |
| AWS Lambda | ~1,200 invocations, 256 MB, ~500 ms | $0.00 – $0.10 |
| Amazon API Gateway (REST) | ~5,000 calls | $0.02 – $0.03 |
| Amazon S3 | 1.5 GB Standard + ít request | $0.03 – $0.10 |
| Amazon DynamoDB (on-demand) | 1,000 writes + 5,000 reads, < 1 GB storage | $0.05 – $0.30 |
| Amazon EventBridge + Amazon SQS | ~100 events, queue volume thấp | $0.00 – $0.10 |
| AWS Step Functions Standard | 100 executions × ~9 states | $0.00 – $0.10 |
| Amazon CloudWatch + AWS X-Ray | ~500 MB logs, retention 7 ngày, traces, ~5 alarms | $0.60 – $1.40 |
| Amazon SNS/SES | ~100 alerts | $0.00 – $0.10 |
| Amazon Cognito | 5–10 MAU (free tier cover 50k) | $0.00 |
| Amazon CloudFront + AWS Amplify Hosting | static React build, demo traffic thấp, build không thường xuyên | $0.00 – $1.00 |
| AWS CloudTrail + AWS Budgets | management events và spending alerts | $0.00 |
| **Tổng** | | **$3.50 – $8.00** |

Quy ra năm: khoảng **$42 – $96 cho 12 tháng**. Con số dao động theo region, số trang Textract, pricing của External AI API provider, số lần retry, kích thước token/payload và retention CloudWatch/X-Ray. AWS SAM/CloudFormation (deployment) và AWS Budgets (cost alerts) cũng được dùng và giữ ở mức **$0.00** trong free usage limits.

#### Ảnh hưởng từ Free Tier

Free Tier phụ thuộc vào ngày tạo account và account plan đang chọn. Account tạo trước ngày 15/07/2025 vẫn có thể theo mô hình legacy 12-month Free Tier, còn account mới hơn có thể dùng AWS Free Tier plan và credits hiện tại. Trước khi chạy workshop, team sẽ kiểm tra Free Tier status thật trong AWS Billing và giữ AWS Budgets alert ở ngưỡng $5 và $10.

Với workload này, một số dịch vụ vẫn có thể miễn phí hoặc gần như miễn phí ở quy mô workshop, như Lambda, DynamoDB, SNS, Cognito, CloudFront và Amplify static hosting. API Gateway, S3 storage, CloudWatch logs/alarms, X-Ray traces, Step Functions và Amplify build minutes/data transfer cần được kiểm tra theo Free Tier và pricing page thực tế của account, thay vì mặc định xem là miễn phí.

Textract và External AI API **không** được giả định là nằm trong AWS Free Tier ở proposal này, nên team phải kiểm tra trực tiếp cả hai loại chi phí và giữ ngân sách workshop hàng tháng ở mức tối đa **$10**.

#### Kịch bản scale

Nếu workload tăng 10× lên 1,000 documents và 2,000 trang/tháng, Textract scale tuyến tính tới khoảng $20/tháng và External AI API scale theo pricing provider, số lần retry và payload size. Các dịch vụ còn lại vẫn ở mức hiện tại, đưa phần AWS-side platform về khoảng **$25 – $40/tháng** trước khi tính provider-specific AI charges.

#### Kiểm soát chi phí

- Đặt AWS Budgets alert ở ngưỡng $5 và $10 với email notification.
- CloudWatch log retention 7 ngày; metric filters chỉ giữ các metric cần cho workflow.
- Cap External AI API request bằng payload size, retry count, timeout và provider-side spending/usage limits nếu có.
- S3 lifecycle: expire raw uploads sau 60 ngày, chuyển processed JSON sang STANDARD-IA sau 30 ngày.
- File mẫu Textract giới hạn ≤ 5 trang/file trong giai đoạn phát triển.
- Step Functions per-task timeout 30 giây để tránh execution chạy lan.
- Cleanup cuối workshop chạy `aws cloudformation delete-stack` (hoặc `sam delete`) trên backend workshop stack, sau đó xoá Amplify app/branches và kiểm tra CloudFront distribution trong Amplify console hoặc CLI. Tổng thể cleanup sẽ xoá Cognito User Pool, S3 buckets, DynamoDB table, Step Functions state machine, API Gateway, Lambda functions, EventBridge rules, SQS queues, SNS topic, SES notification resources, IAM roles, CloudWatch log groups và frontend hosting resources.
- CloudTrail, Budgets, X-Ray, SNS và SES được giữ trong phạm vi workshop hẹp để có governance/evidence mà không tạo chi phí mở.

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

Severity = impact × probability theo thang 1–3 (tối đa 9). Owner là module workshop chịu trách nhiệm cho mitigation.

| ID | Risk | Impact | Probability | Severity | Owner |
|---|---|---|---|---|---|
| R-01 | Scope creep vượt khỏi MVP invoice/receipt | High (3) | Medium (2) | 6 | Tech lead |
| R-02 | Textract accuracy thấp trên scan kém | High (3) | Medium (2) | 6 | AI module |
| R-03 | External AI API trả JSON sai cấu trúc / sai schema | Medium (2) | Medium (2) | 4 | AI module |
| R-04 | Workflow bất đồng bộ lỗi (Step Functions / SQS) | High (3) | Low (1) | 3 | Workflow module |
| R-05 | Cost overrun vượt ngân sách $10/tháng | Medium (2) | Low (1) | 2 | IaC/Ops module |
| R-06 | IAM cấu hình sai làm lộ dữ liệu | High (3) | Low (1) | 3 | Security module |
| R-07 | External AI API timeout, rate limit hoặc outage | Medium (2) | Medium (2) | 4 | AI module |
| R-08 | Presigned URL bị lộ từ frontend hoặc log | High (3) | Low (1) | 3 | Frontend / Security |
| R-09 | Document edge case (scan low-DPI, file mã hóa, quá lớn) | Medium (2) | Medium (2) | 4 | AI module |
| R-10 | Dữ liệu tài liệu nhạy cảm bị gửi không cần thiết sang external provider | High (3) | Low (1) | 3 | Security / AI module |
| R-11 | External AI API key bị lộ hoặc thiếu | High (3) | Low (1) | 3 | Security / IaC module |

#### Chiến lược giảm thiểu

- **R-01 (Scope)**: Khóa MVP chỉ ở invoice và receipt; mọi loại tài liệu mới phải qua change-request review trước khi vào backlog.
- **R-02 (Extraction)**: Dùng sample documents rõ trong dev, đặt confidence threshold 0.7, kết quả thấp confidence chuyển sang `REVIEW_REQUIRED` cho luồng review thủ công.
- **R-03 (External AI JSON)**: Prompt/instruction nghiêm ngặt với JSON-only contract, JSON schema validation trong normalization Lambda, retry tự động 1 lần, sau đó đánh dấu `REVIEW_REQUIRED` nếu cấu trúc vẫn không hợp lệ.
- **R-04 (Workflow)**: Catch states trên mỗi task của Step Functions, SQS Standard queue với DLQ retry 3 lần, CloudWatch alarm trên DLQ depth, và DynamoDB status transitions rõ ràng.
- **R-05 (Cost)**: AWS Budgets alert ở $5 và $10, CloudWatch log retention 7 ngày, External AI API request/retry caps, provider-side spending limit nếu có, S3 lifecycle policy, và giới hạn Textract sample ≤5 trang/file trong dev.
- **R-06 (IAM)**: Mỗi Lambda function có IAM role riêng, Step Functions execution role tách khỏi task role, dùng AWS KMS managed keys cho storage, bật S3 Block Public Access trên mọi bucket, và review định kỳ bằng IAM Access Analyzer.
- **R-07 (External API availability)**: Retry với exponential backoff khi timeout/rate-limit, giới hạn số retry, emit CloudWatch metrics, sau đó mark `REVIEW_REQUIRED` hoặc `FAILED` thay vì chặn toàn bộ workflow.
- **R-08 (Presigned URL)**: TTL 5 phút mỗi URL, mỗi URL scope đúng 1 object key, không log URL đầy đủ, và lọc URL ra khỏi client-side error report.
- **R-09 (Edge cases)**: Frontend pre-validate file type, size (≤10 MB) và page count; backend trả lỗi có cấu trúc và ghi file vào prefix S3 `quarantine/` để xử lý thủ công.
- **R-10 (Data minimization)**: Chỉ gửi extracted text/fields và raw text preview ngắn khi cần; không gửi raw PDF/image files, AWS credentials, API keys hoặc field nhạy cảm không liên quan sang external provider.
- **R-11 (Secret handling)**: Lưu External AI API key trong Secrets Manager, chỉ cấp quyền read secret cụ thể cho AI Proxy Lambda, không log key, và rotate key nếu nghi ngờ bị lộ.

#### Kế hoạch dự phòng

- **R-02 / R-03 demo failure**: Dùng prepared sample extraction result để trình bày status/result UI đồng thời show failed Step Functions execution history để minh bạch.
- **R-04 backlog tăng**: Dừng upload, kiểm tra SQS và DLQ, replay chỉ 1 batch mẫu nhỏ sau khi fix root cause.
- **R-05 cost spike**: Dừng test upload, disable External AI API key hoặc hạ provider usage limits, hạ CloudWatch retention xuống 1 ngày, dọn raw/processed buckets, và chạy `delete-stack` của CloudFormation nếu cần.
- **R-06 IAM exposure**: Rotate credential bị lộ, disable role bị ảnh hưởng, re-deploy stack từ commit sạch.
- **R-07 External AI API outage**: Route tài liệu bị ảnh hưởng sang `REVIEW_REQUIRED`, show Step Functions execution history, và chỉ dùng prepared normalized JSON cho demo UI nếu provider vẫn unavailable.
- **R-08 URL leak**: Vô hiệu hóa credential đang active của user, re-issue presigned URL, audit Amplify access logs / API Gateway logs để tìm pattern URL bị lộ.
- **R-10 / R-11 data hoặc key exposure**: Dừng external API calls, rotate provider key, kiểm tra CloudWatch logs để tìm dữ liệu lộ, và chỉ bật lại sau khi verify payload minimization và log redaction.

### 8. Kết quả kỳ vọng

#### Cải thiện kỹ thuật

DocuFlow AI thay thế nhập liệu invoice/receipt thủ công bằng workflow xử lý tài liệu serverless. Hệ thống cung cấp secure upload, Textract extraction tự động, External AI API normalization qua AI Proxy Lambda, schema validation, metadata storage, status tracking, notifications, operational logs, audit visibility và cost governance.

#### Tiêu chí thành công

Workshop được xem là hoàn thành khi từng tiêu chí sau có evidence:

- Field-level extraction accuracy ≥ 90% cho các summary fields bắt buộc trên test set 20 invoice/receipt mẫu được chuẩn bị sẵn. Các field bắt buộc gồm `vendorName`, `invoiceDate`, `totalAmount`, `taxAmount` và `currency`; line-item extraction được xem là bonus evidence vì biến động nhiều hơn theo format tài liệu.
- End-to-end processing latency ≤ 60 giây ở mức 95th percentile.
- Chi phí AWS hàng tháng của workshop ≤ **$10**, verify qua AWS Budgets và Cost Explorer.
- Mỗi Lambda function có IAM role riêng theo least-privilege; không role nào được chia sẻ.
- Các status path chính của DynamoDB đều có screenshots: `UPLOADED`, `QUEUED`, `PROCESSING`, `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED`, `CORRECTED`, `APPROVED`.
- Step Functions chạy đủ các bước validate, Textract, AI Proxy, confidence/status và save-result.
- External AI API key được lưu trong Secrets Manager, không nằm trong source code, frontend code, logs hoặc API responses.
- Payload gửi sang External AI chỉ chứa output Textract đã rút gọn; raw PDF/image files mặc định không gửi sang external provider.
- Có CloudWatch logs/alarms, X-Ray trace evidence, SNS/SES alert evidence, CloudTrail audit evidence và Budgets screenshot.
- Stack SAM/CloudFormation deploy sạch từ tài khoản mới và teardown sạch, không cần dọn thủ công.
- Báo cáo song ngữ (EN/VI) cover đủ tám module workshop trong `5-Workshop`.

#### Giá trị dài hạn

Dự án tạo nền tảng tái sử dụng cho xử lý chứng từ tài chính trên AWS. Các nhóm sau có thể mở rộng cùng pattern này cho nhiều loại tài liệu hơn, validation rules chặt hơn, reporting đầy đủ hơn và tích hợp sâu hơn với hệ thống finance. Với FCAJ, workshop thể hiện cách dùng thực tế các AWS serverless services, AI-assisted document processing, external API security, audit visibility, cost control, observability và cleanup trong một giải pháp thống nhất.
