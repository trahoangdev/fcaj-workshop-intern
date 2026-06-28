---
title: "Tổng quan Workshop"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Bạn sẽ xây gì

Trong workshop này bạn xây dựng **DocuFlow AI**, một nền tảng serverless biến invoice và receipt được upload thành dữ liệu có cấu trúc, tìm kiếm được, với ít thao tác thủ công nhất. Người dùng đã đăng nhập upload file PDF, JPG hoặc PNG; nền tảng trích xuất vendor, date, tax, totals và line items; kết quả được lưu lại, theo dõi theo status và hiển thị trên dashboard web kèm đường review thủ công cho tài liệu confidence thấp.

Hệ thống hoàn toàn serverless và event-driven. Không có server phải quản lý, chỉ trả tiền cho phần xử lý thực tế, và toàn bộ nền tảng được định nghĩa bằng một AWS SAM stack duy nhất deploy ở `ap-southeast-1` (Singapore).

#### Kiến trúc

![Kiến trúc high-level của DocuFlow AI](/images/2-Proposal/docuflow_high_level_architecture.png)

Nền tảng được tổ chức theo các layer:

- **Identity & Frontend** - Amazon Cognito xác thực người dùng; React single-page app được phân phối qua Amazon CloudFront và deploy bằng AWS Amplify Hosting.
- **API & Compute** - Amazon API Gateway cung cấp REST endpoint backed bởi các AWS Lambda function.
- **Storage** - Amazon S3 giữ raw uploads và processed output; Amazon DynamoDB lưu metadata và status của tài liệu.
- **Ingestion & Workflow** - Amazon EventBridge và Amazon SQS tách upload khỏi processing; AWS Step Functions điều phối pipeline.
- **AI Extraction** - Amazon Textract đọc tài liệu; AI Proxy Lambda gọi External AI API để chuẩn hóa output về một JSON schema thống nhất.
- **Observability & Notification** - Amazon CloudWatch và AWS X-Ray cung cấp logs, metrics, alarms và traces; Amazon SNS kích hoạt Amazon SES email notifications.

#### Luồng xử lý end-to-end

Một tài liệu đi qua nền tảng như sau:

1. Người dùng đăng nhập bằng Cognito và xin một upload slot.
2. Một Lambda trả S3 presigned URL ngắn hạn và ghi DynamoDB item ban đầu với status `UPLOADED`.
3. Browser upload file trực tiếp lên S3; document bytes không đi qua API.
4. S3 phát event sang EventBridge, route qua SQS; status chuyển sang `QUEUED`.
5. Job Starter Lambda start một Step Functions execution; workflow validate input, gọi Textract, rồi gọi AI Proxy Lambda để normalize bằng External AI API.
6. Confidence + Status Lambda lưu metadata vào DynamoDB và processed JSON vào S3 với status `EXTRACTED`, `REVIEW_REQUIRED` hoặc `FAILED`.
7. Khi confidence thấp hoặc lỗi, SNS và SES thông báo reviewer, người này có thể sửa field và chuyển status sang `CORRECTED` hoặc `APPROVED`.

#### Các module Workshop

Bạn xây nền tảng theo từng bước. Mỗi module thêm resource vào cùng một SAM stack.

| Module | Trọng tâm | Dịch vụ chính được thêm |
|---|---|---|
| 5.1 | Tổng quan (trang này) | — |
| 5.2 | Chuẩn bị | Tooling, AWS profile, region, budget guardrails |
| 5.3 | Frontend, Auth, Upload | CloudFront, Amplify Hosting, Cognito, API Gateway, presign Lambda |
| 5.4 | Storage, Ingestion, Workflow | S3 buckets, EventBridge, SQS + DLQ, Step Functions |
| 5.5 | AI Extraction | Textract, AI Proxy Lambda, External AI API, confidence/status Lambda |
| 5.6 | Data, Result, Review | DynamoDB, API Gateway, review Lambdas |
| 5.7 | Observability & Security | CloudWatch, X-Ray, SNS/SES, IAM, KMS, Secrets Manager, CloudTrail, Budgets |
| 5.8 | Cleanup | Teardown stack và kiểm tra |

#### Workshop dành cho ai

Workshop phù hợp cho nhóm 5 người, mỗi người có thể phụ trách một hoặc hai module. Yêu cầu quen cơ bản với AWS console, command line và JavaScript hoặc TypeScript. Không cần kinh nghiệm sâu trước về dịch vụ serverless, mỗi module đều giải thích các dịch vụ mà nó giới thiệu.

#### Chi phí và thời gian

Workload workshop tốn khoảng **$3.50 - $8.00 mỗi tháng** ở `ap-southeast-1`, và ít hơn nhiều trên tài khoản mới nơi AWS Free Tier bao phủ hầu hết dịch vụ. Dự kiến khoảng 10 đến 12 tuần cho một nhóm xây và document toàn bộ module end-to-end. Nhớ chạy cleanup ở module 5.8 để tránh phát sinh chi phí.

Khi đã sẵn sàng, tiếp tục sang **5.2 Chuẩn bị** để thiết lập môi trường.
