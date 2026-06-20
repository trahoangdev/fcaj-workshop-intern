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

Giải pháp sử dụng kiến trúc serverless, event-driven, deploy qua AWS CloudFormation tại region `ap-southeast-1` (Singapore). Người dùng đăng nhập bằng Amazon Cognito và upload tài liệu qua presigned URL. Amazon S3 lưu file gốc, EventBridge và SQS tách lớp ingestion khỏi processing, Step Functions điều phối workflow, Amazon Textract trích xuất dữ liệu invoice/receipt, Amazon Bedrock chuẩn hóa và enrich kết quả, Lambda validate JSON cuối cùng, và DynamoDB lưu trạng thái cùng metadata của tài liệu.

Workshop được giới hạn cho nhóm 5 người. Mỗi người phụ trách một module: frontend/auth/upload, ingestion/workflow, AI extraction và validation, data/result dashboard, observability/security/IaC. Cách chia này phù hợp cho báo cáo thực tập nhưng vẫn thể hiện được một hệ thống AWS serverless end-to-end.

### 2. Tuyên bố vấn đề

#### Vấn đề là gì?

Nhiều doanh nghiệp vẫn xử lý invoice và receipt qua email, folder chia sẻ, spreadsheet hoặc nhập liệu thủ công. Cách làm này tạo ra nhiều vấn đề:

- Đội Finance và Operations mất thời gian lặp lại để nhập vendor name, invoice date, tax, total amount và line items.
- Nhập liệu thủ công dễ gây sai giá trị, thiếu field và format dữ liệu không nhất quán.
- Người dùng khó biết tài liệu đang uploaded, processing, failed, extracted hay waiting for review.
- Tài liệu khó tìm kiếm, kiểm toán và tổng hợp khi bị phân tán qua email hoặc shared drive.
- Chạy server thường trực cho workload tài liệu không đều làm tăng chi phí và công vận hành.

#### Giải pháp

DocuFlow AI tập trung upload và xử lý tài liệu trên AWS. Frontend cung cấp login, upload, theo dõi trạng thái, xem kết quả và chỉnh sửa thủ công khi cần. Backend dùng presigned URL để upload an toàn lên S3, sau đó xử lý bất đồng bộ bằng EventBridge, SQS, Lambda và Step Functions.

Amazon Textract trích xuất field từ invoice và receipt. Amazon Bedrock nhận output Textract, map các field name không nhất quán về một schema thống nhất, phân loại tài liệu là invoice hoặc receipt, enrich contextual fields và trả structured JSON. Lambda validate schema, confidence score và business rules trước khi lưu kết quả vào DynamoDB và S3 processed storage.

#### Lợi ích và giá trị đầu tư

DocuFlow AI giảm công nhập liệu thủ công và tạo quy trình lặp lại được cho xử lý chứng từ tài chính. Hệ thống tạo metadata store có thể tìm kiếm, hiển thị trạng thái rõ ràng và hỗ trợ review loop cho tài liệu có confidence thấp. Vì MVP dùng serverless, nền tảng giữ chi phí thấp cho workload workshop và có thể mở rộng theo số lượng tài liệu khi cần.

Chi phí AWS ước tính cho workload workshop khoảng **$3.00-$6.00 mỗi tháng**, tương đương **$36-$72 cho 12 tháng** tại region `ap-southeast-1` (Singapore), tùy model Bedrock được chọn, token usage và log retention. Đề tài cũng có giá trị học tập rõ ràng về AWS serverless architecture, AI document processing, asynchronous workflow, security, monitoring và cleanup.

### 3. Kiến trúc giải pháp

DocuFlow AI dùng kiến trúc AWS serverless cho secure upload, asynchronous processing, AI-assisted extraction, result storage và operational monitoring. Kiến trúc tổng quan như sau:

![Kiến trúc high-level của DocuFlow AI](/images/2-Proposal/docuflow_high_level_architecture.png)

#### Dịch vụ AWS sử dụng

Các dịch vụ dưới đây được nhóm theo layer kiến trúc. Mỗi dòng map trực tiếp với bảng Ước tính ngân sách ở Section 6.

