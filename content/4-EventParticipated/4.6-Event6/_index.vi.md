---
title: "Agentic AI Build Week Day 2"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 4.6. </b> "
---

# Bài thu hoạch “Agentic AI Build Week Day 2”

### Mục đích của sự kiện

- Tham gia chuỗi workshop thực tế trong ngày thứ hai của **Agentic AI Build Week**, tập trung vào cách xây dựng, triển khai và vận hành AI agent trong môi trường production
- Hiểu cách kết hợp AI coding tools, AWS, Amazon Bedrock, Kiro, Codex, Claude Code, Bedrock AgentCore, Agora ConvoAI và TinyFish vào các workflow có tính ứng dụng cao
- Quan sát các pattern quan trọng như spec-driven development, voice agents thời gian thực, multi-agent orchestration, memory, tool access, web automation và auditability
- Kết nối với cộng đồng builder, mentor, founder và các nhóm đang xây dựng sản phẩm agentic AI trong khuôn khổ hackathon

### Lịch trình sự kiện

| Thời gian | Nội dung | Diễn giả / Đơn vị |
|---|---|---|
| 08:45 - 10:30 | From Spec to Production Code - Kiro, Claude Code & Codex on AWS | Diep Nguyen / AWS |
| 10:30 - 12:00 | Physical AI Party: Build Voice Agents with Agora ConvoAI | Derek Zheng / Agora |
| 13:00 - 14:30 | Production Multi-Agent AI on AWS - Bedrock AgentCore | Thi Nguyen / AWS |
| 14:30 - 16:00 | Enable Track Live-Demo Workshop | Huy Vo / TinyFish |
| 18:30 - 21:00 | Agentic AI Build Week Builder Night | GenAI Fund - Official |

### Nguồn workshop

