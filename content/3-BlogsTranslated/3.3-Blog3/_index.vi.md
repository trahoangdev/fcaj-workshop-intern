---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Kiến trúc AI-powered Resilience Framework trên AWS

## Nguồn tham khảo

- AWS Architecture Blog: [Architecting AI-powered resilience framework on AWS](https://aws.amazon.com/vi/blogs/architecture/architecting-ai-powered-resilience-framework-on-aws/)
- Tác giả: Medha Shree
- Ngày phát hành: 22/06/2026

## Sau khi đọc bài này, mình hiểu resilience theo một cách khác

Trước đây, khi nghĩ về resilience trên AWS, mình thường nghĩ đến những thiết kế quen thuộc như Multi-AZ, Auto Scaling, health check, backup, retry hoặc failover. Những thứ đó đều quan trọng, nhưng bài blog này làm mình chú ý đến một câu hỏi khác: **làm sao biết chắc các cơ chế resilience đó thật sự hoạt động khi hệ thống gặp lỗi?**

Một hệ thống có thể được thiết kế rất đẹp trên sơ đồ kiến trúc, nhưng production lại luôn thay đổi. Một service mới được thêm vào, một connection string bị hard-code trong code, một timeout cấu hình sai, một dependency tạm thời trở thành dependency lâu dài, hoặc một hotfix không được cập nhật lại vào tài liệu. Đến lúc sự cố xảy ra, team mới phát hiện hệ thống có một điểm yếu chưa từng được kiểm thử.

Thông điệp mình rút ra từ bài AWS này là: **resilience không nên chỉ được giả định, mà phải được kiểm chứng liên tục**.

## Vì sao resilience testing khó?

Nếu chỉ nhìn ở mức hạ tầng, chúng ta có thể biết ứng dụng đang dùng EC2, RDS, Lambda, DynamoDB, S3 hay Load Balancer. Nhưng resilience không chỉ nằm ở resource. Nó còn nằm ở cách ứng dụng gọi dependency, cách retry được cấu hình, cách timeout hoạt động, cách circuit breaker phản ứng, và cách team khôi phục khi một thành phần bị lỗi.

Vấn đề là những thông tin này thường nằm rải rác ở nhiều nơi: architecture diagram, CloudFormation hoặc Terraform, source code, runbook, log, dashboard và cả kinh nghiệm của từng team. Khi hệ thống deploy liên tục, tài liệu rất dễ bị lỗi thời.

Đó là lý do bài viết đề xuất một AI-powered resilience framework. AI ở đây không phải để “tự động phá hệ thống”, mà để giúp team hiểu hệ thống kỹ hơn, tìm dependency ẩn, tạo experiment phù hợp và đưa việc kiểm thử resilience vào quy trình phát triển hằng ngày.

## Framework 5 lớp: từ hiểu hệ thống đến kiểm chứng liên tục

Bài AWS chia framework thành 5 lớp: Discovery, Test Generation, Experimentation, Gap Analysis và Continuous Validation. Mình hiểu đơn giản như sau:

| Lớp | Mình hiểu là |
| --- | --- |
| Discovery | Tìm xem hệ thống thật sự đang phụ thuộc vào những gì |
| Test Generation | Tạo các kịch bản lỗi đúng với kiến trúc và rủi ro của hệ thống |
| Experimentation | Chạy chaos experiment có kiểm soát, có guardrail và stop condition |
| Gap Analysis | Phân tích điểm yếu sau khi test và ưu tiên hướng xử lý |
| Continuous Validation | Đưa resilience checks vào CI/CD để tránh regression sau mỗi lần thay đổi |

Điều mình thích ở cách chia này là nó tạo thành một vòng lặp. Không phải chạy một lần rồi thôi, mà mỗi lần hệ thống thay đổi, dependency map và experiment cũng nên được cập nhật theo.

## Discovery: phần quan trọng nhất thường bị xem nhẹ

Phần Discovery làm mình ấn tượng nhất. AWS Resilience Hub thế hệ mới có khả năng native dependency discovery để tìm các AWS services, internal endpoints và third-party endpoints mà ứng dụng đang dùng. Sau đó, custom agent chạy trên Amazon Bedrock AgentCore có thể mở rộng việc phân tích xuống tầng code và Infrastructure as Code.

Agent có thể đọc CloudFormation, Terraform, repository code, connection string, timeout configuration, retry logic và circuit breaker. Đây là điểm rất thực tế, vì nhiều vấn đề resilience không nằm trên sơ đồ kiến trúc mà nằm trong cách code vận hành.

Ví dụ, một database có thể đã bật Multi-AZ, nhưng nếu application không xử lý retry khi failover thì người dùng vẫn có thể gặp lỗi. Trên giấy tờ, kiến trúc có vẻ resilient. Trong thực tế, behavior của ứng dụng lại chưa chắc resilient.

Theo bài gốc, discovery ban đầu có thể rút ngắn từ vài tuần xuống còn vài giờ trong môi trường single-account có hàng ngàn resource. Các lần chạy sau có thể dựa vào AWS Config để chỉ xử lý phần thay đổi. Với mình, đây là giá trị lớn: architecture map không còn là tài liệu tĩnh mà tiến gần hơn đến trạng thái runtime thật.

## AI giúp tạo experiment có ngữ cảnh hơn

Sau khi hiểu dependency, bước tiếp theo là tạo experiment. Đây là nơi Amazon Bedrock và Amazon Bedrock AgentCore phát huy vai trò.

Thay vì chạy các bài test chung chung như “tắt một instance” hoặc “inject network latency” mà không rõ mục tiêu, framework dùng context của hệ thống để tạo các hypothesis có ý nghĩa hơn. Nó có thể kết hợp dependency map, RTO, RPO, availability target, application tier và business impact để ưu tiên experiment.

Ví dụ, nếu một service customer-facing phụ thuộc vào một database quan trọng, experiment về database failover có thể đáng ưu tiên hơn một test ít ảnh hưởng đến người dùng. Nếu một dependency có dấu hiệu single point of failure, experiment liên quan đến dependency đó nên được đưa lên trước.

Điểm mình thấy hay là AI không thay thế hoàn toàn SRE hay cloud architect. Nó giống một trợ lý giúp gom thông tin, chỉ ra rủi ro và đề xuất hướng kiểm thử. Còn việc có chạy experiment trong production hay không vẫn cần approval, review và guardrail rõ ràng. Bài viết cũng đề xuất dùng AWS Step Functions để tạo manual approval workflow trước khi chạy các experiment nhạy cảm.

## Chaos engineering không phải là làm hỏng hệ thống một cách ngẫu nhiên

Một hiểu lầm phổ biến là chaos engineering nghĩa là cố tình làm sập hệ thống. Bài này giải thích ngược lại: chaos experiment phải được thiết kế có kiểm soát.

AWS Fault Injection Service được dùng để chạy các kịch bản như terminate EC2 instance, inject network latency, throttle API call, failover Amazon RDS hoặc mô phỏng vấn đề ở Availability Zone. Nhưng experiment không nên bắt đầu với phạm vi lớn. Bài viết gợi ý bắt đầu rất nhỏ, ví dụ 1% resource, rồi tăng dần lên 5%, 10% hoặc 25% tùy mức độ an toàn và risk tolerance.

Amazon CloudWatch alarms đóng vai trò stop condition. Nếu error rate, latency hoặc availability bắt đầu tiến gần ngưỡng rủi ro, experiment phải dừng lại trước khi ảnh hưởng nghiêm trọng đến SLA. Mình thấy đây là phần làm cho chaos engineering trở nên thực dụng hơn: mục tiêu là học từ lỗi trong phạm vi an toàn, không phải tạo thêm outage.

## Sau mỗi experiment, hệ thống phải học được điều gì đó

Nếu chạy experiment xong chỉ để có report thì chưa đủ. Bài viết đưa ra lớp Gap Analysis để biến kết quả test thành hành động cụ thể.

AWS Resilience Hub có thể liên kết kết quả experiment với resilience policy và phân loại gap theo nhiều nhóm: kiến trúc, vận hành, bảo vệ dữ liệu và phạm vi kiểm thử. Mỗi gap được ưu tiên theo severity, likelihood và business impact. Cách này hợp lý vì không phải gap nào cũng quan trọng như nhau.

Một điểm mình thích là AWS Systems Manager Automation documents có thể được dùng để biến recovery procedure thành automation runbook. Nếu một cách khôi phục đã được kiểm chứng qua experiment, team có thể codify nó để giảm MTTR trong sự cố thật.

Nói cách khác, framework không chỉ tìm lỗi. Nó còn giúp biến bài học sau mỗi lần kiểm thử thành cải tiến có thể tái sử dụng.

## Đưa resilience vào CI/CD thay vì để cuối dự án

Phần Continuous Validation là phần làm bài viết này khác với cách nghĩ truyền thống. Resilience testing không nên là hoạt động thỉnh thoảng mới làm, hoặc chỉ làm trước khi go-live. Nó nên được đưa vào pipeline.

Bài viết đề xuất một cách làm hai tầng. Tầng nhẹ là policy-as-code check, ví dụ dùng Open Policy Agent để kiểm tra Infrastructure as Code hoặc Dockerfile. Các check này chạy nhanh trong mỗi commit và giúp phát hiện lỗi cơ bản như thiếu health check, cấu hình single-AZ hoặc thiếu baseline resilience.

Tầng nặng hơn là full resilience assessment, phù hợp với pre-production gate hoặc khi có thay đổi kiến trúc đáng kể. Với deployment thường ngày, có thể chỉ chạy một số regression test quan trọng như database failover, Availability Zone loss hoặc circuit breaker activation.

Mình thấy cách chia này thực tế vì không làm pipeline quá chậm, nhưng vẫn giúp phát hiện vấn đề trước production. Resilience vì vậy được shift-left, giống như security testing hoặc unit testing trong quy trình phát triển hiện đại.

## Khi scale lên enterprise thì cần cẩn thận hơn

Bài viết cũng nói về cách triển khai theo giai đoạn. Không nên vừa đọc xong là áp dụng ngay cho toàn bộ production.

Giai đoạn đầu nên chọn một ứng dụng không quá critical, kiến trúc tương đối dễ hiểu, bật AWS Config, deploy discovery agent trên Amazon Bedrock AgentCore và chạy baseline assessment trong AWS Resilience Hub. Khi team đã hiểu cách framework hoạt động, có thể mở rộng sang vài ứng dụng khác ở nhiều tier khác nhau và bắt đầu chạy experiment với scope nhỏ trong thời gian low-traffic.

Khi triển khai ở quy mô enterprise, bài viết đề xuất multi-account architecture, centralized reporting, cross-account experiment coordination, shared templates qua AWS Organizations và dashboard bằng Amazon QuickSight. Lúc này cũng cần chính sách resilience theo tier, vì ứng dụng mission-critical không thể có cùng RTO/RPO và tần suất kiểm thử với workload ít quan trọng.

## Bảo mật cho AI agent cũng là một phần của kiến trúc

Một điểm mình nghĩ không nên bỏ qua là bảo mật cho AI agent. Vì agent có thể đọc thông tin hạ tầng, code và cấu hình, quyền của nó phải được giới hạn rõ ràng.

Bài viết nhấn mạnh least privilege, read-only access trong giai đoạn discovery, audit bằng AWS CloudTrail, encryption bằng AWS KMS và guardrail cho Amazon Bedrock. Amazon Bedrock AgentCore Runtime cung cấp MicroVM session isolation để mỗi discovery session chạy trong môi trường tách biệt.

Với mình, đây là nhắc nhở quan trọng: dùng AI trong cloud operations không có nghĩa là trao toàn quyền cho AI. AI agent nên hoạt động trong một boundary rõ ràng, có IAM scope, logging, approval gate và guardrail.

## Điều mình rút ra

Bài blog này làm mình nhìn resilience testing theo hướng trưởng thành hơn. Trước đây mình nghĩ dùng AWS FIS để mô phỏng lỗi đã là phần chính. Nhưng thật ra, trước khi test, mình phải hiểu hệ thống đang phụ thuộc vào gì. Nếu dependency map sai, experiment có thể chạy rất “xịn” nhưng vẫn kiểm thử nhầm thứ.

AI phù hợp với bài toán này vì nó có thể xử lý nhiều loại context cùng lúc: infrastructure, code, dependency, RTO/RPO, business impact, kết quả experiment cũ và thay đổi trong CI/CD. Nhưng AI chỉ nên hỗ trợ phân tích và đề xuất. Phần thực thi vẫn cần guardrail, approval và monitoring.

Thông điệp mình nhớ nhất là: hệ thống chắc chắn sẽ có lúc gặp lỗi. Sự khác biệt nằm ở việc mình phát hiện điểm yếu trong một experiment có kiểm soát, hay để khách hàng phát hiện nó trong production.

## Kết luận

Bài viết của AWS cho thấy một hướng tiếp cận rất đáng học: kết hợp AWS Resilience Hub, AWS Fault Injection Service, Amazon Bedrock AgentCore, AWS Systems Manager, AWS Config và CloudWatch để tạo một vòng lặp resilience testing liên tục.

Giá trị lớn nhất không chỉ là tự động tạo chaos experiment, mà là biến resilience thành một quy trình sống: khám phá dependency, tạo test đúng ngữ cảnh, chạy experiment an toàn, phân tích gap, tự động hóa remediation và kiểm chứng lại trong CI/CD.

Nếu áp dụng thực tế, mình sẽ bắt đầu nhỏ với một ứng dụng không quá critical, xác định RTO/RPO rõ ràng, bật AWS Config, chạy assessment trong AWS Resilience Hub, rồi mới đưa AI agent và experiment tự động vào. Khi team đã tin vào guardrail và quy trình review, lúc đó mới nên mở rộng sang workload quan trọng hơn.
