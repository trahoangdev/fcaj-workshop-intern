---
title: "Workshop"
date: 2026-04-20
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# DocuFlow AI - Building a Serverless Invoice & Receipt Processing Platform

#### Overview

This workshop builds **DocuFlow AI**, a serverless platform that lets authenticated users upload invoices and receipts, then automatically extracts financial fields using Amazon Textract and Amazon Bedrock. The result is stored, tracked by status, and surfaced in a web dashboard with a manual review loop for low-confidence documents.

You will build the system incrementally, one module at a time. Each module adds resources to a single AWS SAM stack deployed in `ap-southeast-1` (Singapore), so the platform grows from an empty template to a complete event-driven pipeline.

#### Architecture

The platform uses a serverless, event-driven architecture: Amazon Cognito for auth, Amazon S3 + CloudFront for the frontend, API Gateway and Lambda for the API, EventBridge and SQS for ingestion, Step Functions for orchestration, Textract and Bedrock for AI extraction, DynamoDB for metadata, and CloudWatch and SNS for observability.

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequisite/)
3. [Frontend, Auth, and Upload](5.3-Frontend-Auth-Upload/)
4. [Storage, Ingestion, and Workflow](5.4-Storage-Ingestion-Workflow/)
5. [AI Extraction with Bedrock](5.5-AI-Extraction-Bedrock/)
6. [Data, Result, and Review](5.6-Data-Result-Review/)
7. [Observability and Security](5.7-Observability-Security/)
8. [Cleanup](5.8-Cleanup/)
