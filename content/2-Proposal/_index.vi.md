---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm. -->

# DocuFlow AI - Nền tảng xử lý invoice & receipt thông minh bằng AWS Serverless

{{% notice info %}}
**Khuyến nghị triển khai:** chốt MVP xử lý invoice/receipt end-to-end bằng kiến trúc serverless core.
{{% /notice %}}

### 1. Tóm tắt điều hành

DocuFlow AI là nền tảng xử lý chứng từ thông minh trên AWS, tập trung vào invoice và receipt cho đội Finance, Accounting, Operations và Reviewer/Admin của doanh nghiệp vừa và nhỏ. Hệ thống cho phép người dùng đăng nhập, upload tài liệu qua presigned URL, tự động trích xuất thông tin bằng Amazon Textract, chuẩn hóa và enrich kết quả bằng Amazon Bedrock, lưu metadata, theo dõi trạng thái xử lý, gửi cảnh báo khi lỗi hoặc confidence thấp, và hỗ trợ reviewer chỉnh sửa kết quả khi cần.

Điểm mạnh của đề tài là bám sát pain point thật: nhập liệu chứng từ thủ công tốn thời gian, dễ sai, khó audit và khó tổng hợp dữ liệu. Kiến trúc cũng phù hợp yêu cầu workshop vì có thiết kế cloud rõ ràng, nhiều dịch vụ AWS managed/serverless, quy trình end-to-end, log/metric/alert, bảo mật cơ bản, kiểm thử và cleanup.

| Thông tin | Giá trị đề xuất |
| --- | --- |
| Tên dự án | DocuFlow AI - Serverless Intelligent Invoice & Receipt Processing Platform on AWS |
| Đối tượng người dùng | Finance, Accounting, Operations, Admin/Reviewer trong doanh nghiệp vừa và nhỏ |
| Mục tiêu chính | Tự động tiếp nhận, trích xuất, chuẩn hóa, lưu trữ, giám sát và phân tích hóa đơn/biên nhận |
| Nguyên tắc kiến trúc | Serverless, event-driven, scale-to-zero cho compute, không EC2 trong MVP |
| Quy mô nhóm | 5 người, mỗi người sở hữu một module AWS rõ ràng |
| Trạng thái khuyến nghị | Đủ chuẩn để triển khai nếu khóa MVP và data contract trước khi code |

### 2. Tuyên bố vấn đề

Nhiều doanh nghiệp vẫn xử lý hóa đơn, biên nhận, phiếu chi và chứng từ mua hàng bằng email, folder chia sẻ hoặc nhập liệu thủ công. Khi số lượng chứng từ tăng vào cuối tháng hoặc cuối quý, quy trình thủ công dễ gây nghẽn, thiếu khả năng theo dõi và khó kiểm toán.

| Pain point | Tác động nghiệp vụ | Cách DocuFlow AI xử lý |
| --- | --- | --- |
| Nhập liệu thủ công | Sai vendor, ngày hóa đơn, số tiền, thuế hoặc line item | Textract trích xuất trường; Lambda chuẩn hóa JSON và validate schema |
| Không có trạng thái xử lý | Người dùng không biết file đang xử lý, lỗi hay cần review | DynamoDB status table và frontend polling |
| Tài liệu phân tán | Khó tìm lại, khó audit, khó phân tích | S3 raw/processed bucket, documentId, metadata thống nhất |
| Tài liệu chất lượng thấp | OCR có thể sai hoặc thiếu field | Confidence score, `REVIEW_REQUIRED`, SNS alert và manual review loop |
| Chi phí vận hành khó kiểm soát | Server chạy thường trực gây lãng phí cho workload không đều | Lambda, SQS, Step Functions, event-driven processing; không EC2 trong MVP |

{{% notice note %}}
**Quyết định phạm vi:** MVP chỉ xử lý invoice và receipt. Luồng triển khai tập trung vào login, upload bằng presigned URL, extraction, storage, result/status, alert và manual review.
{{% /notice %}}

### 3. Phạm vi, mục tiêu và tiêu chí thành công

#### 3.1 Phạm vi MVP

