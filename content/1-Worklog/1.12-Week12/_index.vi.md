---
title: "Worklog Tuần 12 - Kiểm thử, hoàn thiện demo và tổng kết DocuFlow AI"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 12:

* Hoàn thiện các phần cuối của **DocuFlow AI** trước khi demo: workshop guide, evidence, test cases và final runbook.
* Kiểm thử end-to-end các luồng chính: login, upload, queue, workflow, Textract, AI Proxy, result storage, review, alert và cleanup.
* Chuẩn bị báo cáo cuối, demo script 7-10 phút và checklist xóa tài nguyên để kiểm soát chi phí.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Rà soát lại toàn bộ workshop guide <br>&emsp; + Kiểm tra overview, prerequisite, architecture, module AI extraction, observability và cleanup <br>&emsp; + Đồng bộ nội dung tiếng Việt/tiếng Anh <br>&emsp; + Kiểm tra link nội bộ, hình ảnh, lệnh deploy và expected output | 06/07/2026 | 06/07/2026 | [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) <br> [AWS Serverless](https://aws.amazon.com/serverless/) |
| 3 | - Kiểm thử happy path end-to-end <br>&emsp; + Login bằng Cognito user demo <br>&emsp; + Upload invoice rõ nét qua presigned URL vào S3 Raw <br>&emsp; + Xác nhận EventBridge, SQS và Step Functions chạy đúng <br>&emsp; + Kiểm tra Textract, AI Proxy, DynamoDB và S3 Processed result | 07/07/2026 | 07/07/2026 | [Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) <br> [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) |
| 4 | - Kiểm thử failure path và review path <br>&emsp; + Test file sai định dạng hoặc quá giới hạn <br>&emsp; + Test receipt mờ hoặc thiếu field để chuyển `REVIEW_REQUIRED` <br>&emsp; + Test external AI timeout/rate limit/invalid JSON <br>&emsp; + Kiểm tra reviewer sửa field và approve kết quả | 08/07/2026 | 08/07/2026 | [Step Functions Error Handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html) <br> [Amazon SQS DLQ](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) |
| 5 | - Thu thập evidence cho từng module <br>&emsp; + Screenshot frontend login/upload/list/detail/review <br>&emsp; + Screenshot S3 Raw, S3 Processed, DynamoDB item, Step Functions execution history, SQS/DLQ <br>&emsp; + Lưu CloudWatch logs, X-Ray trace, SNS/SES alert, Budget alert và Secrets Manager evidence | 09/07/2026 | 09/07/2026 | [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) <br> [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) <br> [SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) |
| 6 | - Chuẩn bị final demo và báo cáo cuối <br>&emsp; + Viết demo script 7-10 phút <br>&emsp; + Chốt thứ tự demo: problem, architecture, login, upload, workflow, result, review/alert, observability, cleanup <br>&emsp; + Rà soát Definition of Done và checklist từng thành viên | 10/07/2026 | 10/07/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [Amazon Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) |
| 7 | - Dọn dẹp tài nguyên và kiểm tra chi phí <br>&emsp; + Lập cleanup checklist thủ công theo từng service vì phần lớn tài nguyên được cấu hình trực tiếp trên AWS Console <br>&emsp; + Chỉ dùng `sam delete` cho tài nguyên nào được deploy bằng SAM/CloudFormation stack <br>&emsp; + Kiểm tra S3 objects, CloudWatch logs/alarms, SQS/DLQ, DynamoDB, Secrets Manager và SNS/SES <br>&emsp; + Kiểm tra AWS Budgets/Billing để bảo đảm không còn tài nguyên tốn phí ngoài dự kiến | 11/07/2026 | 11/07/2026 | [AWS Resource Cleanup Guidance](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/learn-whats-running-and-delete-resources.html) <br> [SAM CLI Delete](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-delete.html) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) <br> [AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html) |


### Kết quả đạt được tuần 12:

* Hoàn thiện nội dung workshop cuối cùng cho **DocuFlow AI**:
  * Cập nhật overview theo kiến trúc admin duyệt.
  * Đồng bộ prerequisite, deploy steps, AI Proxy setup, observability và cleanup.
  * Kiểm tra lại các link nội bộ, hình ảnh kiến trúc, lệnh mẫu và expected result.
  * Bảo đảm nội dung tiếng Việt và tiếng Anh nhất quán.

* Kiểm thử happy path end-to-end:
  * User login bằng Cognito và truy cập protected frontend.
  * Frontend gọi `POST /documents/upload-url`, nhận presigned URL và upload invoice lên S3 Raw.
  * S3 ObjectCreated event đi qua EventBridge, SQS và Job Starter Lambda.
  * Step Functions chạy workflow xử lý gồm validate, Textract AnalyzeExpense, AI Proxy Lambda, External AI API, validate JSON, confidence/status và persist result.
  * DynamoDB có metadata/status `EXTRACTED`; S3 Processed có `result.json` đúng path convention.

* Kiểm thử failure path và review path:
  * File sai định dạng hoặc metadata không hợp lệ được reject hoặc chuyển `FAILED`.
  * Receipt mờ/thiếu field được chuyển `REVIEW_REQUIRED` kèm `reviewReasons`.
  * External AI timeout/rate limit/invalid JSON được workflow retry/catch và log `errorType` rõ ràng.
  * Reviewer có thể sửa field và chuyển kết quả sang `CORRECTED` hoặc `APPROVED`.

* Thu thập evidence cho báo cáo:
  * Frontend: login, upload, document list, document detail, review page và error states.
  * Backend/workflow: S3 Raw, EventBridge rule, SQS/DLQ, Step Functions execution history, Textract output, AI Proxy logs.
  * Data: DynamoDB item, S3 Processed `result.json`, API response mẫu.
  * Ops/security: CloudWatch logs/alarms, X-Ray trace, SNS/SES email alert, Secrets Manager, IAM summary, CloudTrail, Budgets.

* Hoàn thiện demo script 7-10 phút:
  * Giới thiệu pain point xử lý invoice/receipt thủ công.
  * Trình bày kiến trúc tổng quan và phân công module.
  * Demo login, upload invoice, theo dõi workflow, xem kết quả và review case.
  * Trình bày observability, alerting, cost guardrails và cleanup.

* Rà soát Definition of Done:
  * Upload được ít nhất 3 sample invoice/receipt từ frontend.
  * Happy path, review path và failure path đều có evidence.
  * Textract extract được field chính; AI Proxy trả JSON đúng schema.
  * DynamoDB/S3 Processed lưu đúng data contract và path convention.
  * CloudWatch/SNS/SES/Secrets Manager/Budgets/SAM cleanup đều có bằng chứng.

* Hoàn thành cleanup và kiểm soát chi phí:
  * Chuẩn bị cleanup checklist thủ công theo từng dịch vụ do nhiều tài nguyên được tạo trực tiếp trên AWS Console.
  * Xác định rõ `sam delete` chỉ áp dụng cho tài nguyên nằm trong SAM/CloudFormation stack, không dùng để xóa tài nguyên tạo thủ công.
  * Kiểm tra bucket objects, queue, table, secret, alarm, log group và notification resource trên AWS Console.
  * Rà soát AWS Budgets/Billing để giảm rủi ro phát sinh chi phí sau demo.
