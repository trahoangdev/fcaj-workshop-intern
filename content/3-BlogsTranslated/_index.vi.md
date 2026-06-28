---
title: "Các bài blogs đã dịch"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

<!-- {{% notice warning %}}  
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Tại đây sẽ là phần liệt kê, giới thiệu các blogs mà các bạn đã dịch. Ví dụ: -->

###  [Blog 1 - Cyber Resilience trên AWS: Phục hồi sau ransomware và sự cố phá hủy](3.1-Blog1/)
Blog này chia sẻ cách AWS đề xuất xây dựng năng lực cyber resilience cho các workload quan trọng. Nội dung tập trung vào việc tách biệt production, recovery account và isolated recovery environment, sử dụng AWS Backup logically air-gapped vault để bảo vệ recovery point, kiểm tra backup trước khi restore, chọn recovery point an toàn và áp dụng framework Rebuild-Restore-Rotate.
###  [Blog 2 - Xây dựng Multi Account Patch Compliance Dashboard với Kiro Specs](3.2-Blog2/)
Blog này chia sẻ cách AWS xây dựng dashboard theo dõi patch compliance trên nhiều account bằng AWS Systems Manager Patch Manager, Resource Data Sync, Amazon S3, AWS Lambda, EventBridge, internal ALB và Session Manager. Nội dung cũng tập trung vào cách dùng Kiro Specs, steering files và MCP servers để phát triển giải pháp theo hướng spec-driven development.
###  [Blog 3 - Kiến trúc AI-powered Resilience Framework trên AWS](3.3-Blog3/)
Blog này chia sẻ cách AWS kết hợp AWS Resilience Hub, AWS Fault Injection Service, Amazon Bedrock AgentCore, AWS Systems Manager, AWS Config và CloudWatch để xây dựng framework kiểm thử resilience liên tục. Nội dung tập trung vào 5 lớp chính: discovery, test generation, experimentation, gap analysis và continuous validation trong CI/CD.