- Người dùng đăng nhập bằng Amazon Cognito và upload PDF, JPG hoặc PNG qua frontend.
- Frontend gọi Amazon API Gateway và AWS Lambda để tạo presigned URL và tạo processing job.
- Tài liệu vào S3 raw bucket kích hoạt EventBridge, SQS và Step Functions Standard Workflow.
- Amazon Textract đọc invoice/receipt bằng AnalyzeExpense hoặc AnalyzeDocument/StartExpenseAnalysis tùy định dạng và cách triển khai.
- Amazon Bedrock nhận output Textract để normalize field name, classify invoice/receipt, enrich contextual fields và trả JSON thống nhất.
- Lambda validation layer kiểm tra schema, business rules và confidence score.
- DynamoDB lưu metadata và trạng thái; S3 processed lưu JSON/CSV kết quả.
- CloudWatch Logs/Metrics/Alarms và Amazon SNS gửi cảnh báo khi lỗi hoặc confidence thấp.

#### 3.2 Ngoài phạm vi MVP

- Không xử lý tất cả loại tài liệu ngay từ đầu; MVP chỉ nhận invoice và receipt.
- Không dùng EC2 cho compute xử lý chính.
- Không làm ingestion từ hệ thống bên ngoài trong MVP; upload trực tiếp bằng presigned URL đủ cho workshop.

#### 3.3 Tiêu chí thành công

| Tiêu chí | Mức đạt yêu cầu |
| --- | --- |
| End-to-end demo | Login, upload, processing, extraction, status/result display, alert, cleanup chạy được |
| Extraction quality | Tối thiểu vendor, invoice date, total amount, currency, tax, line items nếu có |
| Reliability | Có retry, DLQ hoặc failure path; status không bị treo vô hạn |
| Observability | Có log, metric, alarm, Step Functions execution history và bằng chứng test |
| Security | Không public bucket; không hard-code access key; IAM role theo least privilege |
| Cost control | Có giới hạn file mẫu, lifecycle policy, cleanup script và cảnh báo chi phí cơ bản |

### 4. Kiến trúc giải pháp

Kiến trúc gom giải pháp thành các khối chính: Frontend & Access, API, Storage & Ingestion, Processing Workflow, Data & Results, Manual Review & Notifications, Observability/Operations và Security/Governance. File được upload trực tiếp lên S3 bằng presigned URL và xử lý bất đồng bộ qua EventBridge, SQS, Lambda, Step Functions, Amazon Textract, Amazon Bedrock và Lambda validation.

![Kiến trúc high-level của DocuFlow AI](/images/2-Proposal/docuflow_high_level_architecture.png)

```text
[User] -> [Route 53 / CloudFront + WAF] -> [Amplify + Cognito]
       -> [API Gateway] -> [Lambda Business APIs]
       -> [S3 Raw via presigned URL] -> [EventBridge] -> [SQS]
       -> [Lambda Job Starter] -> [Step Functions]
       -> [Textract] -> [Amazon Bedrock] -> [Validation]
       -> [DynamoDB + S3 Processed]
       -> [SNS / Manual Review if needed]
```

### 5. Lựa chọn dịch vụ AWS và lý do

| Layer | AWS services | Lý do |
| --- | --- | --- |
| Global Edge & Frontend/Auth | Route 53, CloudFront, AWS WAF, Amplify Hosting, Cognito | Tách frontend khỏi backend, có CDN/protection, login managed và boundary rõ giữa UI và API |
| API Layer | API Gateway REST API, Lambda Business APIs | API tạo `documentId`, ghi metadata ban đầu và trả presigned URL để browser upload trực tiếp lên S3 |
| Ingestion & Orchestration | S3 raw, EventBridge, SQS, Lambda Job Starter | S3 lưu bản gốc; EventBridge/SQS tách event khỏi processing; Lambda khởi động workflow ổn định |
| Processing Workflow | Step Functions, Textract, Amazon Bedrock, Lambda validation | Workflow validate file, gọi Textract, dùng Bedrock để normalize/enrich/classify output, sau đó validate schema/confidence |
| Data Stores | DynamoDB, S3 raw/processed/archive | DynamoDB lưu status/metadata cho frontend; S3 lưu raw/processed/audit data |
| Manual Review & Notifications | Reviewer UI, SNS, Review Update Lambda | Low-confidence item được đưa vào review loop; reviewer correction cập nhật lại status/result |
| Observability | CloudWatch Metrics/Logs/Alarms | Theo dõi execution, Lambda errors, SQS backlog, workflow failures và alert vận hành |
| Security & Governance | IAM, KMS, S3 Block Public Access, bucket policies | Áp dụng least privilege, encryption và bucket protection cho tài liệu tài chính |