**Identity & Security**

- Amazon Cognito: User Pool có groups (end-user, reviewer, admin), dùng làm Cognito User Pool authorizer cho mọi endpoint API Gateway.
- AWS IAM: Role least-privilege riêng cho từng Lambda function và một execution role riêng cho Step Functions; không dùng role admin chia sẻ.
- AWS KMS: AWS-managed keys (`aws/s3`, `aws/dynamodb`, `aws/sqs`) cho encryption at rest; không dùng customer-managed key để giữ chi phí $0.

**Frontend Delivery**

- Amazon S3 + Amazon CloudFront: Bucket S3 private giữ static SPA build (HTML/JS/CSS); CloudFront serve qua HTTPS với Origin Access Control trỏ vào S3, custom domain (tùy chọn) và global edge cache. Build chạy trên GitHub Actions, không trên AWS.

**API & Compute**

- Amazon API Gateway (REST): Các endpoint `POST /uploads`, `GET /documents/{id}`, `GET /documents/{id}/result`, `PUT /documents/{id}/review`, đều được Cognito authorizer bảo vệ.
- AWS Lambda: Năm function Node.js 20.x ở mức 256 MB — `presignUpload`, `startProcessing`, `validateExtraction`, `updateStatus`, `reviewUpdate`.

**Storage**

- Amazon S3: Hai bucket — `docuflow-raw` (Block Public Access, SSE-KMS, lifecycle expire sau 60 ngày) và `docuflow-processed` (bật versioning, chuyển STANDARD-IA sau 30 ngày).
- Amazon DynamoDB: Bảng on-demand `Documents` với PK `documentId` và GSI trên `userId`+`status` phục vụ dashboard người dùng; lưu metadata, status, confidence score, S3 paths và review state.

**Eventing & Workflow**

- Amazon EventBridge: Default bus nhận S3 `Object Created` events và route sang SQS.
- Amazon SQS: Standard queue + Dead-Letter Queue (tối đa 3 retry) để đệm spike và cô lập poison message.
- AWS Step Functions (Standard Workflow): State machine 8 state — `Validate` → `Textract` → `Bedrock` → `ValidateJSON` → `SaveResult` → `UpdateStatus` ở happy path, cộng thêm 2 terminal Catch state `MarkReviewRequired` và `MarkFailed` cập nhật DynamoDB item sang `REVIEW_REQUIRED` hoặc `FAILED` tương ứng.

**AI Services**

- Amazon Textract `AnalyzeExpense`: API chuyên cho invoice và receipt; trả về `SUMMARY_FIELDS` và `LINE_ITEM_FIELDS` để Bedrock xử lý ở bước sau.
- Amazon Bedrock: Small text model (Amazon Nova Lite hoặc Claude 3 Haiku), gọi với system prompt nghiêm ngặt, cap `maxTokens` và yêu cầu output JSON-only.

**Observability & Notification**

- Amazon CloudWatch: Logs (retention 7 ngày), Metrics, Alarms (Lambda error rate, Step Functions failed executions, SQS DLQ depth, Bedrock throttling) và Logs Insights cho query ad-hoc.
- Amazon SNS: Topic `docuflow-alerts` gửi email cho reviewer/admin khi document `FAILED` hoặc `REVIEW_REQUIRED`.

**Operations & Cost**

- AWS CloudFormation: Một stack duy nhất cho mỗi environment để deploy có thể tái lập và teardown sạch.
- AWS Budgets: Email alert ở ngưỡng $5 và $10 hàng tháng đã định nghĩa ở Section 6.

#### Luồng xử lý end-to-end

Happy path cho một invoice hoặc receipt đi qua hệ thống như sau:

