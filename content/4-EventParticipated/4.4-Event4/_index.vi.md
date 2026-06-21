---
title: "Amazon Quick & Kiro Fiesta #2"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

<!--{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}-->

# Bài thu hoạch “Amazon Quick & Kiro Fiesta #2”

### Mục đích của sự kiện

- Một workshop AI hands-on trọn ngày do AWS tổ chức, chia thành hai phiên cho hai nhóm đối tượng
- **Buổi sáng – Amazon Quick:** cho các lãnh đạo kinh doanh và tài chính thấy một trợ lý điều hành chạy bằng AI giúp tinh gọn việc ra quyết định ra sao, từ morning brief đến phân tích chiến lược, không yêu cầu kỹ năng kỹ thuật
- **Buổi chiều – Kiro:** cho các đội hạ tầng thấy Kiro — AI-native IDE của AWS — kết hợp năng suất AI với guardrails cấp doanh nghiệp cho Security, DevOps và FinOps
- Dành thời gian hands-on để người tham dự tự cấu hình Amazon Quick workspace của mình và set up Kiro với sự hướng dẫn của AWS Solutions Architect

### Đơn vị tổ chức

- **Phiên sáng (Amazon Quick):** Đội ngũ AWS từ Singapore
- **Phiên chiều (Kiro):** Đội ngũ AWS Vietnam & các AWS Solutions Architects
- AWS GenAI Builders Club

### Lịch trình sự kiện

#### Phiên sáng: Amazon Quick — “A Day in the Life of a CxO”

| Thời gian | Nội dung |
|---|---|
| 9:00 – 9:30 | Morning Brief — AI tổng hợp cập nhật qua đêm, gắn cờ ngoại lệ, chuẩn bị snapshot trong ngày |
| 9:30 – 10:30 | Financial Deep-Dive — Budget vs actual, dự báo dòng tiền, case study (Zurich, Jabil tiết kiệm $400K) |
| 10:30 – 11:15 | Strategic Analysis — Quick Research: trả lời câu hỏi phức tạp trong vài phút từ dữ liệu của bạn |
| 11:15 – 12:00 | Hands-On — Tự cấu hình Amazon Quick workspace với hướng dẫn của AWS SA |
| 12:00 – 1:30 | 🍽️ Ăn trưa & Networking |

#### Phiên chiều: Kiro — “KiroOps”

| Thời gian | Nội dung |
|---|---|
| 1:30 – 1:45 | Introduction — GenAI cho hạ tầng: use case cho DevOps, Security, FinOps, SRE |
| 1:45 – 2:15 | Getting Started — Set up Kiro IDE/CLI, khám phá chế độ Supervised vs Autopilot |
| 2:15 – 3:00 | Empowering Your Kiro — Agent Hooks (auto cfn-lint, chặn lệnh phá hủy), Steering (security baseline, tagging), AWS MCP Server |
| 3:00 – 3:15 | ☕ Giải lao |
| 3:15 – 3:45 | Enterprise Governance — SCPs + user-agent markers, quản lý chi phí, CloudTrail audit |
| 3:45 – 4:15 | Troubleshoot Lab — Chẩn đoán một sự cố hạ tầng thực tế với Kiro (network, IAM, cost) |
| 4:15 – 4:30 | Wrap-up — Bước tiếp theo, Assisted Trial Program, Q&A |

### Nội dung nổi bật

#### Buổi sáng – Amazon Quick: “Trợ lý điều hành chạy bằng AI 24/7”

- Vấn đề đặt ra: lãnh đạo mất hơn 40 giờ mỗi tháng để chuyển qua lại giữa các app, tổng hợp dữ liệu và chờ câu trả lời
- Đi qua một ngày làm việc thật của CxO để thấy **Amazon Quick** đóng vai trò AI Chief of Staff đã hiểu sẵn dữ liệu, con người và ưu tiên của bạn
- **Morning Brief:** trợ lý tổng hợp cập nhật qua đêm, gắn cờ ngoại lệ và chuẩn bị snapshot trong ngày
- **Financial Deep-Dive:** budget vs actual, dự báo dòng tiền, kèm case study thực tế (Zurich, Jabil tiết kiệm $400K)
- **Strategic Analysis:** Quick Research trả lời câu hỏi phức tạp trong vài phút ngay từ dữ liệu của bạn
- **Hands-On:** người tham dự tự cấu hình Amazon Quick workspace với hướng dẫn của AWS SA — không cần kỹ năng kỹ thuật

#### Buổi chiều – Kiro: “KiroOps cho đội hạ tầng”

- Vấn đề đặt ra: đội hạ tầng đang dùng AI coding tools và tốc độ tăng lên, nhưng ai đảm bảo AI tuân theo security baseline, enforce tagging và kiểm soát những gì AI được làm trong tài khoản AWS?
- **Getting Started:** set up Kiro IDE/CLI và khám phá chế độ **Supervised vs Autopilot**
- **Empowering Your Kiro:** **Agent Hooks** (auto cfn-lint, chặn lệnh phá hủy), **Steering** (security baseline, chính sách tagging) và **AWS MCP Server**
- **Enterprise Governance:** SCPs kết hợp user-agent markers, quản lý chi phí và CloudTrail audit
- **Troubleshoot Lab:** chẩn đoán một sự cố hạ tầng thực tế với Kiro qua network, IAM và cost
- **Wrap-up:** bước tiếp theo, Assisted Trial Program và Q&A

