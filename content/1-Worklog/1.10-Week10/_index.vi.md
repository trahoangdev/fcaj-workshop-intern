---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 10:

* Điều chỉnh lại kiến trúc **DocuFlow AI** sau khi đánh giá Bedrock không phù hợp làm dependency bắt buộc cho môi trường Free Tier/workshop.
* Thay bước Bedrock bằng **External AI provider/API** được gọi qua Lambda adapter để vẫn giữ được luồng chuẩn hóa JSON sau Textract.
* Làm lại proposal và workshop để đồng bộ với kiến trúc mới, chi phí mới, rủi ro mới và các bước triển khai mới.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Rà soát lại ràng buộc Free Tier và quyền truy cập AI <br>&emsp; + Kiểm tra Bedrock pricing/model access <br>&emsp; + Xác định Bedrock không nên là service bắt buộc trong MVP <br>&emsp; + Chốt hướng chuyển sang external AI để giảm rủi ro account/policy | 22/06/2026 | 22/06/2026 | [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/) <br> [AWS Free Tier](https://aws.amazon.com/free/) |
| 3 | - Sửa lại kiến trúc hệ thống <br>&emsp; + Bỏ state Bedrock khỏi Step Functions workflow <br>&emsp; + Thêm Lambda `externalAiNormalize` gọi external AI API <br>&emsp; + Lưu API key bằng Secrets Manager, không hard-code trong code hoặc frontend <br>&emsp; + Cập nhật diagram và luồng dữ liệu mới | 23/06/2026 | 23/06/2026 | [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) <br> [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) <br> [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) |
| 4 | - Cập nhật data contract, status model và error handling <br>&emsp; + Giữ output JSON schema chung cho invoice/receipt <br>&emsp; + Thêm lỗi `EXTERNAL_AI_FAILED` hoặc map về `REVIEW_REQUIRED`/`FAILED` <br>&emsp; + Cập nhật retry/catch trong workflow <br>&emsp; + Điều chỉnh IAM role cho Lambda adapter | 24/06/2026 | 24/06/2026 | [Step Functions Error Handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html) <br> [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 5 | - Cập nhật chi phí và rủi ro <br>&emsp; + Loại Bedrock khỏi bảng cost baseline <br>&emsp; + Thêm chi phí external AI provider theo request/token <br>&emsp; + Bổ sung rủi ro API key, quota, latency, network timeout và dữ liệu nhạy cảm gửi ra ngoài AWS <br>&emsp; + Cập nhật cost control và cleanup checklist | 25/06/2026 | 25/06/2026 | [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) <br> [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) <br> [Secrets Manager Pricing](https://aws.amazon.com/secrets-manager/pricing/) |
| 6 | - Làm lại proposal theo kiến trúc mới <br>&emsp; + Sửa executive summary, architecture, service selection và workflow <br>&emsp; + Thay Bedrock bằng External AI Lambda Adapter <br>&emsp; + Cập nhật timeline, cost estimation, risk mitigation và definition of done | 26/06/2026 | 26/06/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [Serverless Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html) |
| 7 | - Làm lại workshop theo proposal mới <br>&emsp; + Cập nhật overview, prerequisite và module AI extraction <br>&emsp; + Viết lại bước cấu hình secret external AI <br>&emsp; + Cập nhật hướng dẫn deploy, test case, evidence và cleanup <br>&emsp; + Kiểm tra lại liên kết nội bộ của workshop | 27/06/2026 | 27/06/2026 | [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) <br> [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [Amazon Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) |


### Kết quả đạt được tuần 10:

* Đã xác định lại hướng kiến trúc cho **DocuFlow AI**:
  * Bedrock không còn là thành phần bắt buộc trong MVP vì rủi ro policy/model access/cost đối với tài khoản Free Tier hoặc tài khoản workshop.
  * Textract vẫn giữ vai trò trích xuất dữ liệu invoice/receipt.
  * Bước chuẩn hóa, phân loại và tạo JSON schema được chuyển sang external AI provider thông qua một Lambda adapter riêng.

* Cập nhật luồng xử lý mới:
  * User upload invoice/receipt lên S3 raw bằng presigned URL.
  * S3 Object Created event đi qua EventBridge và SQS.
  * Step Functions điều phối các bước validate file, gọi Textract, gọi `externalAiNormalize`, validate JSON, lưu kết quả và cập nhật status.
  * DynamoDB lưu metadata/status; S3 processed lưu JSON kết quả; CloudWatch và SNS xử lý log/alarm/notification.

* Thiết kế lại phần external AI integration:
  * Tạo Lambda adapter để cô lập logic gọi external AI API khỏi workflow chính.
  * API key được lưu trong AWS Secrets Manager, không đưa vào frontend, source code hoặc file cấu hình public.
  * Lambda adapter chịu trách nhiệm mask dữ liệu nhạy cảm nếu cần, giới hạn input length, timeout, retry và chuẩn hóa response về schema nội bộ.

* Cập nhật data contract và status model:
  * Giữ schema chung gồm `documentId`, `userId`, `fileName`, `documentType`, `vendorName`, `invoiceDate`, `currency`, `totalAmount`, `taxAmount`, `lineItems`, `confidenceScore`, `status`, `s3RawPath`, `s3ProcessedPath`.
  * Bổ sung handling cho lỗi external AI như timeout, quota exceeded, invalid JSON hoặc schema mismatch.
  * Các lỗi có thể chuyển sang `REVIEW_REQUIRED` nếu cần reviewer kiểm tra, hoặc `FAILED` nếu workflow không thể tiếp tục.

* Cập nhật phân tích chi phí:
  * Loại Bedrock khỏi baseline AWS cost.
  * Giữ các dịch vụ AWS chính: Cognito, S3, CloudFront, API Gateway, Lambda, EventBridge, SQS, Step Functions, Textract, DynamoDB, CloudWatch, SNS, Secrets Manager và SAM/CloudFormation.
  * Thêm chi phí external AI provider như một hạng mục riêng, phụ thuộc vào request/token và không tính là AWS Free Tier.
  * Bổ sung cost control: giới hạn số file demo, giới hạn số trang/file, giới hạn token đầu vào/đầu ra, budget alert và cleanup sau workshop.

* Cập nhật risk matrix:
  * Rủi ro external AI API key bị lộ được giảm bằng Secrets Manager và IAM least privilege.
  * Rủi ro quota/rate limit được giảm bằng retry có kiểm soát, timeout và fallback sang `REVIEW_REQUIRED`.
  * Rủi ro gửi dữ liệu nhạy cảm ra ngoài AWS được giảm bằng cách chỉ gửi field cần chuẩn hóa, mask dữ liệu nếu cần và ghi rõ giới hạn trong workshop.
  * Rủi ro latency/network timeout được giảm bằng Step Functions retry/catch và status rõ ràng.

* Làm lại proposal:
  * Cập nhật kiến trúc high-level và service selection.
  * Thay Bedrock bằng External AI Lambda Adapter.
  * Cập nhật workflow, cost estimation, risk mitigation, security baseline và definition of done.
  * Đồng bộ lại các phần bilingual để nội dung tiếng Việt và tiếng Anh nhất quán.

* Làm lại workshop:
  * Cập nhật overview và prerequisite theo kiến trúc mới.
  * Viết lại module AI extraction để dùng Textract + external AI adapter thay vì Textract + Bedrock.
  * Bổ sung bước cấu hình secret, deploy, test happy path, test low confidence/failure path và cleanup.
  * Rà soát lại checklist evidence để phục vụ báo cáo cuối kỳ.
