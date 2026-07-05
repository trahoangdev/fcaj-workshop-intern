---
title: "Blog 5"
date: 2025-02-11
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
---

# Create a Serverless Custom Retry Mechanism for Stateless Queue Consumers

## References

- AWS Architecture Blog: [Create a serverless custom retry mechanism for stateless queue consumers](https://aws.amazon.com/vi/blogs/architecture/create-a-serverless-custom-retry-mechanism-for-stateless-queue-consumers/)
- Author: Kaizad Wadia
- Published: February 11, 2025

## Overview

In serverless architectures, a common pattern is AWS Lambda consuming messages from Amazon SQS and then calling a downstream service or external API. When the downstream service fails temporarily or returns rate limits, the system needs retries to avoid losing messages. However, retrying too quickly can make the downstream problem worse, and uncontrolled retries can push valid messages into a Dead-Letter Queue (DLQ) too early.

The AWS blog proposes a pattern that uses **Amazon EventBridge Scheduler** to build a custom retry mechanism for stateless queue consumers. The main idea is to move the waiting period out of Lambda. Lambda processes the message and decides when to retry, while EventBridge Scheduler sends the message back to SQS at a future time.

## Problem Context

SQS and Lambda already provide default retry behavior, but it is not always flexible enough. Real systems often need more control:

- A downstream API returns HTTP 429 because of rate limiting.
- An external service has a short outage and needs more time before retry.
- Different error types require different retry intervals.
- Some errors should be retried, while others should go directly to the DLQ.
- The system needs exponential backoff or linear retry intervals.
- Lambda should not sleep while waiting because idle time still costs money.

If workflow state is already managed by Step Functions, built-in retry and backoff may be appropriate. For stateless queue consumers, this article presents a lighter pattern using EventBridge Scheduler as an external scheduler.

## Solution Architecture

![Custom retry architecture with EventBridge Scheduler](/fcaj-workshop-intern/images/3-BlogsTranslated/Blog5/architecture.png)

*Source: AWS Architecture Blog - Create a serverless custom retry mechanism for stateless queue consumers*

The main flow works as follows:

1. Lambda receives a message from Amazon SQS.
2. Lambda processes the message and calls the downstream service.
3. If a retryable error occurs, Lambda catches the error instead of failing the invocation through default behavior.
4. Lambda calculates the next retry time based on error type, previous retry count, or backoff policy.
5. Lambda creates a schedule in Amazon EventBridge Scheduler.
6. At the scheduled time, EventBridge Scheduler sends the message back to SQS.
7. Lambda processes the message again when it becomes available in the queue.
8. If retry attempts exceed the limit or the error should not be retried, the message is sent to the DLQ.

The important point is that the message does not stay inside Lambda while waiting. The system avoids paying for idle compute and still controls exactly when the next retry should happen.

## AWS Services Involved

| Service | Role in the solution |
| --- | --- |
| Amazon SQS | Stores the main messages and provides a DLQ for failed messages |
| AWS Lambda | Consumes messages, detects errors, and creates retry schedules |
| Amazon EventBridge Scheduler | Schedules a future delivery of the message back to SQS |
| AWS IAM | Grants Lambda permission to create schedules, pass roles, and send messages to the DLQ |
| Amazon CloudWatch | Monitors logs, metrics, retry behavior, and DLQ usage |
| AWS PrivateLink | Allows private access to EventBridge Scheduler from VPC-based Lambda functions when required |

## Technical Highlights

### Flexible Retry Timing

With this pattern, each message can have its own delay. For example, HTTP 429 errors can wait 5 minutes, timeout errors can retry after 30 seconds, and validation errors can go directly to the DLQ. This is more flexible than relying only on a shared queue visibility timeout.

### Tracking Retries with Message Attributes

The article recommends using SQS message attributes or message body data to track retry attempts. Each retry updates the attempt count or timestamp. If the retry limit is exceeded, the message is no longer scheduled and is sent to the DLQ.

### Idempotency and Partial Failures

Partial failures are a major risk. For example, Lambda may write to a database successfully but fail when calling an external API. When the message is retried, the whole operation may run again. Consumers should therefore be idempotent, meaning the same message can be processed more than once without creating duplicate data or incorrect state.

### Scheduler Precision

EventBridge Scheduler has minute-level granularity, and there is additional latency between Scheduler, SQS, and Lambda. This pattern is better suited for retries measured in minutes or longer, not workflows that require exact second-level retry timing.

### Monitoring and Security

The system should monitor Lambda errors, duration, invocation count, schedules created, DLQ depth, and unusual retry patterns. From a security perspective, the Lambda role should have only the minimum permissions needed to create schedules, pass the scheduler role, read from SQS, and send messages to the DLQ.

## What I Learned

Before reading this article, I mostly thought of SQS/Lambda retry behavior and Step Functions as the main options. After studying the pattern, I see EventBridge Scheduler as a useful middle ground: lighter than Step Functions, but more flexible than default retries.

I also learned that Lambda should not be used for waiting. If the system only needs to "try again later," waiting should be handled by a scheduler, while Lambda should run only when there is real work to do.

## Conclusion

This article presents a practical serverless pattern for retrying stateless queue consumers. By combining Amazon SQS, AWS Lambda, EventBridge Scheduler, and DLQ, the system can control retry timing more precisely, reduce pressure on downstream services, and avoid unnecessary compute cost.

For production use, I would start by classifying error types clearly, setting reasonable retry limits, designing idempotent consumers, monitoring DLQ usage closely, and validating IAM permissions so the scheduler has only the access required to send messages back to the queue.