### 6. Luồng xử lý end-to-end

1. User đăng nhập bằng Cognito; frontend nhận JWT token.
2. User chọn file invoice/receipt; frontend gọi API Gateway để xin presigned URL.
3. Lambda tạo `documentId`, ghi item DynamoDB với status `UPLOADED` và trả presigned URL.
4. Frontend upload file trực tiếp lên S3 raw bucket bằng presigned URL.
5. S3 Object Created event được gửi qua EventBridge, đẩy vào SQS processing queue.
6. Lambda Job Starter đọc queue và start Step Functions execution với `documentId`, bucket, key và `userId`.
7. Step Functions validate file type, size, metadata và cập nhật status `PROCESSING`.
8. Textract trích xuất text, key-value, summary fields và line items.
9. Amazon Bedrock nhận output Textract để normalize field name, classify invoice/receipt, enrich missing/contextual fields và trả JSON theo schema thống nhất.
10. Lambda validation kiểm tra JSON schema, business rules, `confidenceScore` và quyết định `EXTRACTED` hay `REVIEW_REQUIRED`.
11. Kết quả được lưu vào DynamoDB và S3 processed; frontend hiển thị status/result.
12. Nếu lỗi hoặc confidence thấp, CloudWatch alarm và SNS gửi cảnh báo cho reviewer/admin.
13. Reviewer chỉnh trường sai trong admin review page; update Lambda ghi corrected result và status `CORRECTED` hoặc `APPROVED`.

### 7. Phân công module cho nhóm 5 người

| Người | Module | AWS services sở hữu | Deliverable bắt buộc |
| --- | --- | --- | --- |
| 1 | Frontend, Auth & Upload | Route 53/CloudFront/WAF, Amplify, Cognito, API Gateway | Login, protected route, upload UI, status/result page, presigned URL flow |
| 2 | Ingestion & Workflow | S3 raw/processed, EventBridge, SQS, Lambda, Step Functions | Trigger workflow, retry/DLQ, status transition, failure handling, sample execution |
| 3 | Extraction, Bedrock Enrichment & Validation | Textract, Amazon Bedrock, Lambda | Extraction JSON, invoice/receipt field mapping, Bedrock prompt/schema, confidence score và validation logic |
| 4 | Data Storage & Dashboard | DynamoDB, S3 processed/archive/reports, frontend summary | Metadata table, processed JSON, status query và dashboard/report đơn giản trên frontend |
| 5 | Observability, Security & IaC | CloudWatch, SNS, IAM, KMS, S3 policies, CDK/SAM | Alarm, logs, metrics, SNS notification, least privilege policy, deploy/cleanup script |

{{% notice tip %}}
**Quy tắc tích hợp:** mỗi người có module riêng nhưng phải cùng dùng một `documentId`, status model, JSON schema và naming convention. Không được để từng module tự đặt field riêng.
{{% /notice %}}

### 8. Data contract và status model

Data contract là phần cần chốt trước khi chia việc. Đây là contract giữa frontend, workflow, extraction, DynamoDB, summary dashboard và reviewer UI.

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

| Status | Ý nghĩa | Ai/Service cập nhật |
| --- | --- | --- |
| `UPLOADED` | File đã upload vào S3 raw hoặc job đã được tạo | Lambda presigned URL / upload callback |
| `PROCESSING` | Workflow đang validate, OCR hoặc normalize | Step Functions / Lambda |
| `EXTRACTED` | Trích xuất thành công và confidence đạt threshold | Schema validation Lambda |
| `REVIEW_REQUIRED` | Thiếu field hoặc confidence thấp | Schema validation Lambda |
| `CORRECTED` | Reviewer đã chỉnh kết quả | Admin review page / update Lambda |
| `FAILED` | Workflow lỗi, file sai định dạng hoặc external service lỗi sau retry | Step Functions catch handler |

### 9. Bảo mật, IAM và compliance baseline

