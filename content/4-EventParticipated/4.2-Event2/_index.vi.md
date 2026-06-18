---
title: "AWS First Cloud AI Journey - Community Day"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

<!--{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}-->

# Bài thu hoạch “AWS First Cloud AI Journey – Community Day”

### Mục đích của sự kiện

- Kết nối cộng đồng **FCAJ (First Cloud AI Journey)** tại TP. Hồ Chí Minh
- Chia sẻ kinh nghiệm thực tế khi làm việc với **AI / GenAI** trong môi trường production
- Cập nhật các dịch vụ **AWS** hữu ích cho AI workloads (CloudFront, bộ Amazon Quick)
- Truyền cảm hứng nghề nghiệp qua các câu chuyện hackathon và case study doanh nghiệp

### Danh sách diễn giả

- **Tinh Truong** – Platform Engineer tại GoTymeX
- **Thinh Nguyen** – DevOps Engineer | FCAJ Ambassador
- **Anh Pham** – Cloud Consultant tại G-AsiaPacific Vietnam
- **Uyen Le** – GenAI Engineer tại VIB
- **Thao Nguyen** – GenAI Engineer tại VIB
- **Mai Nguyen** – GenAI Engineer tại VIB
- **Duc Dao** – Solutions Architect tại Cloud Kinetics
- **Vy Lam** – Senior Business Systems Analyst tại VPBank

### Lịch trình sự kiện

| Thời gian | Diễn giả | Chủ đề |
|---|---|---|
| 09:00 – 09:30 | **Tinh Truong** | Context Is Everything: Making AI Actually Work for You |
| 09:30 – 09:45 | **Anh Pham** | Friendly AI Assistant with Amazon Quick |
| 09:45 – 10:25 | **Thinh Nguyen** | From Edge To Origin: CloudFront as Your Foundation |
| 10:25 – 10:55 | **Uyen Le, Thao Nguyen, Mai Nguyen** | 36 hrs with LotusHacks – Building UTMorpho from Idea to Reality |
| 10:55 – 11:00 | — | Giải lao |
| 11:00 – 11:30 | **Duc Dao** | Non-Determinism of "Deterministic" LLM Settings |
| 11:30 – 12:00 | **Vy Lam** | Enterprise-Grade Multi-Agent System: The Case of Startup Credit Scoring |

### Nội dung nổi bật

#### Tinh Truong – Context Is Everything: Làm AI thực sự hữu ích

- Vì sao AI thất bại khi thiếu context, và “context” thực sự là gì
- Từ prompt tới memory: AI đang tiến hóa thế nào với khái niệm **Second AI Brain**
- Context tốt hơn dẫn đến kết quả tốt hơn — mindset và tips thực tế
- Định hướng nghề nghiệp và cách sinh viên có thể bắt đầu xây dựng cùng AI

#### Anh Pham – Friendly AI Assistant với Amazon Quick

- **Quick Chat Agent**: trợ lý AI để khám phá và phân tích dữ liệu
- **Quick Flows**: xây dựng workflow thông minh bằng ngôn ngữ tự nhiên, không cần code
- **Quick Spaces**: không gian cộng tác biến insight cá nhân thành tri thức team
- **Quick Sight**: dựng dashboard và report từ dữ liệu thô bằng natural language

#### Thinh Nguyen – From Edge To Origin: CloudFront làm nền tảng

- Amazon CloudFront cho mọi loại workload
- Tối ưu chi phí với CloudFront
- Khả năng bảo mật ngay tại edge
- Tăng độ tin cậy và hiệu năng với CloudFront

#### Uyen Le, Thao Nguyen, Mai Nguyen – 36 giờ tại LotusHacks: Xây dựng UTMorpho

- Vì sao team tham gia **LotusHacks**
- Hành trình brainstorm ý tưởng từ con số 0
- Định hình vấn đề và xây dựng sản phẩm **UTMorpho**
- Sprint 36 giờ dưới áp lực thời gian
- Những thử thách, thất bại và bước ngoặt quan trọng
- Demo sản phẩm và các bài học rút ra

#### Duc Dao – Non-Determinism của LLM khi cấu hình "Deterministic"

- Cách LLM chọn token tiếp theo
- Giả định phổ biến: **Temperature = 0** đảm bảo kết quả tái lập
- Thực tế: các tối ưu inference khiến giả định này bị phá vỡ
- Tác động trong production và các chiến lược giảm thiểu

#### Vy Lam – Multi-Agent System cấp doanh nghiệp: Chấm điểm tín dụng startup

- Sự lệch pha giữa hệ thống ngân hàng truyền thống và dữ liệu startup
- **Single Agent**: khi nào nên và khi nào không nên dùng
- Mô hình **Multi-Agent** cho các bài toán phức tạp
- Blueprint của một Virtual Credit Committee
- Guardrails, compliance, ROI vận hành và lộ trình triển khai

### Những gì học được

#### Tư duy khi làm việc với AI

