---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 8:

* Tìm hiểu bài toán và phạm vi MVP cho dự án **DocuFlow AI** - nền tảng xử lý invoice/receipt thông minh trên AWS.
* Xác định các dịch vụ AWS phù hợp, chi phí vận hành, rủi ro kỹ thuật và kiến trúc tổng thể cho dự án.
* Hoàn thiện proposal ban đầu để làm cơ sở triển khai workshop và chia module cho nhóm.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu tổng quan dự án **DocuFlow AI** <br>&emsp; + Xác định pain point nhập liệu hóa đơn/biên nhận thủ công <br>&emsp; + Chốt phạm vi MVP tập trung vào invoice/receipt processing <br>&emsp; + Ghi nhận các đối tượng người dùng: EndUser, Reviewer, Admin | 08/06/2026 | 08/06/2026 | [Amazon Textract - Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/invoices-receipts.html) |
| 3 | - Tìm hiểu các dịch vụ AWS cần dùng cho dự án <br>&emsp; + Cognito, API Gateway, Lambda, S3 <br>&emsp; + EventBridge, SQS, Step Functions <br>&emsp; + Textract, Bedrock, DynamoDB <br>&emsp; + CloudWatch, SNS/SES, IAM/KMS | 09/06/2026 | 09/06/2026 | [Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) <br> [API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) <br> [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) <br> [S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) <br> [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) <br> [Textract](https://docs.aws.amazon.com/textract/latest/dg/what-is.html) <br> [Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) |
| 4 | - Phân tích chi phí cho dự án <br>&emsp; + Xác định các dịch vụ tính phí theo request/trang/token <br>&emsp; + Đánh giá chi phí Textract, Bedrock, Step Functions, S3, Lambda và CloudWatch <br>&emsp; + Đề xuất giới hạn file demo, budget alert và cleanup sau workshop | 10/06/2026 | 10/06/2026 | [AWS Pricing Calculator](https://calculator.aws/) <br> [Textract Pricing](https://aws.amazon.com/textract/pricing/) <br> [Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/) <br> [Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| 5 | - Phân tích rủi ro cho dự án <br>&emsp; + Scope quá rộng nếu xử lý mọi loại tài liệu <br>&emsp; + Textract đọc sai chứng từ chất lượng thấp <br>&emsp; + Bedrock trả JSON sai schema <br>&emsp; + IAM quá rộng hoặc chi phí AI vượt dự kiến | 11/06/2026 | 11/06/2026 | [Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) <br> [Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) <br> [IAM Security Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| 6 | - Vẽ kiến trúc giải pháp <br>&emsp; + Thiết kế luồng upload qua presigned URL vào S3 raw bucket <br>&emsp; + Thiết kế workflow EventBridge, SQS, Step Functions, Textract và Bedrock <br>&emsp; + Xác định luồng lưu metadata vào DynamoDB và kết quả vào S3 processed | 12/06/2026 | 12/06/2026 | [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [S3 EventBridge Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html) <br> [SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) <br> [Step Functions Workflow Type](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html) <br> [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) |
| 7 | - Làm proposal cho dự án <br>&emsp; + Viết executive summary, problem statement, MVP scope, success criteria <br>&emsp; + Mô tả architecture, service selection, workflow, risk và cost control <br>&emsp; + Chia module cho nhóm 5 người và định nghĩa deliverable | 13/06/2026 | 13/06/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [Serverless Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html) <br> [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) |


### Kết quả đạt được tuần 8:

* Nắm được bài toán nghiệp vụ của **DocuFlow AI**: giảm nhập liệu thủ công cho invoice/receipt, tăng khả năng theo dõi trạng thái, chuẩn hóa dữ liệu và hỗ trợ audit cho đội Finance/Ops.

* Chốt phạm vi MVP ở mức phù hợp với workshop:
  * Người dùng đăng nhập bằng Cognito.
  * Upload PDF/JPG/PNG qua frontend và presigned URL.
  * S3 raw bucket kích hoạt workflow xử lý bất đồng bộ.
  * Textract trích xuất dữ liệu chứng từ.
  * Bedrock chuẩn hóa, phân loại và trả output theo JSON schema.
  * DynamoDB lưu metadata/status, S3 processed lưu kết quả JSON/CSV.
  * CloudWatch và SNS/SES cảnh báo lỗi hoặc confidence thấp.

* Xác định danh sách dịch vụ AWS chính và vai trò của từng dịch vụ trong kiến trúc serverless, event-driven:
  * **Cognito** cho xác thực và phân quyền EndUser/Reviewer/Admin.
  * **API Gateway + Lambda** cho API tạo presigned URL, cập nhật trạng thái và xử lý logic nhẹ.
  * **S3** cho raw document và processed result.
  * **EventBridge + SQS + Step Functions** cho luồng xử lý bất đồng bộ, retry và audit execution.
  * **Textract + Bedrock** cho extraction, normalization, classification và schema output.
  * **DynamoDB** cho status table và metadata.
  * **CloudWatch + SNS/SES** cho log, metric, alarm và notification.
  * **IAM/KMS** cho least privilege và mã hóa dữ liệu.

* Phân tích chi phí và đề xuất các biện pháp kiểm soát:
  * Giới hạn demo ở 5-10 file invoice/receipt kích thước nhỏ.
  * Theo dõi các dịch vụ tính phí theo request, trang hoặc token như Textract, Bedrock và Step Functions.
  * Chỉ query dữ liệu processed khi dùng Athena để tránh tăng chi phí scan.
  * Đặt lifecycle policy cho S3 raw/processed và chuẩn bị lệnh cleanup/destroy sau demo.
  * Thiết lập budget alert để phát hiện chi phí bất thường.

* Lập ma trận rủi ro chính và hướng giảm thiểu:
  * Scope quá rộng: khóa MVP vào invoice/receipt, đưa contract, purchase order, email ingestion, QuickSight nâng cao thành extension.
  * OCR sai với chứng từ chất lượng thấp: dùng confidence score, ngưỡng REVIEW_REQUIRED và manual review loop.
  * Bedrock trả JSON sai schema: thiết kế prompt/schema rõ ràng, Lambda validate schema và retry/fallback.
  * IAM quá rộng: tách role theo module, áp dụng least privilege, không hard-code access key.
  * Chi phí AI vượt dự kiến: giới hạn file demo, bật cảnh báo ngân sách và cleanup sau workshop.

* Hoàn thành bản phác thảo kiến trúc end-to-end cho DocuFlow AI, gồm các lớp client/access, ingestion/workflow, AI processing, storage, analytics, observability và DevOps/IaC.

* Hoàn thành proposal ban đầu cho dự án với các nội dung chính:
  * Executive summary và problem statement.
  * MVP scope, out-of-scope và success criteria.
  * Solution architecture và service selection.
  * End-to-end workflow.
  * Data contract, status model và phân chia module cho nhóm 5 người.
  * Security baseline, observability, analytics, cost control, risk mitigation và definition of done.