- S3 raw và processed buckets bật Block Public Access; frontend không dùng public write.
- Upload dùng presigned URL có TTL ngắn, giới hạn content type và prefix theo user/documentId.
- Cognito group tách `EndUser`, `Reviewer`, `Admin`; API Gateway dùng authorizer.
- Mỗi Lambda/Step Functions state dùng IAM role với least privilege theo bucket/table/queue cụ thể.
- Dữ liệu lưu ở S3 và DynamoDB nên mã hóa bằng SSE-S3 hoặc KMS; log không ghi toàn bộ nội dung chứng từ nhạy cảm.
- Không hard-code access key trong code, repo hoặc frontend; mọi quyền truy cập dùng IAM role và policy theo least privilege.
- CloudFront có thể kết hợp WAF để giảm request bất thường; bucket origin dùng OAC/OAI nếu triển khai S3 static web.

### 10. Observability, notification và vận hành

Module vận hành không phải phần phụ. Đây là bằng chứng project có thể chạy, debug và kiểm soát lỗi trong workshop.

| Hạng mục | Metric/log/alert đề xuất | Mục đích |
| --- | --- | --- |
| Step Functions | `ExecutionStarted`, `ExecutionFailed`, `ExecutionSucceeded`, duration | Audit từng chứng từ và debug state lỗi |
| Lambda | Errors, Throttles, Duration, IteratorAge nếu có queue | Phát hiện lỗi function, timeout, retry bất thường |
| SQS | ApproximateNumberOfMessagesVisible, DLQ depth | Phát hiện backlog hoặc message lỗi lặp lại |
| Textract + Amazon Bedrock | Request count, error count, latency, token usage, confidence distribution | Kiểm soát chất lượng extraction và chi phí AI |
| DynamoDB | Consumed capacity, throttled requests, item status distribution | Theo dõi metadata/status store |
| SNS | Notification delivery status | Bảo đảm reviewer nhận cảnh báo lỗi hoặc low confidence |

Alarm đề xuất:

- Step Functions failed execution > 0 trong 5 phút.
- DLQ visible messages > 0.
- Lambda error rate vượt ngưỡng.
- Tỷ lệ `REVIEW_REQUIRED` vượt 30% trong batch demo.

### 11. Analytics và reporting

Analytics trong MVP chỉ dùng DynamoDB, S3 processed JSON và dashboard/summary đơn giản trên frontend.

| Mức | Thành phần | Output demo |
| --- | --- | --- |
| MVP | DynamoDB + S3 processed JSON + frontend summary/dashboard | Tổng số chứng từ, tổng tiền, số lỗi, top vendor, danh sách item cần review |

### 12. Lộ trình triển khai

| Giai đoạn | Thời lượng | Mục tiêu | Output |
| --- | --- | --- | --- |
| Phase 0 - Design lock | Tuần 1 | Chốt MVP, data contract, service boundary, naming convention | Architecture diagram, JSON schema, module owner list |
| Phase 1 - Foundation | Tuần 2-3 | Tạo IaC base, S3, Cognito, API Gateway, Lambda upload | User login + upload file vào S3 raw |
| Phase 2 - Workflow | Tuần 4-5 | EventBridge, SQS, Step Functions, DynamoDB status | Workflow chạy từ upload tới `PROCESSING`/`FAILED` |
| Phase 3 - Extraction & enrichment | Tuần 6-7 | Textract + Bedrock enrichment + schema validation | Extracted JSON và confidence score |
| Phase 4 - Result UI & review | Tuần 8 | Result page, review page, correction flow | User xem kết quả; reviewer chỉnh field |
| Phase 5 - Observability & MVP dashboard | Tuần 9-10 | CloudWatch alarm, SNS, DynamoDB/S3 processed summary | Dashboard vận hành và dashboard/summary đơn giản |
| Phase 6 - Testing & documentation | Tuần 11 | Test case, screenshots, workshop steps | Lab guide, expected result, failure test |
| Phase 7 - Final polish | Tuần 12 | Cleanup, cost review, bilingual report | Final demo + report website |

### 13. Cấu trúc workshop dự kiến

