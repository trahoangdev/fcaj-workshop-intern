---
title: "Workshop"
date: 2026-04-20
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# DocuFlow AI - Xây dựng nền tảng xử lý invoice & receipt serverless

#### Tổng quan

Workshop này xây dựng **DocuFlow AI**, một nền tảng serverless cho phép người dùng đã đăng nhập upload invoice và receipt, sau đó tự động trích xuất dữ liệu tài chính bằng Amazon Textract và Amazon Bedrock. Kết quả được lưu lại, theo dõi theo status và hiển thị trên dashboard web kèm review loop thủ công cho tài liệu có confidence thấp.

Bạn sẽ xây hệ thống theo từng bước, mỗi module một phần. Mỗi module thêm resource vào một AWS SAM stack duy nhất deploy ở `ap-southeast-1` (Singapore), nên nền tảng lớn dần từ một template rỗng thành một pipeline event-driven hoàn chỉnh.

#### Kiến trúc

Nền tảng dùng kiến trúc serverless, event-driven: Amazon Cognito cho auth, Amazon S3 + CloudFront cho frontend, API Gateway và Lambda cho API, EventBridge và SQS cho ingestion, Step Functions để điều phối, Textract và Bedrock cho AI extraction, DynamoDB cho metadata, và CloudWatch cùng SNS cho observability.

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequisite/)
3. [Frontend, Auth và Upload](5.3-Frontend-Auth-Upload/)
4. [Storage, Ingestion và Workflow](5.4-Storage-Ingestion-Workflow/)
5. [AI Extraction với Bedrock](5.5-AI-Extraction-Bedrock/)
6. [Data, Result và Review](5.6-Data-Result-Review/)
7. [Observability và Security](5.7-Observability-Security/)
8. [Cleanup](5.8-Cleanup/)
