---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 9:

* Dựng nền tảng dự án **DocuFlow AI** làm baseline cho các module sau.
* Xây các layer identity, storage và upload: Cognito, S3 buckets, DynamoDB, API Gateway và Lambda upload.
* Hoàn thành luồng upload an toàn từ frontend lên S3 raw bucket qua presigned URL.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Thiết lập nền tảng dự án <br>&emsp; + Khởi tạo monorepo `docuflow-ai` (pnpm workspaces) <br>&emsp; + Dựng SAM root template và `samconfig.toml` <br>&emsp; + Cấu hình ESLint/Prettier và `tsconfig` dùng chung | 15/06/2026 | 15/06/2026 | [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) <br> [pnpm workspaces](https://pnpm.io/workspaces) |
| 3 | - Xây layer identity <br>&emsp; + Tạo Cognito User Pool với groups `end-user`, `reviewer`, `admin` <br>&emsp; + Cấu hình app client và hosted sign-in <br>&emsp; + Gắn Cognito authorizer cho API Gateway | 16/06/2026 | 16/06/2026 | [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) <br> [API Gateway Cognito Authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html) |
| 4 | - Xây layer storage <br>&emsp; + Tạo S3 bucket `docuflow-raw` và `docuflow-processed` (Block Public Access, SSE-KMS) <br>&emsp; + Tạo DynamoDB table `Documents` (PK `documentId`, GSI trên `userId`+`status`) <br>&emsp; + Áp dụng S3 lifecycle rules | 17/06/2026 | 17/06/2026 | [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) <br> [DynamoDB Secondary Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html) |
| 5 | - Xây upload API <br>&emsp; + Tạo API Gateway REST API và route `POST /uploads` <br>&emsp; + Viết Lambda `presignUpload` (Node.js 20.x) <br>&emsp; + Sinh presigned URL ngắn hạn và ghi item `UPLOADED` vào DynamoDB | 18/06/2026 | 18/06/2026 | [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [Lambda Node.js](https://docs.aws.amazon.com/lambda/latest/dg/lambda-nodejs.html) |
| 6 | - Xây frontend hosting và auth <br>&emsp; + Dựng SPA React + Vite <br>&emsp; + Host build trong S3 bucket private serve qua CloudFront với Origin Access Control <br>&emsp; + Làm Cognito login và trang upload | 19/06/2026 | 19/06/2026 | [CloudFront Origin Access Control](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) <br> [amazon-cognito-identity-js](https://www.npmjs.com/package/amazon-cognito-identity-js) |
| 7 | - Tích hợp và kiểm thử luồng upload <br>&emsp; + Nối frontend → `POST /uploads` → presigned URL → S3 PUT <br>&emsp; + Xác nhận status `UPLOADED` trong DynamoDB <br>&emsp; + Deploy SAM stack hợp nhất và thu thập evidence | 20/06/2026 | 20/06/2026 | [SAM CLI Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) |


### Kết quả đạt được tuần 9:

* Dựng xong nền tảng dự án **DocuFlow AI**:
  * Khởi tạo monorepo `docuflow-ai` với pnpm workspaces (`apps/`, `packages/`, `services/`, `infrastructure/`).
  * Dựng SAM root template với một stack cho mỗi environment để deploy tái lập và teardown sạch.
  * Cấu hình ESLint, Prettier và `tsconfig` dùng chung trên toàn workspace.

* Xây layer identity với Amazon Cognito:
  * Tạo User Pool với các groups `end-user`, `reviewer` và `admin`.
  * Cấu hình app client và luồng sign-in.
  * Gắn Cognito User Pool authorizer để bảo vệ các endpoint API Gateway.

* Xây layer storage:
  * Tạo bucket `docuflow-raw` (Block Public Access, SSE-KMS, expire sau 60 ngày) và bucket `docuflow-processed` (versioning, chuyển STANDARD-IA sau 30 ngày).
  * Tạo DynamoDB table `Documents` (on-demand) với PK `documentId` và GSI trên `userId`+`status` phục vụ dashboard người dùng.

* Xây upload API an toàn:
  * Tạo API Gateway REST API với route `POST /uploads` đặt sau Cognito authorizer.
  * Viết Lambda `presignUpload` (Node.js 20.x, 256 MB) sinh `documentId`, ghi item `UPLOADED` vào DynamoDB và trả presigned URL ngắn hạn theo path `docuflow-raw/{userId}/{documentId}.{ext}`.

* Xây frontend hosting và auth:
  * Dựng SPA React 18 + Vite + TypeScript.
  * Host build trong S3 bucket private serve qua CloudFront với Origin Access Control trên HTTPS.
  * Làm Cognito login và trang upload file.

* Kiểm thử luồng upload end-to-end:
  * Frontend xin upload slot, nhận presigned URL và PUT file trực tiếp lên S3 mà bytes không đi qua API Gateway hay Lambda.
  * Xác nhận status `UPLOADED` được ghi vào DynamoDB.
  * Deploy SAM stack hợp nhất và chụp screenshots làm evidence.