1. **Đăng nhập** — Người dùng xác thực qua Cognito và nhận JWT cho các API call sau.
2. **Xin upload slot** — Frontend gọi `POST /uploads`. Lambda `presignUpload` sinh `documentId`, ghi DynamoDB item với status `UPLOADED`, và trả presigned URL S3 ngắn hạn theo path `docuflow-raw/{userId}/{documentId}.{ext}`.
3. **Upload trực tiếp lên S3** — Browser PUT file qua presigned URL; document bytes không bao giờ đi qua API Gateway hay Lambda.
4. **Nạp event** — S3 phát `Object Created` sang EventBridge, route tiếp sang SQS. Lambda `startProcessing` consume message và start Step Functions execution; status chuyển `PROCESSING`.
5. **Trích xuất AI** — Step Functions gọi Textract `AnalyzeExpense`, rồi Bedrock để chuẩn hóa, rồi Lambda `validateExtraction` để check schema và confidence.
6. **Lưu kết quả** — State `SaveResult` ghi JSON cấu trúc vào `docuflow-processed` và cập nhật DynamoDB item sang `EXTRACTED` hoặc `REVIEW_REQUIRED` tùy confidence và kết quả validate.
7. **Thông báo reviewer** — Khi `REVIEW_REQUIRED` hoặc `FAILED`, SNS publish lên topic `docuflow-alerts` và gửi email cho subscriber.
8. **Sửa lỗi review** — Reviewer mở document, gọi `PUT /documents/{id}/review`. Lambda `reviewUpdate` ghi field đã chỉnh và chuyển status sang `REVIEWED`.

Các đường lỗi được xử lý qua Catch branches của Step Functions và retry/DLQ của SQS, không dùng try/catch rải rác trong Lambda. Khi state bất kỳ catch lỗi, workflow chuyển sang `MarkFailed` (hoặc `MarkReviewRequired` cho trường hợp confidence thấp / schema lỗi) và DynamoDB status được cập nhật tương ứng. Các lỗi ingestion xảy ra trước khi Step Functions chạy được hấp thụ bởi SQS retry và rơi vào DLQ.

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

Dự án chạy theo bốn giai đoạn ngắn. Section 5 chia mỗi giai đoạn thành các tuần cụ thể.

1. **Thiết kế** — Khóa MVP scope, vẽ kiến trúc và định nghĩa data contract.
2. **Ước tính** — Chạy AWS Pricing Calculator và chốt mức chi phí workshop.
3. **Tinh chỉnh** — Điều chỉnh workflow boundaries, confidence threshold và log retention cho nhóm 5 người.
4. **Build, test, deploy** — Triển khai, test end-to-end, thu thập evidence và verify cleanup.

#### Yêu cầu kỹ thuật

- Frontend: Single-page web app lưu trong bucket Amazon S3 private và serve qua Amazon CloudFront với Origin Access Control, có login, upload, status/result pages và reviewer correction UI.
- Authentication: Cognito user pool với ba groups — `end-user`, `reviewer`, `admin` — gắn vào API Gateway dưới dạng Cognito authorizer.
- Document Input: Invoice/receipt dạng PDF, JPG hoặc PNG được upload qua presigned URL TTL 5 phút, với pre-validate ở frontend cho file type, size (≤10 MB) và page count.
- Processing: Step Functions Standard Workflow (8 state) với Lambda task chạy Node.js 20.x ở mức 256 MB, Textract `AnalyzeExpense`, Bedrock normalization có cap `maxTokens`, JSON schema validation, Retry/Catch trên mỗi state, và status rõ ràng `REVIEW_REQUIRED` và `FAILED`.
- Data Model: DynamoDB table `Documents` với PK `documentId` và GSI trên `userId`+`status`. Item attributes: `documentId`, `userId`, `fileName`, `documentType`, `status`, extracted fields, `confidenceScore`, `rawS3Key`, `processedS3Key`, `createdAt`, `updatedAt`, và review fields (`reviewerId`, `reviewedAt`).
- Observability: CloudWatch Logs (retention 7 ngày), Metrics và Alarms trên Lambda errors, Step Functions failed executions, SQS DLQ depth và Bedrock throttling, kèm Logs Insights cho query ad-hoc.
- Security: Không public S3 bucket (Block Public Access), không hard-code key, IAM role theo least privilege cho mỗi Lambda, AWS KMS managed keys cho encryption at rest, presigned URL TTL ngắn, và CloudFormation teardown script.
- Infrastructure as Code: Một stack AWS CloudFormation duy nhất cho mỗi environment provision toàn bộ resource ở trên và hỗ trợ teardown sạch.
- Reporting Deliverables: Test cases, screenshots cho mỗi status transition, demo recording và workshop instructions song ngữ.

