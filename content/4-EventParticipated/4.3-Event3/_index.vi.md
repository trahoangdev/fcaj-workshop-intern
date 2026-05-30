---
title: "Agentic Build Day and Community Activity"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

<!--{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}-->

# Bài thu hoạch “Agentic Build Day and Community Activity”

### Mục đích của sự kiện

- Buổi deep-dive kỹ thuật 3 tiếng về các **agentic services** của AWS
- Đi qua một reference architecture thực tế: nền tảng **AI-powered marketing campaign** điều phối nhiều agent chuyên biệt để tự động hóa phân khúc khách hàng, tạo chiến dịch và khuyến mãi từ đầu đến cuối
- Giúp người tham dự có mô hình tư duy rõ ràng về cách các agentic services của AWS ghép lại với nhau
- Kết hợp nội dung kỹ thuật với **hoạt động cộng đồng** (làm thú nhồi bông không cần may cho trẻ em)

### Diễn giả & người dẫn

- **Diem My / Dinh Nguyen** – Dẫn dắt hoạt động cộng đồng
- **My Nguyen** – Why Agentic AI & use case MarTech
- **Dai Truong / Thi Nguyen** – Hướng dẫn Kiro Workshop
- Đội ngũ AWS GenAI Builders Club – các phiên kỹ thuật

### Lịch trình sự kiện

| Thời gian | Nội dung | Diễn giả |
|---|---|---|
| 13:00 – 14:00 | 🧸 Làm thú nhồi bông không cần may cho trẻ em (Hoạt động cộng đồng) | Diem My / Dinh Nguyen |
| 14:00 – 14:15 | Vì sao Agentic AI & use case MarTech sẽ khám phá | My Nguyen |
| 14:15 – 15:00 | AWS Agentic Services in Action: AgentCore Runtime, Memory, MCP Gateway & Agent-to-Agent (A2A) Communication | — |
| 15:00 – 15:15 | ☕ Giải lao | — |
| 15:15 – 15:40 | Strands Agents Framework & cách nó kết nối mọi thứ | — |
| 15:40 – 16:10 | Full Workflow Walkthrough: lần theo code từ user prompt đến tạo chiến dịch | — |
| 16:10 – 16:30 | Security Patterns, Session Persistence & Infrastructure as Code | — |
| 16:30 – 16:45 | Key Takeaways & Source Code Tour | — |
| 16:45 – 17:00 | Hướng dẫn Kiro Workshop | Dai Truong / Thi Nguyen |

### Nội dung nổi bật

#### Hoạt động cộng đồng – Thú nhồi bông không cần may cho trẻ em

- Phần mở đầu ấm áp, ai cũng tự tay làm **thú nhồi bông không cần may** để tặng cho trẻ em
- Nhắc nhở rằng một cộng đồng builder là về con người, không chỉ công nghệ

#### My Nguyen – Vì sao Agentic AI & use case MarTech

- Vì sao **agentic AI** khác với mô hình prompt-response truyền thống
- Giới thiệu use case tham chiếu **MarTech**: nền tảng marketing campaign chạy bằng AI
- Cách nhiều agent chuyên biệt phối hợp để tự động hóa phân khúc khách hàng, tạo chiến dịch và khuyến mãi

#### AWS Agentic Services in Action

- **AgentCore Runtime**: môi trường thực thi để chạy các agent
- **Memory**: cách agent lưu giữ và truy hồi context qua các lần tương tác
- **MCP Gateway**: kết nối agent với tools và dữ liệu qua Model Context Protocol
- **Agent-to-Agent (A2A) Communication**: cách các agent chuyên biệt phối hợp với nhau

#### Strands Agents Framework

- Tổng quan về framework **Strands Agents**
- Cách nó gắn kết các agentic services thành một workflow thống nhất
- Vai trò của orchestration trong hệ multi-agent

#### Full Workflow Walkthrough

- Lần theo code từ đầu đến cuối — từ **user prompt** đến **chiến dịch được tạo ra**
- Thấy reference architecture chạy thực tế ra sao

#### Security Patterns, Session Persistence & Infrastructure as Code

- **Security patterns** cho agentic workloads (IAM, least privilege)
- **Session persistence** để giữ trạng thái qua các lần chạy agent
- **Infrastructure as Code** (CDK) để kiến trúc có thể tái lập

#### Key Takeaways & Source Code Tour

