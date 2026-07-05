---
title: "Blog 6"
date: 2026-05-19
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---

# Revolutionizing Core Logging with Machine Learning and Amazon EKS

## References

- AWS Architecture Blog: [How ALS GeoAnalytics LITHOLENS ™ revolutionizes core logging through machine learning with Amazon EKS](https://aws.amazon.com/vi/blogs/architecture/how-als-geoanalytics-litholens-revolutionizes-core-logging-through-machine-learning-with-amazon-eks/)
- Authors: Saransh Burman, Sriharsh Adari, and Shervin Azad
- Published: May 19, 2026

## Overview

In the mining industry, geological analysis and core logging are important for building resource models, supporting mine design, and making extraction decisions. Traditionally, geologists inspect drill core samples physically, often at remote sites. This process is time-consuming, can vary by expert interpretation, and is difficult to scale as image data grows.

The AWS blog describes how ALS GeoAnalytics deployed **LITHOLENS™** on AWS to automate core logging with machine learning and computer vision. The interesting part is that the solution is not only about ML models. It also solves an operations problem: running heavy GPU workloads, scaling with demand, and reducing cost when no jobs are running.

## Problem Context

A mining project may require thousands of drill holes to understand an ore body and build a 3D geological model. The traditional process has several limitations:

- Experts need to travel to remote sites to inspect drill core samples.
- Interpretations can vary between geologists.
- Historical imagery is often underused.
- Physical samples can be lost or degrade over time.
- A small pool of qualified experts can become a bottleneck.
- Non-standardized data makes cross-project comparison difficult.

ALS GeoAnalytics needed a system that could process high-resolution drill core images, run multiple machine learning and deep learning models, and stay cost-efficient for unpredictable workloads.

## Machine Learning in LITHOLENS™

The article describes several ML components in the LITHOLENS™ platform:

- **Color Extraction**: scans core images, extracts pixel colors, and stores results in Amazon S3.
- **Color Clustering**: uses algorithms such as K-Means or Gaussian Mixture Model to group colors and highlight mineralogical variation.
- **Percentage Report**: segments images, for example into 20 cm intervals, and calculates the color distribution for each cluster.
- **RoQE Net**: a deep learning model that extracts geotechnical parameters such as Rock Quality Designation (RQD) and alpha angles.
- **VeinNet and CobbleNet**: identify geological features such as veins, cobbles, and lithological structures.

The important point is that ML is not only used for classification. It also standardizes analysis so results are easier to compare across projects and less dependent on manual interpretation.

## Solution Architecture

![LITHOLENS architecture on AWS](/fcaj-workshop-intern/images/3-BlogsTranslated/Blog6/architecture.png)

*Source: AWS Architecture Blog - How ALS GeoAnalytics LITHOLENS revolutionizes core logging through machine learning with Amazon EKS*

ALS GeoAnalytics built LITHOLENS™ with a hybrid architecture that combines serverless components and containerized workloads.

1. Users submit analysis jobs through Amazon API Gateway.
2. AWS Lambda handles the API backend, job parameters, and EKS configuration.
3. Amazon EKS runs ML containers on GPU-backed G6 instances.
4. Input data is read from Amazon S3.
5. Logs and intermediate results are tracked during processing.
6. Final results are stored in Amazon S3 or Amazon RDS through API calls.
7. When jobs complete, compute resources scale down automatically to reduce cost.

This architecture separates the lightweight API layer from the heavy compute layer. Lambda is suitable for job submission, status checks, and result retrieval. EKS is suitable for GPU-based model training and inference that require longer runtime and more compute power.

## AWS Services Involved

| Service | Role in the solution |
| --- | --- |
| Amazon API Gateway | Provides the unified REST API for job submission and status checks |
| AWS Lambda | Handles API backend logic and job orchestration |
| Amazon EKS | Runs containerized ML workloads on GPU instances |
| Amazon EC2 G6 instances | Provides GPU compute for training and inference |
| Amazon S3 | Stores drill core images, intermediate data, and some results |
| Amazon RDS | Stores metadata and structured analysis results |
| Custom AMIs | Package dependencies and model artifacts to reduce startup time |

## Technical Highlights

### Hybrid Architecture

Using only serverless would not fit heavy ML workloads that need GPUs and longer processing time. Using only Kubernetes could mean running API components even when there are no jobs. Combining Lambda and EKS lets each service handle the work it is best suited for.

### Scale to Zero

Geological workloads are uneven. Some periods may receive many jobs, while others may have little activity. The article emphasizes that EKS containers or instances can scale down when no jobs are queued, avoiding the cost of idle GPU capacity.

### Custom AMIs for Faster Startup

ALS uses custom Amazon Machine Images with dependencies and model artifacts already installed, reducing startup time from several minutes to under 30 seconds. This detail matters because scale-to-zero is only practical if the system can scale up quickly when a new job arrives.

### Business Impact

According to the article, LITHOLENS™ has been used with 10 mining companies across more than 40 projects. The platform improves analysis consistency, reduces the need for experts to travel to sites, increases traceability, and supports real-time monitoring and reporting.

## What I Learned

Before reading this article, I often thought the main challenge in ML systems was the model itself. After studying this solution, I realized that for industrial workloads, operational architecture is just as important. A strong model can still become expensive or hard to scale if the infrastructure is not designed well.

I also learned the value of choosing the right service for each part of the workload. Lambda handles lightweight API and orchestration. EKS handles heavy compute. S3 stores large data. RDS stores structured results. Custom AMIs and scale-to-zero help balance performance and cost.

## Conclusion

This article is a practical example of bringing machine learning into a traditional industry through cloud-native architecture. ALS GeoAnalytics did not only build geological recognition models. They designed a scalable, cost-aware platform for large image data and GPU workloads on AWS.

For a similar system, I would start by understanding the workload pattern: data size, GPU requirements, job frequency, and latency expectations. From there, I would decide which parts should be serverless, which parts need EKS, and how to optimize startup and scale-down behavior to avoid wasting resources.