#### Technology Stack

Stack dưới đây bổ trợ cho danh sách AWS services ở section 3. Đây là toolset code-level cụ thể mà team dùng để build, test, deploy và document nền tảng.

**Frontend**

| Layer | Choice | Notes |
|---|---|---|
| Language | TypeScript 5 | Share type với backend qua workspace package |
| Framework | React 18 | SPA serve qua S3 + CloudFront |
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
| AWS SDK | AWS SDK v3 modular | `client-s3`, `s3-request-presigner`, `client-dynamodb`, `lib-dynamodb`, `client-textract`, `client-bedrock-runtime`, `client-sns`, `client-sfn` |
| Validation | Ajv (JSON Schema) + Zod | Ajv enforce JSON contract của Bedrock; Zod validate HTTP payload |
| Observability | AWS Lambda Powertools for TypeScript | Structured logs, custom metrics, tracing |
| Testing | Vitest + `aws-sdk-client-mock` | |

**Infrastructure & DevOps**

| Concern | Choice | Notes |
|---|---|---|
| IaC | AWS SAM (CloudFormation transform) | Compile ra CloudFormation; teardown qua `sam delete` hoặc `aws cloudformation delete-stack` theo contingency ở section 7 |
| Local Lambda | AWS SAM CLI (`sam local invoke`, `sam local start-api`) | |
| Bedrock model | Amazon Nova Lite (chính), Claude 3 Haiku (fallback) | Cả 2 đều GA ở `ap-southeast-1`; chọn Nova Lite để khớp dòng $0.10–$1.00 ở section 6 |
| Monorepo | pnpm workspaces | |
| Linting | ESLint + Prettier | Một config chung cho `apps/*`, `services/*`, `packages/*` |
| Secrets scan | gitleaks pre-commit hook | Chặn commit nhầm key |
| CI/CD | GitHub Actions với OIDC → IAM role | Jobs: `lint-test` khi PR, `deploy-dev` khi push `main`, `deploy-prod` khi tag `v*` |
| Documentation | Hugo + Learn theme (repo hiện tại) | Song ngữ EN/VI, diagrams bằng draw.io |

**Repository Layout**

```text
docuflow-ai/
├── apps/
│   └── web/                  # Frontend React + Vite
├── packages/
│   ├── shared-types/         # Zod schemas, DTOs
│   └── shared-config/        # eslint, tsconfig, prettier
├── services/
│   ├── functions/            # 5 Lambda handler từ section 3
│   │   ├── presignUpload/
│   │   ├── startProcessing/
│   │   ├── validateExtraction/
│   │   ├── updateStatus/
│   │   └── reviewUpdate/
│   └── statemachines/        # ASL JSON cho Step Functions 8 state
├── infrastructure/
│   ├── template.yaml         # SAM root
│   └── parameters/{dev,prod}.json
├── samconfig.toml
├── pnpm-workspace.yaml
└── .github/workflows/
```

Năm folder Lambda function khớp với tên đã khai báo ở section 3, và state machine trong `services/statemachines/` ứng với workflow 8 state liệt kê ở mục "Eventing & Workflow".

### 5. Tiến độ & Cột mốc

#### Tiến độ dự án

- Tuần 1: Nghiên cứu yêu cầu FCAJ, chốt MVP scope, định nghĩa data contract và vẽ architecture.
- Tuần 2-3: Xây nền tảng: Cognito, frontend hosting, API Gateway, Lambda upload API, S3 buckets và DynamoDB table.
- Tuần 4-5: Xây ingestion và workflow: EventBridge, SQS, Step Functions, retry/catch handling và status transitions.
- Tuần 6-7: Xây AI extraction: Textract processing, Bedrock enrichment, JSON schema validation, confidence score và result storage.
- Tuần 8: Xây result và review UI: document status page, extracted-field display, reviewer correction flow và update API.
- Tuần 9-10: Bổ sung observability, security và IaC: CloudWatch logs/metrics/alarms, SNS alerts, IAM review, KMS encryption, AWS Budgets, và hợp nhất stack CloudFormation theo environment.
- Tuần 11: Chạy test cases, chụp screenshots, chuẩn bị workshop instructions và verify cleanup.
- Tuần 12: Hoàn thiện nội dung song ngữ, final demo flow, budget estimate và submission materials.

