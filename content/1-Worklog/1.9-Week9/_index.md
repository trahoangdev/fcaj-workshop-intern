---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 9 Objectives:

* Stand up the **DocuFlow AI** project foundation as the baseline for all later modules.
* Build the identity, storage, and upload layers: Cognito, S3 buckets, DynamoDB, API Gateway, and the upload Lambda.
* Deliver a working secure upload flow from the frontend to the S3 raw bucket via presigned URL.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Set up the project foundation <br>&emsp; + Initialize the `docuflow-ai` monorepo (pnpm workspaces) <br>&emsp; + Scaffold the AWS SAM root template and `samconfig.toml` <br>&emsp; + Configure ESLint/Prettier and shared `tsconfig` | 06/15/2026 | 06/15/2026 | [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) <br> [pnpm workspaces](https://pnpm.io/workspaces) |
| 3 | - Build the identity layer <br>&emsp; + Create the Cognito User Pool with `end-user`, `reviewer`, `admin` groups <br>&emsp; + Configure the app client and hosted sign-in <br>&emsp; + Wire the Cognito authorizer for API Gateway | 06/16/2026 | 06/16/2026 | [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) <br> [API Gateway Cognito Authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html) |
| 4 | - Build the storage layer <br>&emsp; + Create `docuflow-raw` and `docuflow-processed` S3 buckets (Block Public Access, SSE-KMS) <br>&emsp; + Create the DynamoDB `Documents` table (PK `documentId`, GSI on `userId`+`status`) <br>&emsp; + Apply S3 lifecycle rules | 06/17/2026 | 06/17/2026 | [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) <br> [DynamoDB Secondary Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html) |
| 5 | - Build the upload API <br>&emsp; + Create the API Gateway REST API and `POST /uploads` route <br>&emsp; + Implement the `presignUpload` Lambda (Node.js 20.x) <br>&emsp; + Generate short-lived presigned URLs and write the `UPLOADED` item to DynamoDB | 06/18/2026 | 06/18/2026 | [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [Lambda Node.js](https://docs.aws.amazon.com/lambda/latest/dg/lambda-nodejs.html) |
| 6 | - Build frontend hosting and auth <br>&emsp; + Scaffold the React + Vite SPA <br>&emsp; + Host the build in a private S3 bucket served via CloudFront with Origin Access Control <br>&emsp; + Implement Cognito login and the upload page | 06/19/2026 | 06/19/2026 | [CloudFront Origin Access Control](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) <br> [amazon-cognito-identity-js](https://www.npmjs.com/package/amazon-cognito-identity-js) |
| 7 | - Integrate and verify the upload flow <br>&emsp; + Connect frontend → `POST /uploads` → presigned URL → S3 PUT <br>&emsp; + Confirm the `UPLOADED` status in DynamoDB <br>&emsp; + Deploy the consolidated SAM stack and capture evidence | 06/20/2026 | 06/20/2026 | [SAM CLI Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) |


### Week 9 Achievements:

* Stood up the **DocuFlow AI** project foundation:
  * Initialized the `docuflow-ai` monorepo with pnpm workspaces (`apps/`, `packages/`, `services/`, `infrastructure/`).
  * Scaffolded the AWS SAM root template with one stack per environment for repeatable deploy and clean teardown.
  * Configured shared ESLint, Prettier, and `tsconfig` across the workspace.

* Built the identity layer with Amazon Cognito:
  * Created the User Pool with `end-user`, `reviewer`, and `admin` groups.
  * Configured the app client and sign-in flow.
  * Wired the Cognito User Pool authorizer to protect API Gateway endpoints.

* Built the storage layer:
  * Created the `docuflow-raw` bucket (Block Public Access, SSE-KMS, 60-day expiry) and the `docuflow-processed` bucket (versioning, STANDARD-IA after 30 days).
  * Created the on-demand DynamoDB `Documents` table with PK `documentId` and a GSI on `userId`+`status` for the user dashboard.

* Built the secure upload API:
  * Created the API Gateway REST API with the `POST /uploads` route behind the Cognito authorizer.
  * Implemented the `presignUpload` Lambda (Node.js 20.x, 256 MB) that generates a `documentId`, writes the `UPLOADED` item to DynamoDB, and returns a short-lived presigned URL scoped to `docuflow-raw/{userId}/{documentId}.{ext}`.

* Built the frontend hosting and auth:
  * Scaffolded the React 18 + Vite + TypeScript SPA.
  * Hosted the build in a private S3 bucket served through CloudFront with Origin Access Control over HTTPS.
  * Implemented Cognito login and the file upload page.

* Verified the end-to-end upload flow:
  * Frontend requests an upload slot, receives a presigned URL, and PUTs the file directly to S3 without the bytes passing through API Gateway or Lambda.
  * Confirmed the `UPLOADED` status is recorded in DynamoDB.
  * Deployed the consolidated SAM stack and captured screenshots as evidence.
