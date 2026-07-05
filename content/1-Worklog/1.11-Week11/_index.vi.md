---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 11:

* Chốt lại kiến trúc **DocuFlow AI** phiên bản admin duyệt: Textract + AI Proxy Lambda + External AI API, EventBridge + SQS + Step Functions, đầy đủ observability/security/governance.
* Cập nhật bảng phân công cho nhóm 5 người, đảm bảo mỗi service trong kiến trúc có owner, deliverable, evidence và cleanup responsibility.
* Hoàn thiện tài liệu triển khai: data contract, status model, API contract, workflow, RACI, timeline, test plan, demo script và Definition of Done.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Rà soát kiến trúc admin đã duyệt <br>&emsp; + Xác nhận luồng chính từ frontend đến workflow xử lý tài liệu <br>&emsp; + Kiểm tra các service còn nằm trong diagram: CloudFront/Amplify, Cognito, API Gateway, Lambda, S3, EventBridge, SQS/DLQ, Step Functions, Textract, AI Proxy, DynamoDB, CloudWatch/X-Ray, SNS/SES, IAM/KMS/Secrets Manager/CloudTrail/Budgets/SAM <br>&emsp; + Loại bỏ các service không còn thuộc kiến trúc hiện tại | 29/06/2026 | 29/06/2026 | [AWS Architecture Center](https://aws.amazon.com/architecture/) <br> [AWS Serverless](https://aws.amazon.com/serverless/) |
| 3 | - Cập nhật bảng phân công và service ownership map <br>&emsp; + Chia 5 module: Integration/Workflow, AI, Frontend/Auth, Data, Ops/Security/IaC <br>&emsp; + Gán owner cho từng service/component <br>&emsp; + Xác định deliverable, evidence và cleanup responsibility cho từng thành viên | 30/06/2026 | 30/06/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) |
| 4 | - Chốt data contract, status model và API contract <br>&emsp; + Chuẩn hóa JSON schema cho document result <br>&emsp; + Chốt status: `UPLOADED`, `QUEUED`, `PROCESSING`, `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED`, `CORRECTED`, `APPROVED` <br>&emsp; + Viết API contract cho upload-url, list documents, get document detail và review update | 01/07/2026 | 01/07/2026 | [API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) <br> [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) <br> [S3 Object Key Naming](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html) |
| 5 | - Thiết kế workflow xử lý end-to-end <br>&emsp; + Mô tả luồng S3 ObjectCreated -> EventBridge -> SQS/DLQ -> Job Starter Lambda -> Step Functions <br>&emsp; + Chốt state machine: validate, Textract, AI Proxy, validate JSON, confidence/status, save result, alert <br>&emsp; + Bổ sung retry/catch cho Textract, External AI timeout/rate limit, invalid JSON và DynamoDB/S3 write failure | 02/07/2026 | 02/07/2026 | [S3 EventBridge Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html) <br> [Amazon SQS DLQ](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) <br> [Step Functions Error Handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html) |
| 6 | - Cập nhật security, observability, governance và cost control <br>&emsp; + Thiết kế IAM least privilege, KMS encryption baseline và Secrets Manager cho external AI API key <br>&emsp; + Chốt CloudWatch logs/alarms, X-Ray trace, SNS/SES alert, CloudTrail audit và AWS Budgets <br>&emsp; + Viết checklist deploy/cleanup bằng AWS SAM | 03/07/2026 | 03/07/2026 | [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) <br> [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) <br> [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) |
| 7 | - Hoàn thiện test plan, demo script và Definition of Done <br>&emsp; + Viết test cases: login, upload invoice rõ, receipt mờ, file sai định dạng, external AI timeout/rate limit, invalid JSON, DLQ path và cleanup <br>&emsp; + Chuẩn bị demo script 7-10 phút <br>&emsp; + Chốt evidence checklist cho từng thành viên | 04/07/2026 | 04/07/2026 | [Amazon Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) <br> [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |


### Kết quả đạt được tuần 11:

* Chốt kiến trúc **DocuFlow AI** phiên bản admin duyệt:
  * User truy cập frontend qua CloudFront/Amplify.
  * Cognito đảm nhiệm login, JWT và protected route.
  * API Gateway + Lambda Presigned URL tạo `documentId`, metadata ban đầu và upload URL.
  * S3 Raw nhận file gốc; EventBridge route ObjectCreated event sang SQS; DLQ giữ message lỗi lặp lại.
  * Job Starter Lambda start Step Functions Standard Workflow.
  * Workflow gọi Validate Lambda, Textract AnalyzeExpense, AI Proxy Lambda, External AI API, Confidence/Status Lambda.
  * DynamoDB lưu metadata/status/job state; S3 Processed lưu `result.json`.
  * CloudWatch/X-Ray/SNS/SES phục vụ log, trace, alarm và notification.
  * IAM/KMS/Secrets Manager/CloudTrail/Budgets/SAM đảm nhiệm security, governance, cost control và deployment.

* Cập nhật bảng phân công nhóm 5 người:
  * **Hoàng Trọng Trà** - Leader / Integration Owner: S3 raw/processed, EventBridge, SQS/DLQ, Job Starter Lambda, Step Functions, Validate Lambda và integration test.
  * **Vũ Duy Tài** - AI Owner: Textract AnalyzeExpense, AI Proxy Lambda, External AI API normalization, confidence/status logic và JSON schema validation.
  * **Nguyễn Hữu Tịnh** - Frontend/Auth Owner: CloudFront/Amplify, Cognito, API Gateway integration, upload UI, document list/detail và review UI.
  * **Lâm Quang Lộc** - Data Owner: DynamoDB, S3 processed JSON, metadata schema, result management, review update và report đơn giản.
  * **Phạm Tùng Dương** - Ops/Security/IaC Owner: IAM, KMS, Secrets Manager, CloudTrail, Budgets, SAM, CloudWatch, X-Ray, SNS/SES và cleanup.

* Chốt trách nhiệm chung bắt buộc:
  * Không tự ý thêm/bớt service ngoài kiến trúc admin đã duyệt nếu chưa thống nhất lại trong team.
  * Data contract, status model, API contract và S3 key naming phải được dùng chung giữa frontend, workflow, AI và data module.
  * External AI API key chỉ nằm trong Secrets Manager; frontend không gọi external AI trực tiếp; AI Proxy Lambda là lớp duy nhất gọi HTTPS ra external API.
  * Log cần có `documentId`, `userId`, `status`, `stateName` và `errorType` nhưng không log secret hoặc payload nhạy cảm.

* Hoàn thiện data contract và status model:
  * Data contract gồm `documentId`, `userId`, `fileName`, `documentType`, `status`, `vendorName`, `invoiceDate`, `currency`, `totalAmount`, `taxAmount`, `confidenceScore`, `reviewReasons`, `aiProvider`, `normalizationMethod`, `s3RawPath`, `s3ProcessedPath`, `createdAt`, `updatedAt`.
  * Status flow gồm `UPLOADED`, `QUEUED`, `PROCESSING`, `EXTRACTED`, `REVIEW_REQUIRED`, `FAILED`, `CORRECTED`, `APPROVED`.
  * S3 key naming được chốt theo hướng `raw/{userId}/{documentId}/original` và `processed/{userId}/{documentId}/result.json`.

* Hoàn thiện API contract sơ bộ:
  * `POST /documents/upload-url` để tạo presigned URL.
  * `GET /documents` để list documents theo user/status.
  * `GET /documents/{documentId}` để xem status và extracted fields.
  * `POST /documents/{documentId}/review` để sửa hoặc approve kết quả sau review.

* Chốt workflow Step Functions:
  * `ValidateInput` -> `UpdateStatusProcessing` -> `RunTextractAnalyzeExpense` -> `ParseTextractOutput` -> `CallAIProxyLambda` -> `ValidateNormalizedJson` -> `CalculateConfidenceAndStatus` -> `SaveMetadataToDynamoDB` -> `SaveProcessedJsonToS3` -> `PublishSNSAlert` nếu cần.
  * Catch path cập nhật status `FAILED`, gửi alert và giữ log/evidence để debug.
  * Các lỗi chính cần xử lý: file sai định dạng/kích thước, Textract throttle/error, external AI timeout/rate limit/invalid API key, invalid JSON/missing fields, DynamoDB/S3 write failure và SNS/SES issue.

* Cập nhật test plan và demo script:
  * Test happy path với invoice rõ nét: upload thành công, workflow chạy, status `EXTRACTED`, DynamoDB/S3 Processed có kết quả.
  * Test receipt mờ hoặc thiếu field: status `REVIEW_REQUIRED` và có alert cho reviewer/admin.
  * Test file sai định dạng: reject hoặc chuyển `FAILED`.
  * Test external AI timeout/rate limit và invalid JSON: retry/catch hoạt động, log rõ `errorType`.
  * Test DLQ path và cleanup bằng SAM delete.

* Chốt Definition of Done:
  * Upload được ít nhất 3 sample invoice/receipt từ frontend sau khi login.
  * EventBridge, SQS/DLQ và Step Functions chạy được cả happy path và failure path.
  * Textract extract được field chính; AI Proxy Lambda gọi external AI và trả JSON hợp lệ.
  * DynamoDB và S3 Processed lưu đúng schema/path convention.
  * Frontend hiển thị list/detail/result/review flow.
  * CloudWatch logs, alarms, SNS/SES alert, Secrets Manager, IAM, CloudTrail, Budgets và cleanup đều có evidence.