- [From Spec to Production Code - Kiro, Claude Code & Codex on AWS](https://luma.com/1mxdg4em)
- [Physical AI Party: Build Voice Agents with Agora ConvoAI](https://luma.com/3j43oewt)
- [Production Multi-Agent AI on AWS - Bedrock AgentCore](https://luma.com/lptsgwm6)
- [Enable Track Live-Demo Workshop](https://luma.com/5a4h9zsx)
- [Agentic AI Build Week Builder Night](https://luma.com/gaf-u7gd)

### Nội dung nổi bật

#### From Spec to Production Code - Kiro, Claude Code & Codex on AWS

Phiên này nhấn mạnh khoảng cách giữa demo AI coding chạy được trên máy cá nhân và hệ thống có thể đưa vào production. Nội dung đi từ việc viết đặc tả có cấu trúc bằng Kiro, dùng Kiro Hooks để duy trì tiêu chuẩn kỹ thuật, đến cách triển khai Claude Code và Codex trên Amazon Bedrock với IAM, usage policy và CloudTrail để tăng khả năng kiểm soát.

Các điểm em ghi nhận:

- Spec-driven development giúp giảm rủi ro khi dùng AI coding tools vì yêu cầu, thiết kế và code được liên kết rõ ràng
- AI coding trong môi trường doanh nghiệp cần governance, logging, kiểm soát quyền và khả năng đổi model thay vì chỉ tập trung vào tốc độ sinh code
- Agentic coding phù hợp cho các tác vụ như refactor nhiều file, tạo test, đọc hiểu codebase và chuẩn hóa implementation theo yêu cầu

#### Physical AI Party: Build Voice Agents with Agora ConvoAI

Workshop của Agora tập trung vào AI voice agents có khả năng tương tác thời gian thực. Thay vì chỉ xem voice AI như speech-to-text hoặc chatbot có giọng nói, phiên này trình bày cách voice, video, real-time communication và automation có thể kết hợp để tạo ra hệ thống tương tác với thế giới thực.

Các điểm em ghi nhận:

- Voice agent cần xử lý độ trễ, lượt nói, ngắt lời, chất lượng âm thanh và khả năng phản hồi liên tục
- Physical AI mở rộng ứng dụng của AI ra robot, thiết bị thông minh, chăm sóc khách hàng, y tế và giáo dục
- Một prototype voice agent tốt cần được kiểm thử như một hệ thống real-time, không chỉ đánh giá chất lượng câu trả lời của model

#### Production Multi-Agent AI on AWS - Bedrock AgentCore

Phiên Bedrock AgentCore tập trung vào kiến trúc multi-agent trong production. Nội dung cho thấy một agent đơn lẻ thường chưa đủ cho các tác vụ phức tạp; hệ thống cần memory, tool access, orchestration pattern và hạ tầng quản lý agent có khả năng mở rộng.

Các điểm em ghi nhận:

- AgentCore Runtime hỗ trợ triển khai multi-agent system trên hạ tầng AWS được quản lý, giảm nhu cầu tự vận hành runtime
- Episodic memory và semantic memory giúp agent duy trì context và học từ các tương tác trước đó
- Supervisor orchestration và collaborative orchestration cần được chọn theo loại workflow, mức độ phụ thuộc giữa các agent và yêu cầu kiểm soát
- Gateway, model versioning và routing là các phần quan trọng khi vận hành nhiều agent hoặc nhiều model trong cùng hệ thống

#### Enable Track Live-Demo Workshop

Phiên TinyFish tập trung vào vấn đề thực tế: nhiều agent suy luận tốt nhưng thiếu khả năng truy cập dữ liệu web sống, dữ liệu phía sau login hoặc trạng thái hệ thống hiện tại. Workshop demo cách một agent được hỗ trợ bởi web layer có thể tìm nguồn, đọc dữ liệu sạch, thực hiện hành động và trả lại bằng chứng.

Các điểm em ghi nhận:

- Agent production cần phân biệt rõ giữa dữ liệu huấn luyện cũ và dữ liệu live từ nguồn đáng tin cậy
- Web automation đáng tin cậy cần nhiều lớp như Search, Fetch, Browser, Vault, AgentQL và Agent thay vì chỉ scraping đơn giản
- Khi agent thao tác trên portal hoặc nguồn dữ liệu có xác thực, bằng chứng, logging và kiểm soát hành động là yêu cầu bắt buộc
- Các demo theo track như mobility, real estate, F&B, gaming, retail và aviation giúp em thấy cách cùng một pattern có thể áp dụng cho nhiều domain

#### Agentic AI Build Week Builder Night

Buổi Builder Night là hoạt động networking buổi tối dành cho các builder, founder, investor và đại diện doanh nghiệp trong hệ sinh thái AI. Không tập trung vào bài giảng kỹ thuật, phần này tạo không gian trao đổi về ý tưởng sản phẩm, hướng triển khai, kinh nghiệm hackathon và cơ hội hợp tác sau sự kiện.

Các điểm em ghi nhận:

- Việc trao đổi trực tiếp với các builder giúp em hiểu rõ hơn cách các nhóm chọn problem statement, scope MVP và chia vai trò khi làm dự án agentic AI
- Networking giúp kết nối phần học kỹ thuật ban ngày với câu chuyện sản phẩm, thị trường và khả năng triển khai thực tế
- Đây là cơ hội tốt để quan sát cách cộng đồng AI tại khu vực Đông Nam Á thảo luận về builder culture và enterprise adoption

### Những gì học được

- AI agent trong production cần bắt đầu từ spec rõ ràng, sau đó mới đến code generation, orchestration và deployment
- Governance không phải phần bổ sung cuối cùng; IAM, logging, audit trail, usage policy và network boundary cần được thiết kế ngay từ đầu
- Voice agents là bài toán hệ thống thời gian thực, trong đó latency và turn-taking ảnh hưởng trực tiếp đến trải nghiệm người dùng
- Multi-agent system cần memory, routing, orchestration và cơ chế kiểm soát để tránh biến thành nhiều agent hoạt động rời rạc
- Agent truy cập web hoặc portal cần bằng chứng, nguồn dữ liệu sạch, khả năng đăng nhập an toàn và kiểm soát hành động có rủi ro
- Một ngày workshop hiệu quả không chỉ cung cấp công cụ mới mà còn giúp định hình tư duy sản phẩm và cách làm việc với cộng đồng builder

### Ứng dụng vào công việc

- Khi phát triển dự án, em có thể áp dụng spec-driven development để viết rõ yêu cầu, design contract và acceptance criteria trước khi dùng AI hỗ trợ code
- Các module xử lý tài liệu nên có logging, audit trail và quyền truy cập tối thiểu, đặc biệt ở các bước gọi AI service hoặc xử lý dữ liệu nhạy cảm
- Nếu mở rộng hệ thống theo hướng agent, cần tách rõ vai trò của từng agent: ingestion, extraction, validation, review suggestion và reporting
- Với các workflow cần dữ liệu ngoài, phải ưu tiên nguồn đáng tin cậy, lưu bằng chứng và tránh để agent tự hành động trên dữ liệu chưa kiểm chứng
- Các tiêu chí học được từ voice agent và multi-agent orchestration có thể dùng để đánh giá thiết kế realtime, asynchronous workflow và human-in-the-loop trong dự án

### Trải nghiệm trong event

Tham gia **Agentic AI Build Week Day 2** giúp em có một cái nhìn rất thực tế về agentic AI. Các phiên không dừng ở phần giới thiệu khái niệm mà đi thẳng vào câu hỏi: làm sao để agent có thể chạy trong production, có governance, có dữ liệu sống, có memory, có tool access và vẫn kiểm soát được rủi ro.

Điểm em ấn tượng nhất là sự liên kết giữa các phiên. Buổi sáng bắt đầu từ đặc tả và AI coding có kiểm soát, sau đó mở rộng sang voice agent thời gian thực, buổi chiều đi vào multi-agent infrastructure và web automation, còn buổi tối là cơ hội trao đổi trực tiếp với cộng đồng builder. Nhờ vậy, em thấy rõ hơn con đường từ ý tưởng hackathon đến một kiến trúc có thể tiếp tục phát triển sau sự kiện.

#### Một số hình ảnh khi tham gia sự kiện

![Opening remarks tại Agentic AI Build Week Day 2](/images/4-EventParticipated/event-6/6C7A9572.jpg)

![Không gian workshop tại AWS HCMC Office](/images/4-EventParticipated/event-6/6C7A0119.jpg)

![Phiên From Spec to Production Code](/images/4-EventParticipated/event-6/1783570209032.jpg)

![Opening remarks của ban tổ chức](/images/4-EventParticipated/event-6/1783570210109.jpg)

![Workshop Kiro, Claude Code và Codex trên AWS](/images/4-EventParticipated/event-6/1783578886626.jpg)

![Phiên chia sẻ về production agent](/images/4-EventParticipated/event-6/1783581458134.jpg)

![Demo web-enabled agent với TinyFish](/images/4-EventParticipated/event-6/1783588020682.jpg)

![Phiên Bedrock AgentCore Observability](/images/4-EventParticipated/event-6/1783607564112.jpg)

![Builder Night và phần giao lưu buổi tối](/images/4-EventParticipated/event-6/6C7A0154.jpg)

![Các builder tham gia workshop](/images/4-EventParticipated/event-6/6C7A9585.jpg)

![Không khí học tập trong hội trường](/images/4-EventParticipated/event-6/6C7A9590.jpg)

![Ảnh check-in tại booth Agentic AI Build Week](/images/4-EventParticipated/event-6/6C7A9656.jpg)

![Các nhóm builder trao đổi trong workshop](/images/4-EventParticipated/event-6/6C7A9776.jpg)

![Mentor hỗ trợ nhóm builder trong buổi thực hành](/images/4-EventParticipated/event-6/6C7A9784.jpg)

![Khoảnh khắc tham gia Agentic AI Build Week Day 2](/images/4-EventParticipated/event-6/6C7A9987.jpg)

> Tổng thể, **Agentic AI Build Week Day 2** giúp em hiểu rằng xây dựng AI agent không chỉ là chọn model mạnh. Một hệ thống agentic tốt cần spec rõ, hạ tầng đáng tin cậy, quyền hạn được giới hạn, dữ liệu có nguồn gốc, logging đầy đủ và con người vẫn giữ quyền kiểm soát ở các bước có tác động lớn.
