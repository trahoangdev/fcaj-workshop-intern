---
title: "Tài liệu tham khảo"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

Trang này tập hợp các nguồn tham khảo được sử dụng xuyên suốt dự án **DocuFlow AI**.

### 1. Mã nguồn và bản demo

* [Mã nguồn dự án: AeroOps-AWS-FCAJ/aws-serverless-document-processing-workshop](https://github.com/AeroOps-AWS-FCAJ/aws-serverless-document-processing-workshop)
* [Bản demo DocuFlow AI trên AWS Amplify](https://main.dm2pgk3pjz05m.amplifyapp.com/)
* **Tài khoản demo:** `ziflapa@cuaks.fun` | `Tra_190704`

### 2. Kiến trúc và thực hành tốt trên AWS

* [AWS Architecture Center](https://aws.amazon.com/architecture/)
* [AWS Serverless](https://aws.amazon.com/serverless/)
* [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
* [Serverless Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)
* [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

### 3. Dịch vụ AWS sử dụng trong dự án

* [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html), [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html), [S3 EventBridge Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html), [quy tắc đặt tên S3 object key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html) và [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
* [CloudFront Origin Access Control](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
* [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) và [DynamoDB Secondary Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html)
* [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) và [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)
* [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) và [API Gateway Cognito Authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)
* [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) và [Lambda với Node.js](https://docs.aws.amazon.com/lambda/latest/dg/lambda-nodejs.html)
* [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) và [SQS Dead-Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)
* [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html), [chọn loại workflow](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html) và [xử lý lỗi](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html)
* [Amazon Textract](https://docs.aws.amazon.com/textract/latest/dg/what-is.html), [Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/invoices-receipts.html) và [AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html)
* [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
* [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) và [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
* [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) và [Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

### 4. Phát triển và triển khai

* [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html), [SAM CLI Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) và [SAM CLI Delete](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-delete.html)
* [Bắt đầu triển khai ứng dụng với Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/getting-started.html)
* [Thiết lập Amplify truy cập repository GitHub](https://docs.aws.amazon.com/amplify/latest/userguide/setting-up-GitHub-access.html)
* [Cấu hình build settings cho ứng dụng Amplify](https://docs.aws.amazon.com/amplify/latest/userguide/build-settings.html)
* [Thiết lập biến môi trường cho Amplify](https://docs.aws.amazon.com/amplify/latest/userguide/setting-env-vars.html)
* [OpenAI API keys](https://platform.openai.com/api-keys)
* [pnpm workspaces](https://pnpm.io/workspaces) và [amazon-cognito-identity-js](https://www.npmjs.com/package/amazon-cognito-identity-js)

### 5. Chi phí và dọn dẹp tài nguyên

* [AWS Pricing Calculator](https://calculator.aws/)
* [AWS Free Tier](https://aws.amazon.com/free/)
* [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/), [Amazon Textract Pricing](https://aws.amazon.com/textract/pricing/) và [AWS Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/)
* [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) và [AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html)
* [Secrets Manager Pricing](https://aws.amazon.com/secrets-manager/pricing/)
* [Hướng dẫn kiểm tra và xóa tài nguyên AWS](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/learn-whats-running-and-delete-resources.html)