| Section | Nội dung nên viết | Bằng chứng cần có |
| --- | --- | --- |
| 5.1 Introduction | Bài toán document processing cho SME Finance/Ops, kiến trúc tổng quan | Mục tiêu, architecture diagram |
| 5.2 Prerequisite | AWS account, region, IAM permissions, CLI, CDK/SAM, sample invoice PDF/JPG | Checklist môi trường |
| 5.3 Frontend & Auth | Triển khai Amplify/S3 static, Cognito, API Gateway authorizer | Screenshot login và protected page |
| 5.4 Ingestion & Workflow | S3 raw, EventBridge, SQS, Step Functions, DynamoDB status | Execution history, queue/DLQ setup |
| 5.5 AI Extraction | Textract, Bedrock enrichment, schema mapping, confidence calculation | Extracted JSON mẫu |
| 5.6 Data & Analytics | DynamoDB, S3 processed, frontend summary/dashboard | Metadata/status query, processed JSON, dashboard đơn giản |
| 5.7 Observability & Cleanup | CloudWatch logs, metrics, alarms, SNS, cleanup script | Alarm test, cleanup commands |

### 14. Kế hoạch kiểm thử và xác thực

| Test case | Input | Expected result |
| --- | --- | --- |
| Happy path invoice | Invoice PDF rõ nét | Status `EXTRACTED`; vendor/date/total/currency/line items có dữ liệu |
| Receipt low quality | Receipt mờ hoặc thiếu field | Status `REVIEW_REQUIRED`; SNS alert gửi reviewer |
| Invalid file type | File `.exe` hoặc file quá size | Reject hoặc status `FAILED`; không gọi Textract |
| Unauthenticated upload | User chưa login | Không lấy được presigned URL |
| Batch upload | 5-10 files liên tiếp | SQS/Step Functions xử lý không mất job; status cập nhật đúng |
| Textract + Bedrock failure | Giả lập lỗi, quota hoặc service error | Retry/catch hoạt động; DLQ hoặc `FAILED` path có log |
| Analytics query | Processed JSON sample + DynamoDB metadata | Frontend summary/dashboard trả tổng tiền theo vendor/tháng hoặc trạng thái |
| Cleanup | Chạy destroy/delete script | Stack, bucket objects, alarms và dữ liệu demo được xóa sau workshop |

### 15. Ước tính ngân sách

