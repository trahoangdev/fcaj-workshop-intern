---
title: "References"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

This page consolidates the references used throughout the **DocuFlow AI** project.

### 1. Source code and demo

* [Project source: AeroOps-AWS-FCAJ/aws-serverless-document-processing-workshop](https://github.com/AeroOps-AWS-FCAJ/aws-serverless-document-processing-workshop)
* [DocuFlow AI demo on AWS Amplify](https://main.dm2pgk3pjz05m.amplifyapp.com/)

### 2. AWS architecture and best practices

* [AWS Architecture Center](https://aws.amazon.com/architecture/)
* [AWS Serverless](https://aws.amazon.com/serverless/)
* [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
* [Serverless Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)
* [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

### 3. AWS services used by the project

* [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html), [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html), [S3 EventBridge Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html), [S3 object key naming guidelines](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html), and [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
* [CloudFront Origin Access Control](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
* [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) and [DynamoDB Secondary Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html)
* [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) and [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)
* [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) and [API Gateway Cognito Authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)
* [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) and [Lambda with Node.js](https://docs.aws.amazon.com/lambda/latest/dg/lambda-nodejs.html)
* [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) and [SQS Dead-Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)
* [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html), [choosing a workflow type](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html), and [error handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html)
* [Amazon Textract](https://docs.aws.amazon.com/textract/latest/dg/what-is.html), [Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/invoices-receipts.html), and [AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html)
* [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
* [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) and [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
* [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html), and [Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

### 4. Development and deployment

* [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html), [SAM CLI Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html), and [SAM CLI Delete](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-delete.html)
* [Getting started with deploying an app to Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/getting-started.html)
* [Setting up Amplify access to GitHub repositories](https://docs.aws.amazon.com/amplify/latest/userguide/setting-up-GitHub-access.html)
* [Configuring the build settings for an Amplify application](https://docs.aws.amazon.com/amplify/latest/userguide/build-settings.html)
* [Setting environment variables](https://docs.aws.amazon.com/amplify/latest/userguide/setting-env-vars.html)
* [OpenAI API keys](https://platform.openai.com/api-keys)
* [pnpm workspaces](https://pnpm.io/workspaces) and [amazon-cognito-identity-js](https://www.npmjs.com/package/amazon-cognito-identity-js)

### 5. Cost and resource cleanup

* [AWS Pricing Calculator](https://calculator.aws/)
* [AWS Free Tier](https://aws.amazon.com/free/)
* [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/), [Amazon Textract Pricing](https://aws.amazon.com/textract/pricing/), and [AWS Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/)
* [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) and [AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html)
* [Secrets Manager Pricing](https://aws.amazon.com/secrets-manager/pricing/)
* [Guidance for checking and deleting AWS resources](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/learn-whats-running-and-delete-resources.html)
