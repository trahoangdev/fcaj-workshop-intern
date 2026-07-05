---
title: "Blog 4"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Building a Scalable User Search Layer on top of Amazon Cognito

## References

- AWS Architecture Blog: [Building a scalable user search layer on top of Amazon Cognito](https://aws.amazon.com/vi/blogs/architecture/building-a-scalable-user-search-layer-on-top-of-amazon-cognito/)
- Authors: Philip Chen and Varun Selvaraj
- Published: June 1, 2026

## Overview

Amazon Cognito is a strong service for user authentication and user management. However, as applications grow, user search requirements often go beyond what default APIs such as `ListUsers` can handle efficiently. For example, support teams may need to find users by partial email, filter by group, audit across custom attributes, or perform fuzzy search with sub-second response time.

The AWS blog proposes a clear approach: do not force Cognito to become a search engine. Instead, build a dedicated search layer on top of Cognito using AWS Lambda, Amazon DynamoDB, DynamoDB Streams, and Amazon OpenSearch Serverless. Cognito remains the identity provider, while OpenSearch handles complex search queries.

## Problem Context

For simple use cases, `ListUsers` may be enough. In production environments with a large number of users, search requirements usually become more advanced:

- Search by partial email, name, or phone number.
- Find users by group membership or app client.
- Filter across standard and custom attributes at the same time.
- Support exact match, prefix match, and fuzzy search.
- Return paginated results quickly for a frontend or API.
- Synchronize data in near real time when users sign up, sign in, or are updated by administrators.

The difficult part is keeping the search index synchronized with Cognito. Scheduled batch jobs can introduce stale data. Cognito Lambda triggers alone can miss some administrator actions from the console or CLI. The solution combines multiple event sources to reduce the chance of data drift.

## Solution Architecture

![Scalable user search layer on Amazon Cognito](/fcaj-workshop-intern/images/3-BlogsTranslated/Blog4/architecture.png)

*Source: AWS Architecture Blog - Building a scalable user search layer on top of Amazon Cognito*

The solution has two main flows: ingestion and search.

### Ingestion Flow

The ingestion flow captures user data from Cognito, stores it in DynamoDB, and updates the OpenSearch index.

1. A user signs up or signs in through Amazon Cognito.
2. Cognito Lambda triggers such as **Post-confirmation** and **Pre-token generation** create or update a user record in DynamoDB.
3. DynamoDB Streams detects changes in the user table.
4. A stream-processing Lambda indexes the user data into Amazon OpenSearch Serverless.
5. For administrator-driven changes, AWS CloudTrail records Cognito Admin API calls.
6. Amazon EventBridge matches those CloudTrail events and invokes a Lambda that reads the latest user state from Cognito, then updates DynamoDB and OpenSearch.

Adding CloudTrail and EventBridge is important because not every user change happens during the authentication flow. If an administrator creates, updates, disables a user, or changes group membership, the architecture can still synchronize the search index.

### Search Flow

The search flow is separate from Cognito's default APIs.

1. An authenticated user submits a query from the UI.
2. Amazon API Gateway receives the request and validates the Cognito JWT token with a Cognito authorizer.
3. A search Lambda receives the query parameters.
4. The Lambda assumes a read-only role and queries OpenSearch Serverless.
5. Results are formatted and returned to the frontend with pagination.

This allows the search API to support fuzzy matching, complex filtering, and sub-second response times without forcing Cognito to handle search workloads it was not designed for.

## AWS Services Involved

| Service | Role in the solution |
| --- | --- |
| Amazon Cognito | Manages users, authentication, and JWT tokens |
| AWS Lambda | Handles Cognito triggers, CloudTrail events, DynamoDB Streams, and search API requests |
| Amazon DynamoDB | Stores normalized user profile data as an intermediate source |
| DynamoDB Streams | Detects data changes and starts the indexing process |
| Amazon OpenSearch Serverless | Stores the search index and handles advanced search queries |
| AWS CloudTrail | Records Cognito Admin API calls |
| Amazon EventBridge | Matches CloudTrail events and starts synchronization Lambda functions |
| Amazon API Gateway | Provides the REST API for frontend search |

## Technical Highlights

### Separating Command and Query

The most important idea is the separation of responsibilities. Cognito handles identity commands such as sign-up, sign-in, user updates, and group membership. OpenSearch handles queries such as fuzzy search, prefix search, and filtering.

This is similar to a practical CQRS pattern: keep the identity system as the source of authority, but create a read model optimized for search.

### Synchronizing Through Multiple Paths

If the system only uses a Post-confirmation trigger, it may miss administrator updates. If it only uses batch synchronization, data can become stale. The article combines Cognito triggers, CloudTrail, EventBridge, and DynamoDB Streams to keep OpenSearch closer to the real Cognito state.

### Cognito Lambda Trigger Limits

The original post notes that Cognito Lambda triggers have a timeout limit. Trigger functions should avoid heavy processing. A better approach is to write the required data to DynamoDB quickly, then let DynamoDB Streams and another Lambda handle indexing asynchronously.

## What I Learned

Before reading this article, I tended to think of Cognito as both the user management system and the user search system. After studying the solution, I think the better approach is to let Cognito focus on identity and move advanced search to a dedicated search layer.

I also learned that user data synchronization is more complex than listening to one trigger. In reality, user data can change through sign-up, sign-in, the admin console, CLI, or API. A production architecture needs to account for all of those paths.

## Conclusion

This article shows how to extend Amazon Cognito with a dedicated search layer, using DynamoDB as an intermediate data store and OpenSearch Serverless as the search index. The architecture supports fuzzy search, complex filtering, pagination, and sub-second response time at scale.

For a real implementation, I would start by defining which user attributes need to be searchable, designing the DynamoDB data model, optimizing the OpenSearch index, handling all Cognito change sources, and applying least-privilege IAM permissions to each Lambda function.