### 6. Ước tính ngân sách

Ngân sách hướng đến workload quy mô workshop mà nhóm 5 người có thể chạy end-to-end và phần lớn vẫn nằm trong AWS Free Tier. Bạn có thể tạo và cập nhật estimate chính thức bằng [AWS Pricing Calculator](https://calculator.aws/#/).

#### Giả định

Ước tính dùng baseline dưới đây để mỗi dòng chi phí đều có thể tái lập:

- Region: `ap-southeast-1` (Singapore — region AWS gần Việt Nam nhất, độ trễ thấp cho người dùng cuối tại VN; giá nhỉnh hơn `us-east-1` ở CloudWatch, DynamoDB, API Gateway và Step Functions, nhưng vẫn nằm trong ngân sách workshop).
- Document volume: 100 file invoice/receipt mỗi tháng, trung bình 2 trang/file (khoảng 200 trang/tháng).
- Bedrock usage: 100 enrichment requests/tháng, cap khoảng 5,000 input tokens và 2,000 output tokens mỗi request, dùng small text model (Amazon Nova Lite hoặc Claude 3 Haiku).
- API traffic: khoảng 5,000 REST API calls/tháng cho các endpoint upload, status, result và review.
- Storage: 1 GB raw documents và 0.5 GB processed JSON/CSV.
- Logs: khoảng 500 MB CloudWatch log ingestion/tháng với retention 7 ngày.
- Users: 5–10 monthly active Cognito users (nằm sâu trong always-free 50,000 MAU).
- Step Functions: 100 Standard-workflow executions/tháng, khoảng 8 states cho mỗi document.

#### Bảng chi phí hàng tháng

| Dịch vụ | Giả định usage | Chi phí/tháng (USD) |
|---|---|---|
| Amazon Textract `AnalyzeExpense` | 200 trang | $2.00 |
| Amazon Bedrock (small text model) | 100 requests, ~5k in / 2k out tokens | $0.10 – $1.00 |
| AWS Lambda | ~1,000 invocations, 256 MB, ~500 ms | $0.00 – $0.10 |
| Amazon API Gateway (REST) | ~5,000 calls | $0.02 – $0.03 |
| Amazon S3 | 1.5 GB Standard + ít request | $0.03 – $0.10 |
| Amazon DynamoDB (on-demand) | 1,000 writes + 5,000 reads, < 1 GB storage | $0.05 – $0.30 |
| Amazon EventBridge + Amazon SQS | ~100 events, queue volume thấp | $0.00 – $0.10 |
| AWS Step Functions Standard | 100 executions × 8 states | $0.00 – $0.10 |
| Amazon CloudWatch | ~500 MB logs, retention 7 ngày, ~5 alarms | $0.55 – $1.20 |
| Amazon SNS | ~100 publishes | $0.00 – $0.10 |
| Amazon Cognito | 5–10 MAU (free tier cover 50k) | $0.00 |
| Amazon S3 (static SPA) + Amazon CloudFront | build nhỏ + demo traffic | $0.00 – $1.00 |
| **Tổng** | | **$3.00 – $6.00** |

Quy ra năm: khoảng **$36 – $72 cho 12 tháng**. Con số dao động theo region, lựa chọn model Bedrock, lượng token và retention CloudWatch. AWS CloudFormation (deployment) và AWS Budgets (cost alerts) cũng được dùng và giữ ở mức **$0.00** trong free usage limits.

#### Ảnh hưởng từ Free Tier

Trên tài khoản AWS mới (dưới 12 tháng), hầu hết các dòng phía trên được Free Tier bao phủ: Lambda (1M requests + 400k GB-s/tháng always free), API Gateway REST (1M calls/tháng trong 12 tháng), DynamoDB (25 GB storage + 25 RCU/WCU always free), S3 (5 GB trong 12 tháng), CloudWatch (5 GB log ingestion + 10 metrics/alarms always free), SNS (1M publishes/tháng), Cognito (50,000 MAU always free), và Amazon CloudFront (1 TB data transfer + 10M HTTPS requests/tháng always free).

Textract và Bedrock **không** có free tier, nên workshop chạy trên tài khoản mới thường chỉ tốn thực tế khoảng **$2 – $3 mỗi tháng**.

#### Kịch bản scale

Nếu workload tăng 10× lên 1,000 documents và 2,000 trang/tháng, Textract scale tuyến tính tới khoảng $20/tháng và Bedrock khoảng $1 – $10/tháng. Các dịch vụ còn lại vẫn ở mức hiện tại, đưa nền tảng về khoảng **$25 – $40/tháng** ở mức volume này.

#### Kiểm soát chi phí

- Đặt AWS Budgets alert ở ngưỡng $5 và $10 với email notification.
- CloudWatch log retention 7 ngày; metric filters chỉ giữ các metric cần cho workflow.
- Cap Bedrock request bằng `maxTokens` và validate input length trong Lambda.
- S3 lifecycle: expire raw uploads sau 60 ngày, chuyển processed JSON sang STANDARD-IA sau 30 ngày.
- File mẫu Textract giới hạn ≤ 5 trang/file trong giai đoạn phát triển.
- Step Functions per-task timeout 30 giây để tránh execution chạy lan.
- Cleanup cuối workshop chạy `aws cloudformation delete-stack` (hoặc `sam delete`) trên stack của workshop, xoá Cognito User Pool, S3 buckets, DynamoDB table, Step Functions state machine, CloudFront distribution, API Gateway, Lambda functions, EventBridge rules, SQS queues, SNS topic, IAM roles và CloudWatch log groups trong một thao tác.

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

Severity = impact × probability theo thang 1–3 (tối đa 9). Owner là module workshop chịu trách nhiệm cho mitigation.

| ID | Risk | Impact | Probability | Severity | Owner |
|---|---|---|---|---|---|
| R-01 | Scope creep vượt khỏi MVP invoice/receipt | High (3) | Medium (2) | 6 | Tech lead |
| R-02 | Textract accuracy thấp trên scan kém | High (3) | Medium (2) | 6 | AI module |
| R-03 | Bedrock trả JSON sai cấu trúc / sai schema | Medium (2) | Medium (2) | 4 | AI module |
| R-04 | Workflow bất đồng bộ lỗi (Step Functions / SQS) | High (3) | Low (1) | 3 | Workflow module |
| R-05 | Cost overrun vượt ngân sách $10/tháng | Medium (2) | Low (1) | 2 | IaC/Ops module |
| R-06 | IAM cấu hình sai làm lộ dữ liệu | High (3) | Low (1) | 3 | Security module |
| R-07 | Model Bedrock đã chọn không GA ở `ap-southeast-1` | Medium (2) | Low (1) | 2 | AI module |
| R-08 | Presigned URL bị lộ từ frontend hoặc log | High (3) | Low (1) | 3 | Frontend / Security |
| R-09 | Document edge case (scan low-DPI, file mã hóa, quá lớn) | Medium (2) | Medium (2) | 4 | AI module |

#### Chiến lược giảm thiểu

- **R-01 (Scope)**: Khóa MVP chỉ ở invoice và receipt; mọi loại tài liệu mới phải qua change-request review trước khi vào backlog.
- **R-02 (Extraction)**: Dùng sample documents rõ trong dev, đặt confidence threshold 0.7, kết quả thấp confidence chuyển sang `REVIEW_REQUIRED` cho luồng review thủ công.
- **R-03 (Bedrock JSON)**: System prompt nghiêm ngặt với JSON-only contract, JSON schema validation trong Lambda `validateExtraction`, retry tự động 1 lần, sau đó đánh dấu `REVIEW_REQUIRED` nếu cấu trúc vẫn không hợp lệ.
- **R-04 (Workflow)**: Catch states trên mỗi task của Step Functions, SQS Standard queue với DLQ retry 3 lần, CloudWatch alarm trên DLQ depth, và DynamoDB status transitions rõ ràng.
- **R-05 (Cost)**: AWS Budgets alert ở $5 và $10, CloudWatch log retention 7 ngày, cap `maxTokens` của Bedrock, S3 lifecycle policy, và giới hạn Textract sample ≤5 trang/file trong dev.
- **R-06 (IAM)**: Mỗi Lambda function có IAM role riêng, Step Functions execution role tách khỏi task role, dùng AWS KMS managed keys cho storage, bật S3 Block Public Access trên mọi bucket, và review định kỳ bằng IAM Access Analyzer.
- **R-07 (Bedrock region)**: Kiểm tra model đã chọn có GA ở `ap-southeast-1` trước khi dev; nếu không thì fallback cross-region invoke sang `us-east-1` chỉ cho Bedrock, cô lập sau 1 Lambda có env var `BEDROCK_REGION`.
- **R-08 (Presigned URL)**: TTL 5 phút mỗi URL, mỗi URL scope đúng 1 object key, không log URL đầy đủ, và lọc URL ra khỏi client-side error report.
- **R-09 (Edge cases)**: Frontend pre-validate file type, size (≤10 MB) và page count; backend trả lỗi có cấu trúc và ghi file vào prefix S3 `quarantine/` để xử lý thủ công.

#### Kế hoạch dự phòng

- **R-02 / R-03 demo failure**: Dùng prepared sample extraction result để trình bày status/result UI đồng thời show failed Step Functions execution history để minh bạch.
- **R-04 backlog tăng**: Dừng upload, kiểm tra SQS và DLQ, replay chỉ 1 batch mẫu nhỏ sau khi fix root cause.
- **R-05 cost spike**: Dừng test upload, hạ CloudWatch retention xuống 1 ngày, dọn raw/processed buckets, và chạy `delete-stack` của CloudFormation nếu cần.
- **R-06 IAM exposure**: Rotate credential bị lộ, disable role bị ảnh hưởng, re-deploy stack từ commit sạch.
- **R-07 Bedrock outage**: Đổi env var `BEDROCK_REGION` của Lambda sang region fallback trong thời gian demo.
- **R-08 URL leak**: Vô hiệu hóa credential đang active của user, re-issue presigned URL, audit log CloudFront / API Gateway tìm pattern URL bị lộ.

### 8. Kết quả kỳ vọng

#### Cải thiện kỹ thuật

DocuFlow AI thay thế nhập liệu invoice/receipt thủ công bằng workflow xử lý tài liệu serverless. Hệ thống cung cấp secure upload, Textract extraction tự động, Bedrock normalization/enrichment, schema validation, metadata storage, status tracking, review handling, notifications và operational logs.

#### Tiêu chí thành công

Workshop được xem là hoàn thành khi từng tiêu chí sau có evidence:

- Extraction accuracy ≥ 90% trên test set 20 invoice/receipt mẫu được chuẩn bị sẵn.
- End-to-end processing latency ≤ 60 giây ở mức 95th percentile.
- Chi phí AWS hàng tháng của workshop ≤ **$10**, verify qua AWS Budgets và Cost Explorer.
- Mỗi Lambda function có IAM role riêng theo least-privilege; không role nào được chia sẻ.
- Cả bốn terminal status path của DynamoDB đều có screenshots: `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED` (qua Step Functions Catch hoặc SQS DLQ), `REVIEWED`.
- Stack CloudFormation deploy sạch từ tài khoản mới và teardown sạch, không cần dọn thủ công.
- Báo cáo song ngữ (EN/VI) cover đủ tám module workshop trong `5-Workshop`.

#### Giá trị dài hạn

Dự án tạo nền tảng tái sử dụng cho xử lý chứng từ tài chính trên AWS. Các nhóm sau có thể mở rộng cùng pattern này cho nhiều loại tài liệu hơn, validation rules chặt hơn, reporting đầy đủ hơn và tích hợp sâu hơn với hệ thống finance. Với FCAJ, workshop thể hiện cách dùng thực tế các AWS serverless services, AI document processing, cost control, security, observability và cleanup trong một giải pháp thống nhất.