- Tóm tắt các khái niệm cốt lõi và cách các mảnh ghép kết nối
- Tour qua **toàn bộ source code** để tự khám phá theo nhịp riêng

#### Dai Truong / Thi Nguyen – Hướng dẫn Kiro Workshop

- Hướng dẫn dùng **Kiro** cho workshop và xây các tính năng agentic
- Cách áp dụng workflow spec-driven vào project thực tế

### Những gì học được

- **Agentic AI** vượt khỏi single prompt, tiến tới hệ thống các agent phối hợp với nhau
- AWS cung cấp các building block — **AgentCore Runtime, Memory, MCP Gateway, A2A** — ghép lại với nhau
- Framework **Strands Agents** là chất keo điều phối các workflow multi-agent
- Hệ agentic chạy production vẫn cần **security, session persistence và IaC** để đáng tin cậy
- Có sẵn **reference architecture + source code** là cách học nhanh nhất qua việc tùy biến lại

### Ứng dụng vào công việc

- Dùng **reference architecture MarTech** làm template cho mọi use case tự động hóa nhiều bước
- Thử nghiệm **AgentCore Memory** để cho trợ lý nội bộ có context bền vững
- Kết nối các tool hiện có qua **MCP Gateway** thay vì hardcode integration
- Áp dụng **A2A communication** để tách một prompt single-agent phức tạp thành các agent chuyên biệt
- Dùng **CDK / Infrastructure as Code** để deploy agentic có thể tái lập và review được
- Thử **Kiro** cho phát triển spec-driven các tính năng agentic

### Yêu cầu cần có

- Quen thuộc với AWS (Lambda, IAM, CDK) và Python cơ bản
- Không cần kinh nghiệm trước với AI agents

### Trải nghiệm trong event

Tham gia **Agentic Build Day and Community Activity** ngày 29/05/2026 là sự kết hợp tuyệt vời giữa hoạt động cộng đồng ý nghĩa và một buổi deep-dive kỹ thuật tập trung. Một số khoảnh khắc nổi bật:

#### Bắt đầu bằng cộng đồng
- Sự kiện mở đầu bằng hoạt động làm **thú nhồi bông không cần may** cho trẻ em, do **Diem My** và **Dinh Nguyen** dẫn dắt — một cách rất ấm áp để kết nối với các builder khác trước khi vào phần code.

#### Học toàn bộ agentic stack
- **My Nguyen** mở màn với use case **MarTech** rõ ràng, biến khái niệm trừu tượng "agentic AI" thành thứ cụ thể.
- Phần deep-dive về **AgentCore Runtime, Memory, MCP Gateway và A2A** cuối cùng đã kết nối những service mà trước đó em chỉ đọc rời rạc.
- Thấy framework **Strands Agents** gắn kết mọi thứ giúp em hiểu orchestration thực sự hoạt động thế nào.

#### Lần theo code thật
- **Full workflow walkthrough** — từ user prompt đến tạo chiến dịch — là phần giá trị nhất, biến lý thuyết thành một đường thực thi có thể lần theo được.
- Phiên về **security, session persistence và IaC** là lời nhắc rằng demo thì dễ nhưng production cần guardrails.

#### Thực hành với Kiro
- **Dai Truong** và **Thi Nguyen** khép lại với **Hướng dẫn Kiro Workshop**, cho thấy cách áp dụng workflow spec-driven để xây tính năng agentic.

#### Những gì em mang về
- Mô hình tư duy rõ ràng về cách các agentic services của AWS ghép lại với nhau
- Một reference architecture có thể tùy biến cho use case của riêng mình
- Quyền truy cập toàn bộ source code để tự khám phá theo nhịp riêng

#### Một số hình ảnh khi tham gia sự kiện

![Agentic Build Day](/images/4-EventParticipated/event-3/1780046299367.jpg)

![Agentic Build Day](/images/4-EventParticipated/event-3/1780046308292.jpg)

![Agentic Build Day](/images/4-EventParticipated/event-3/1780046311474.jpg)

![Agentic Build Day](/images/4-EventParticipated/event-3/1780050160542.jpg)

> Tổng thể, **Agentic Build Day** cân bằng giữa cái tâm và kỹ thuật — cho đi với cộng đồng trong giờ đầu, rồi trao cho chúng em một blueprint hoàn chỉnh, dễ tùy biến để xây dựng hệ agentic thực sự trên AWS.
