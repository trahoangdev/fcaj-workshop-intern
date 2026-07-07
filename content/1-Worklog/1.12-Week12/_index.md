---
title: "Week 12 Worklog - DocuFlow AI Testing, Final Demo, and Wrap-up"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 12 Objectives:

* Finalize the last parts of **DocuFlow AI** before the demo: workshop guide, evidence, test cases, and final runbook.
* Run end-to-end tests for the main flows: login, upload, queue, workflow, Textract, AI Proxy, result storage, review, alert, and cleanup.
* Prepare the final report, 7-10 minute demo script, and resource cleanup checklist to control cost.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Review the full workshop guide <br>&emsp; + Check overview, prerequisite, architecture, AI extraction module, observability, and cleanup <br>&emsp; + Synchronize Vietnamese and English content <br>&emsp; + Check internal links, images, deploy commands, and expected output | 07/06/2026 | 07/06/2026 | [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) <br> [AWS Serverless](https://aws.amazon.com/serverless/) |
| 3 | - Test the end-to-end happy path <br>&emsp; + Log in with a demo Cognito user <br>&emsp; + Upload a clear invoice through a presigned URL into S3 Raw <br>&emsp; + Confirm EventBridge, SQS, and Step Functions execute correctly <br>&emsp; + Verify Textract, AI Proxy, DynamoDB, and S3 Processed result | 07/07/2026 | 07/07/2026 | [Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) <br> [S3 Presigned URL Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) <br> [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) |
| 4 | - Test failure path and review path <br>&emsp; + Test an invalid file type or oversized file <br>&emsp; + Test a low-quality or missing-field receipt to produce `REVIEW_REQUIRED` <br>&emsp; + Test external AI timeout/rate limit/invalid JSON <br>&emsp; + Confirm reviewer correction and approval flow | 07/08/2026 | 07/08/2026 | [Step Functions Error Handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html) <br> [Amazon SQS DLQ](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) |
| 5 | - Collect evidence for each module <br>&emsp; + Capture frontend login/upload/list/detail/review screenshots <br>&emsp; + Capture S3 Raw, S3 Processed, DynamoDB item, Step Functions execution history, and SQS/DLQ screenshots <br>&emsp; + Save CloudWatch logs, X-Ray traces, SNS/SES alerts, Budget alert, and Secrets Manager evidence | 07/09/2026 | 07/09/2026 | [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) <br> [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) <br> [SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) |
| 6 | - Prepare the final demo and report <br>&emsp; + Write the 7-10 minute demo script <br>&emsp; + Finalize demo order: problem, architecture, login, upload, workflow, result, review/alert, observability, cleanup <br>&emsp; + Review Definition of Done and each member checklist | 07/10/2026 | 07/10/2026 | [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) <br> [Amazon Textract AnalyzeExpense](https://docs.aws.amazon.com/textract/latest/dg/analyzing-document-expense.html) |
| 7 | - Clean up resources and check cost <br>&emsp; + Create a manual cleanup checklist by service because most resources were configured directly in the AWS Console <br>&emsp; + Use `sam delete` only for resources deployed through a SAM/CloudFormation stack <br>&emsp; + Check S3 objects, CloudWatch logs/alarms, SQS/DLQ, DynamoDB, Secrets Manager, and SNS/SES <br>&emsp; + Check AWS Budgets/Billing to ensure no unexpected paid resources remain | 07/11/2026 | 07/11/2026 | [AWS Resource Cleanup Guidance](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/learn-whats-running-and-delete-resources.html) <br> [SAM CLI Delete](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-delete.html) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) <br> [AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html) |


### Week 12 Achievements:

* Finalized the **DocuFlow AI** workshop content:
  * Updated the overview according to the admin-approved architecture.
  * Synchronized prerequisites, deploy steps, AI Proxy setup, observability, and cleanup.
  * Rechecked internal links, architecture images, sample commands, and expected results.
  * Kept Vietnamese and English content consistent.

* Tested the end-to-end happy path:
  * The user logs in with Cognito and accesses the protected frontend.
  * The frontend calls `POST /documents/upload-url`, receives a presigned URL, and uploads an invoice to S3 Raw.
  * The S3 ObjectCreated event flows through EventBridge, SQS, and Job Starter Lambda.
  * Step Functions runs validation, Textract AnalyzeExpense, AI Proxy Lambda, External AI API, JSON validation, confidence/status, and result persistence.
  * DynamoDB stores metadata/status `EXTRACTED`; S3 Processed stores `result.json` using the agreed path convention.

* Tested failure path and review path:
  * Invalid file type or invalid metadata is rejected or moved to `FAILED`.
  * A low-quality or missing-field receipt moves to `REVIEW_REQUIRED` with `reviewReasons`.
  * External AI timeout/rate limit/invalid JSON is handled by workflow retry/catch and clear `errorType` logs.
  * The reviewer can correct fields and move the result to `CORRECTED` or `APPROVED`.

* Collected report evidence:
  * Frontend: login, upload, document list, document detail, review page, and error states.
  * Backend/workflow: S3 Raw, EventBridge rule, SQS/DLQ, Step Functions execution history, Textract output, and AI Proxy logs.
  * Data: DynamoDB item, S3 Processed `result.json`, and sample API responses.
  * Ops/security: CloudWatch logs/alarms, X-Ray traces, SNS/SES email alerts, Secrets Manager, IAM summary, CloudTrail, and Budgets.

* Completed the 7-10 minute demo script:
  * Introduce the manual invoice/receipt processing pain point.
  * Present the high-level architecture and module ownership.
  * Demo login, invoice upload, workflow tracking, result view, and review case.
  * Present observability, alerting, cost guardrails, and cleanup.

* Reviewed the Definition of Done:
  * At least 3 sample invoices/receipts can be uploaded from the frontend.
  * Happy path, review path, and failure path all have evidence.
  * Textract extracts key fields; AI Proxy returns schema-valid JSON.
  * DynamoDB/S3 Processed store data using the agreed data contract and path convention.
  * CloudWatch/SNS/SES/Secrets Manager/Budgets/SAM cleanup all have evidence.

* Completed cleanup and cost control:
  * Prepared a manual cleanup checklist by service because many resources were created directly in the AWS Console.
  * Clarified that `sam delete` only applies to resources managed by a SAM/CloudFormation stack, not manually created resources.
  * Checked bucket objects, queues, tables, secrets, alarms, log groups, and notification resources in the AWS Console.
  * Reviewed AWS Budgets/Billing to reduce post-demo cost risk.