- **Context engineering** quan trọng hơn việc viết prompt khéo léo
- Coi AI như một đồng đội cần **memory, tools và grounding**, không phải search engine
- **Determinism chỉ là ảo giác** — luôn thiết kế để chấp nhận biến động, kể cả khi `temperature=0`

#### Kiến trúc & AWS

- **CloudFront** không chỉ là CDN, mà có thể là nền tảng cho cả bảo mật và hiệu năng
- Bộ **Amazon Quick** giúp người không phải engineer cũng có thể xây dựng workflow AI
- Với domain phức tạp, **multi-agent systems** kèm guardrails sẽ vượt trội so với single agent

#### Nghề nghiệp & cộng đồng

- Hackathon như **LotusHacks** là cách nhanh để ship sản phẩm thật và học dưới áp lực
- AI cấp doanh nghiệp cần **compliance, ROI tracking và roadmap rõ ràng**, không chỉ demo
- Tham gia cộng đồng **FCAJ** giúp học hỏi nhanh từ những người đang làm thật trong production

### Ứng dụng vào công việc

- Thêm **context layer** (memory, retrieval, tools) cho các trợ lý AI nội bộ thay vì chỉ dùng prompt thô
- Thử nghiệm **Amazon Quick Flows / Quick Sight** cho các use case báo cáo nội bộ
- Rà soát các tính năng đang dùng LLM để phát hiện **rủi ro non-determinism** và thêm guardrails / eval
- Đánh giá dùng **CloudFront** cho static asset và bảo mật cơ bản tại edge cho project mới
- Phác thảo thiết kế **multi-agent** cho những workflow đang “gồng” trong một prompt duy nhất

### Trải nghiệm trong event

Tham gia **AWS First Cloud AI Journey – Community Day** ngày 23/05/2026 là một buổi sáng đầy cảm hứng với nội dung kỹ thuật sâu và những câu chuyện rất thực tế. Một số khoảnh khắc nổi bật:

#### Học từ dàn diễn giả đa dạng
- Diễn giả đến từ nhiều mảng khác nhau — ngân hàng (**VPBank, VIB**), platform engineering (**GoTymeX**), AWS partner (**G-AsiaPacific, Cloud Kinetics**) và chính cộng đồng **FCAJ**.
- Mỗi talk chạm vào một tầng khác nhau của AI stack: prompting, tooling, infrastructure, kiến trúc enterprise.

#### Bài học kỹ thuật rõ ràng
- **Tinh Truong** thay đổi cách em nhìn về prompting — context trước, prompt sau.
- **Duc Dao** phá vỡ niềm tin rằng `temperature=0` là đủ để output tái lập được.
- **Vy Lam** cho thấy multi-agent system đã được dùng trong domain tài chính nghiêm túc, không chỉ là demo.

#### Tiếp cận AWS thực tế
- **Anh Pham** demo bộ **Amazon Quick** — hạ thấp rào cản khi xây tool AI nội bộ.
- **Thinh Nguyen** đào sâu **CloudFront** ngoài góc CDN — bảo mật, chi phí, độ tin cậy mà em từng dùng chưa hết.

#### Cảm hứng từ những người trẻ
- Câu chuyện **UTMorpho / LotusHacks** từ **Uyen Le, Thao Nguyen, Mai Nguyen** là minh chứng rằng 36 giờ vẫn đủ để ship một sản phẩm có ý nghĩa nếu có team đúng.

#### Networking
- Gặp gỡ các bạn FCAJ/FCJ tại tầng 26 Bitexco, trao đổi về thực tập, lộ trình học AWS và các project AI hiện tại.

#### Một số hình ảnh khi tham gia sự kiện

![FCAJ Community Day](/images/4-EventParticipated/event-2/fcaj-community.jpg)

![Nguyen Gia Hung](/images/4-EventParticipated/event-2/NguyenGiaHung.jpg)

![Tinh Truong – Platform Engineer @ GoTymeX](/images/4-EventParticipated/event-2/TinhTruong.jpg)

![Thinh Nguyen – DevOps Engineer | FCAJ Ambassador](/images/4-EventParticipated/event-2/ThinhNguyen.jpg)

![Anh Pham – Cloud Consultant @ G-AsiaPacific Vietnam](/images/4-EventParticipated/event-2/AnhPham.jpg)

![Uyen Le, Thao Nguyen, Mai Nguyen – GenAI Engineers @ VIB](/images/4-EventParticipated/event-2/UyenLe-ThaoNguyen-MaiNguyen.jpg)

![Duc Dao – Solutions Architect @ Cloud Kinetics](/images/4-EventParticipated/event-2/DucDao.jpg)

![Vy Lam – Senior Business Systems Analyst @ VPBank](/images/4-EventParticipated/event-2/VyLam.jpg)

> Tổng thể, sự kiện củng cố một niềm tin: **xây dựng sản phẩm AI hữu ích chủ yếu là chuyện context, guardrails và năng lực thực thi của team** — và cộng đồng FCAJ tại HCMC là nơi rất tốt để cùng học điều đó.