Có thể tạo và cập nhật estimate chính thức bằng [AWS Pricing Calculator](https://calculator.aws/#/). Các con số dưới đây là ước tính làm việc cho MVP workshop và cần kiểm tra lại theo AWS Region được chọn trước khi nộp.

#### Giả định ngân sách

- Region: `ap-southeast-1` (Singapore), trừ khi nhóm chọn region khác khi triển khai.
- Workload MVP demo: 100 invoice/receipt mỗi tháng.
- Kích thước trung bình: 2 trang/tài liệu, tương đương khoảng 200 trang Textract mỗi tháng.
- Bedrock enrichment: 100 request/tháng, khoảng 2.000 input tokens và 500 output tokens cho mỗi document.
- Lưu trữ file: tối đa 1 GB raw documents và processed JSON/CSV trong thời gian workshop.
- Người dùng: 5-10 user demo/workshop đăng nhập bằng Cognito.

#### Chi phí hạ tầng

| AWS service | Giả định sử dụng MVP | Chi phí ước tính mỗi tháng |
| --- | --- | --- |
| Amazon Textract | AnalyzeExpense khoảng 200 trang/tháng | Khoảng $2.00 |
| Amazon Bedrock | Khoảng 100 enrichment requests/tháng với small text model và token usage được giới hạn | Khoảng $0.10-$1.00 |
| AWS Lambda | Upload API, workflow starter, validation, review update; request thấp | Khoảng $0.00-$0.10 |
| Amazon API Gateway | Vài nghìn REST API calls/tháng | Khoảng $0.01 |
| Amazon S3 | Khoảng 1 GB raw + processed data, request thấp | Khoảng $0.03-$0.10 |
| Amazon DynamoDB | On-demand metadata/status table, read/write thấp | Khoảng $0.05-$0.25 |
| Amazon EventBridge + Amazon SQS | ObjectCreated events và processing queue cho demo volume | Khoảng $0.00-$0.10 |
| AWS Step Functions Standard | Khoảng 100 executions/tháng, vài state cho mỗi document | Khoảng $0.00-$0.10 |
| Amazon CloudWatch | Logs, metrics và basic alarms cho demo workload | Khoảng $0.50-$1.00 |
| Amazon SNS | Alert cho reviewer/admin, volume thấp | Khoảng $0.00-$0.10 |
| Amazon Cognito | 5-10 monthly active users | Khoảng $0.00 |
| AWS Amplify Hosting + Amazon CloudFront | Frontend build/hosting traffic nhỏ cho workshop demo | Khoảng $0.50-$2.00 |

**Tổng MVP ước tính:** khoảng **$3.50-$7.00/tháng**, tương đương **$42.00-$84.00 cho 12 tháng**, tùy region, model Bedrock được chọn, token usage, cách hosting frontend và log retention.

### 16. Kiểm soát chi phí và cleanup

#### 16.1 Quyết định kiểm soát chi phí cho MVP

{{% notice info %}}
**Cost-control decision:** MVP chỉ xử lý invoice và receipt. Nhóm chỉ triển khai các dịch vụ core cần cho luồng login -> upload -> extraction -> storage -> result/status -> alert -> manual review.
{{% /notice %}}

| Nhóm hạng mục | Quyết định MVP | Ghi chú kiểm soát chi phí |
| --- | --- | --- |
| Document types | Chỉ dùng invoice và receipt rõ nét | Giữ số lượng sample nhỏ khi demo |
| AI services | Textract + Amazon Bedrock + Lambda schema validation | Giới hạn số trang, request Bedrock và token usage |
| Analytics | DynamoDB + S3 processed + frontend summary/dashboard đơn giản | Không thêm BI service, chỉ đọc dữ liệu processed demo |
| Operations | CloudWatch Logs/Metrics/Alarms + SNS | Giữ log retention ngắn và alert volume thấp |

- Giới hạn file demo: 5-10 invoice/receipt, kích thước nhỏ, không upload batch lớn khi chưa có budget alert.
- S3 raw/processed đặt lifecycle policy chuyển hoặc xóa dữ liệu demo sau thời gian ngắn.
- Textract tính theo request/trang; Bedrock tính theo model/token usage. Cần giới hạn số request, số trang, token usage và ghi rõ Textract API cùng Bedrock model được dùng.
- Không chạy batch lớn ngoài dữ liệu demo; mọi file test cần nằm trong prefix riêng để dễ cleanup.
- Dashboard MVP hiển thị trực tiếp từ DynamoDB/S3 processed qua API, không cần thêm tầng BI riêng.
- CDK/SAM phải có lệnh deploy và destroy; với S3 bucket cần xóa object trước khi destroy stack.

### 17. Rủi ro và hướng giảm thiểu

| Risk | Impact | Mitigation |
| --- | --- | --- |
| Scope quá rộng | Không hoàn thành end-to-end | Chốt invoice/receipt MVP và không thêm service ngoài core |
| Textract đọc sai format lạ | Sai dữ liệu tài chính | Dùng sample rõ, threshold confidence, manual review loop |
| Bedrock hoặc schema mapping trả JSON sai | Frontend/analytics lỗi | Prompt/schema chặt, Lambda validate output, retry/fallback |
| Analytics service vượt MVP | Tăng chi phí và trễ demo | Dùng DynamoDB/S3 processed + frontend dashboard đơn giản |
| IAM quá rộng | Rủi ro bảo mật và bị trừ điểm | Mỗi module role riêng, least privilege, không hard-code key |
| Async workflow khó demo | User không thấy kết quả ngay | Status polling, execution history, sample dataset ổn định |
| Chi phí AI vượt dự kiến | Tốn ngân sách | Giới hạn trang/file, budget alert, cleanup sau demo |

### 18. Definition of Done

- Có architecture diagram rõ service AWS và luồng dữ liệu.
- Có 5 module owner rõ ràng, mỗi người có deliverable chấm được.
- Có upload thật lên S3 raw và trigger workflow thật.
- Có ít nhất một file invoice/receipt được Textract, Bedrock và Lambda validation xử lý ra JSON.
- Có DynamoDB status table và frontend/result view hoặc API để xem kết quả.
- Có failure path: invalid file hoặc low confidence tạo `REVIEW_REQUIRED`/`FAILED`.
- Có CloudWatch log, metric/alarm và SNS notification.
- Có test cases với expected result và screenshot/log bằng chứng.
- Có cost control, cleanup steps và không còn resource đắt tiền sau demo.
- Báo cáo/workshop có tiếng Việt và tiếng Anh ở phần nội dung chính nếu nộp theo template FCAJ.

### 19. Tài liệu tham khảo

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