### Những gì học được

- Các trợ lý AI như **Amazon Quick** có thể bỏ bớt khối lượng việc lặp lại của lãnh đạo — thu thập dữ liệu, báo cáo, nghiên cứu ad-hoc — và trả lại thời gian cho họ
- Self-service analytics dựa trên chính dữ liệu của bạn rút ngắn đường đi từ câu hỏi đến quyết định
- Với đội kỹ thuật, năng suất AI phải đi kèm **guardrails**: Agent Hooks, Steering và governance giúp việc dùng AI an toàn và tuân thủ
- Chế độ **Supervised vs Autopilot** của Kiro cho phép đội chọn mức kiểm soát phù hợp cho từng task
- Enterprise governance — **SCPs, user-agent markers, kiểm soát chi phí và CloudTrail audit** — là thứ giúp tooling AI an toàn khi áp dụng ở quy mô lớn

### Ứng dụng vào công việc

- Áp dụng pattern của **Amazon Quick** để tự động hóa báo cáo và nghiên cứu định kỳ thay vì chuyển app thủ công
- Dùng **Kiro Agent Hooks** để tự chạy cfn-lint và chặn lệnh phá hủy trong workflow của mình
- Dùng **Steering files** để mã hóa security baseline và chính sách tagging nhằm giữ output của AI nhất quán
- Kết nối **AWS MCP Server** để cho Kiro truy cập context AWS an toàn và có giới hạn
- Mặc định dùng **Supervised mode** cho các thay đổi hạ tầng rủi ro cao và **Autopilot** cho task thường ngày

### Đối tượng nên tham gia

- **Buổi sáng (Amazon Quick):** CxO, VP, Finance Director, CFO, FP&A Analyst, Controller, Head of Operations và IT Director cần self-service analytics
- **Buổi chiều (Kiro):** Platform Engineer, DevOps Engineer, Cloud Architect, Security Engineer, Compliance Officer, FinOps Practitioner, SRE / Operations Engineer và Engineering Manager phụ trách đội hạ tầng

### Trải nghiệm trong event

Tham gia **Amazon Quick & Kiro Fiesta #2** ngày 19/06/2026 tại văn phòng AWS Vietnam là một trải nghiệm trọn ngày dày đặc, bao quát cả khía cạnh kinh doanh lẫn kỹ thuật của việc áp dụng AI. Một số khoảnh khắc nổi bật:

#### Góc nhìn lãnh đạo buổi sáng
- Phiên **Amazon Quick** đặt AI vào một ngày làm việc thật của lãnh đạo, làm giá trị trở nên rõ ràng: bớt thời gian tổng hợp dữ liệu, thêm thời gian ra quyết định.
- Các case study tài chính (Zurich, Jabil tiết kiệm $400K) cho thấy kết quả cụ thể chứ không phải hứa hẹn trừu tượng.
- Phần tự set up workspace khiến công cụ trở nên dễ tiếp cận kể cả khi không có nền tảng kỹ thuật.

#### Góc nhìn kỹ thuật buổi chiều
- Phiên **Kiro** nói thẳng vào nỗi lo của mọi đội hạ tầng: làm sao có tốc độ AI mà không mất kiểm soát.
- Thấy **Agent Hooks** tự chạy cfn-lint và chặn lệnh phá hủy giúp khái niệm guardrails trở nên cụ thể.
- **Steering** cho security baseline và tagging, cùng **AWS MCP Server**, cho thấy cách giữ output AI bám theo chính sách.
- **Troubleshoot Lab** là phần hay nhất — chẩn đoán một sự cố network/IAM/cost thật với Kiro biến lý thuyết thành thực hành.

#### Những gì em mang về
- Bức tranh rõ ràng về cách AI hỗ trợ cả lãnh đạo lẫn kỹ sư, mỗi bên trong workflow riêng
- Các pattern guardrails thực dụng có thể áp dụng với Kiro ngay
- Biết đến Assisted Trial Program để tiếp tục học hỏi

#### Một số hình ảnh khi tham gia sự kiện

![Amazon Quick & Kiro Fiesta #2](/images/4-EventParticipated/event-4/1781862217262.jpg)

![Amazon Quick & Kiro Fiesta #2](/images/4-EventParticipated/event-4/1781862219032.jpg)

![Amazon Quick & Kiro Fiesta #2](/images/4-EventParticipated/event-4/1781914266028.jpg)

> Tổng thể, **Amazon Quick & Kiro Fiesta #2** kết nối hai nhóm đối tượng trong một ngày — cho lãnh đạo thấy AI trả lại thời gian cho lịch làm việc của họ, và cho kỹ sư thấy cách áp dụng AI với guardrails cấp doanh nghiệp tích hợp sẵn.
