## Kết luận lựa chọn

Sau khi đối chiếu rules của FCJuni với xu hướng AI Agent cuối 2025–2026, mình đề xuất chọn project:

# **CloudOps AI Agent: Serverless Incident Triage & Runbook Assistant on AWS**

Tên tiếng Việt có thể dùng trong báo cáo:

# **Trợ lý AI Agent phân tích sự cố hệ thống và gợi ý xử lý trên AWS**

Đây là ý tưởng phù hợp nhất vì nó vừa là **AI Agent theo xu hướng 2026**, vừa bám rất sát yêu cầu project: **AWS use-case thực tế, serverless application, monitoring system, dùng nhiều hơn 3 dịch vụ AWS, có log/metric/alert, có bảo mật IAM, có cleanup và dễ viết workshop step-by-step**.

---

## 1. Những yêu cầu quan trọng từ rules

Trang rules yêu cầu project nên là use-case thực tế trên AWS như **serverless application, data pipeline, monitoring system, IoT**, và phải dùng **ít nhất 3 dịch vụ AWS**. Project cũng cần thể hiện được thiết kế kiến trúc, sơ đồ, dịch vụ sử dụng, lý do chọn dịch vụ, triển khai end-to-end, log, metric, alert, tối ưu chi phí, bảo mật cơ bản và cleanup. ([Nội quy FC Juni][1])

Phần chấm điểm cũng ưu tiên project có bối cảnh rõ ràng, khách hàng rõ, vấn đề rõ, output cụ thể như dashboard/API/alert, và phải gắn với FCAJ/AWS, không quá chung chung hoặc lệch chủ đề cloud. ([Nội quy FC Juni][1])

Ngoài ra, báo cáo bắt buộc có **workshop website**, nội dung song ngữ Việt/Anh, hình ảnh minh họa, sơ đồ kiến trúc, code snippet, file đính kèm như CloudFormation/Dockerfile/script nếu phù hợp, và phần workshop phải tự triển khai như một project cá nhân chứ không copy workshop mẫu. ([Nội quy FC Juni][1])

Mẫu workshop cũng cho thấy cấu trúc cần có: Introduction, Prerequisite, các bước thực hành, test, policy/security và cleanup. ([Workshop Sample][2])

---

## 2. Vì sao không nên chọn agent quá “business SaaS”?

Các ý tưởng như AI SDR, HR Agent, Legal Agent, Accounting Agent hay Customer Support Agent đều có tính thị trường tốt, nhưng với bài FCJuni thì có 3 điểm yếu:

1. **Khó chứng minh AWS rõ ràng** nếu chỉ làm chatbot/API đơn giản.
2. **Khó có log/metric/alert đúng nghĩa cloud monitoring**.
3. **Dễ bị đánh giá là ứng dụng AI chung chung**, không đủ sâu về cloud architecture.

Rules yêu cầu project không lệch chủ đề cloud và phải thể hiện AWS service, luồng dữ liệu, IAM, CloudWatch, alarm, cleanup. Vì vậy ý tưởng tốt nhất nên là agent phục vụ chính bài toán cloud.

---

## 3. Xu hướng AI Agent 2025–2026 liên quan trực tiếp

Gartner dự báo đến cuối 2026, **40% enterprise applications sẽ có task-specific AI agents**, tăng từ dưới 5% trước đó. Điểm đáng chú ý là agent không còn chỉ là assistant trả lời câu hỏi, mà có khả năng thực hiện các task end-to-end, ví dụ cybersecurity threat response agent quét log, traffic, hành vi người dùng và khởi tạo phản ứng phù hợp. ([Gartner][3])

LangChain khảo sát hơn 1.300 chuyên gia và ghi nhận khi bước sang 2026, tổ chức không còn hỏi “có nên build agent không”, mà hỏi làm sao triển khai agent **reliable, efficient, scalable**. 57% respondent đã có agent production; 32% xem quality là rào cản lớn nhất; và gần 89% đã có observability cho agent. ([LangChain][4])

Google Cloud cũng mô tả agent 2026 là hệ thống có thể hiểu goal, lập kế hoạch nhiều bước và hành động dưới sự giám sát của con người; đồng thời agentic workflows sẽ trở thành một phần lõi trong business process, vượt xa chatbot hỏi đáp đơn giản. ([blog.google][5])

Vì vậy project nên thể hiện 4 thứ: **task-specific agent, workflow nhiều bước, action thật, observability**. CloudOps AI Agent đáp ứng đúng cả 4.

---

## 4. So sánh các ý tưởng AI Agent theo tiêu chí FCJuni

| Ý tưởng                               | Độ hợp rules AWS | Độ dễ demo | Độ “AI Agent 2026” |     Rủi ro | Đánh giá                                            |
| ------------------------------------- | ---------------: | ---------: | -----------------: | ---------: | --------------------------------------------------- |
| **CloudOps AI Agent phân tích sự cố** |            10/10 |     8.5/10 |               9/10 | Trung bình | **Nên chọn**                                        |
| Data Analyst Agent trên S3/Athena     |             9/10 |       8/10 |               8/10 | Trung bình | Rất tốt, nhưng ít “monitoring/alert” hơn            |
| Customer Support Agent trên Bedrock   |             7/10 |       8/10 |             8.5/10 | Trung bình | Dễ bị giống chatbot                                 |
| AI QA Agent chạy test web             |             8/10 |       6/10 |               9/10 |        Cao | Hay nhưng phức tạp, dễ quá sức                      |
| Accounting/Reconciliation Agent       |           6.5/10 |       7/10 |               8/10 |        Cao | Business tốt nhưng lệch cloud nếu không thiết kế kỹ |
| Internal Ops Agent                    |             6/10 |       7/10 |             8.5/10 | Trung bình | Tích hợp nhiều tool, khó gói thành workshop AWS     |

**Chọn CloudOps AI Agent** vì nó khớp gần như hoàn hảo với yêu cầu: monitoring system, log, metric, alert, IAM, serverless, end-to-end, chi phí, cleanup.

---

# Ý tưởng được chọn: CloudOps AI Agent

## 5. Bài toán

Khi một ứng dụng serverless trên AWS gặp lỗi, người vận hành thường phải tự làm nhiều bước:

* Mở CloudWatch Alarm.
* Xem CloudWatch Logs.
* Kiểm tra metric như error count, duration, throttle, invocation.
* Đọc runbook hoặc tài liệu xử lý lỗi.
* Tóm tắt nguyên nhân.
* Gửi thông báo cho team.
* Lưu lại incident để phân tích sau.

Project sẽ xây dựng một **AI Agent hỗ trợ triage sự cố**. Agent có thể nhận câu hỏi hoặc được kích hoạt bởi alarm, sau đó tự gọi các công cụ cần thiết để phân tích log/metric, đọc runbook, tạo báo cáo incident và gửi cảnh báo.

---

## 6. Người dùng mục tiêu

**Primary user:** sinh viên/dev mới học AWS, DevOps intern, cloud support intern.
**Secondary user:** team vận hành nhỏ cần assistant phân tích lỗi nhanh.

Đây là điểm mạnh khi trình bày báo cáo: project không chỉ “demo AI”, mà có bối cảnh thật trong cloud operation.

---

## 7. Output mong muốn

Project nên tạo ra các output cụ thể sau:

1. **API hoặc web UI** để hỏi agent:
   “Tại sao Lambda function bị lỗi trong 10 phút gần đây?”

2. **Incident summary** gồm:

   * Thời gian xảy ra lỗi.
   * Service bị ảnh hưởng.
   * Metric bất thường.
   * Log pattern nổi bật.
   * Root cause giả định.
   * Gợi ý xử lý theo runbook.
   * Mức độ nghiêm trọng: Low/Medium/High.

3. **CloudWatch Alarm** để chứng minh hệ thống có monitoring.

4. **SNS Email notification** gửi báo cáo incident.

5. **DynamoDB incident history** để lưu lại các lần phân tích.

6. **Workshop step-by-step** để người khác deploy, test lỗi, xem log, check metric, cleanup.

---

## 8. Kiến trúc đề xuất

```text
User / Tester
   |
   | 1. Gửi request hỏi về sự cố
   v
Amazon API Gateway
   |
   v
AWS Lambda - Chat/API Handler
   |
   v
Amazon Bedrock Agent
   |
   |-- Action Group 1: Query CloudWatch Logs
   |-- Action Group 2: Get CloudWatch Metrics
   |-- Action Group 3: Save Incident to DynamoDB
   |-- Action Group 4: Send Notification via SNS
   |
   |-- Knowledge Base / Runbook Source
   |      S3 bucket chứa runbook markdown/json
   |
   v
Agent Response
   |
   |-- CloudWatch Logs / Metrics / Alarm
   |-- DynamoDB incident table
   |-- SNS email alert
```

Amazon Bedrock Agents phù hợp vì agent có thể dùng foundation model, instructions, action groups và knowledge bases. Action group cho phép định nghĩa hành động agent được phép làm thông qua OpenAPI/function schema, và có thể dùng Lambda để thực thi logic nghiệp vụ. ([AWS Documentation][6])

Nếu dùng AgentCore Observability, project còn có thể trình bày trace/debug/monitor agent workflow, dashboard CloudWatch, metrics như latency, token usage, error rate. AWS docs mô tả AgentCore Observability cung cấp real-time visibility qua CloudWatch dashboards và telemetry cho session count, latency, duration, token usage, error rates. ([AWS Documentation][7])

---

## 9. AWS services nên dùng

Tối thiểu nên dùng 7–9 service để đủ nổi bật nhưng không quá phức tạp:

| Service                             | Vai trò trong project                                      |
| ----------------------------------- | ---------------------------------------------------------- |
| **Amazon Bedrock Agent**            | Điều phối AI Agent, hiểu yêu cầu, quyết định gọi tool      |
| **AWS Lambda**                      | Xử lý API, action group, query logs/metrics, save incident |
| **Amazon API Gateway**              | Public endpoint cho web UI hoặc Postman test               |
| **Amazon CloudWatch Logs**          | Lưu log ứng dụng demo và log Lambda                        |
| **Amazon CloudWatch Metrics/Alarm** | Tạo metric/alarm để trigger incident                       |
| **Amazon SNS**                      | Gửi email alert khi có incident                            |
| **Amazon DynamoDB**                 | Lưu incident history                                       |
| **Amazon S3**                       | Lưu runbook, sample logs, report hoặc static website       |
| **IAM**                             | Role/policy theo least privilege                           |

Có thể thêm **EventBridge** nếu muốn alarm tự kích hoạt workflow, nhưng MVP chưa bắt buộc.

---

## 10. MVP vừa sức

MVP nên giới hạn trong một ứng dụng demo đơn giản:

### Demo app

Một Lambda function tên `demo-order-service` có endpoint:

```text
POST /orders
```

Khi request có `simulateError=true`, Lambda cố tình throw lỗi hoặc timeout.

### Monitoring

* CloudWatch ghi log lỗi.
* CloudWatch metric filter đếm số dòng chứa `ERROR`.
* CloudWatch Alarm kích hoạt khi error count > threshold.

### Agent

Người dùng hỏi:

```text
Analyze latest incident for demo-order-service
```

Agent thực hiện:

1. Gọi Lambda action `get_recent_errors`.
2. Gọi Lambda action `get_metrics`.
3. Đọc runbook từ S3, ví dụ `lambda-timeout-runbook.md`.
4. Tạo incident summary.
5. Lưu kết quả vào DynamoDB.
6. Gửi SNS email nếu severity là High.

---

## 11. Vì sao ý tưởng này dễ đạt điểm cao?

### 4.1. Ý tưởng & mục tiêu

Rất rõ: hệ thống dùng để phân tích sự cố cloud/serverless. Khách hàng là DevOps intern/cloud support. Vấn đề là mất thời gian khi đọc log, metric, runbook thủ công. Output là API, incident report, alert, incident database.

### 4.2. Kiến trúc & thiết kế kỹ thuật

Có sơ đồ kiến trúc rõ, nhiều AWS service, luồng dữ liệu cụ thể. Dễ giải thích vì sao chọn từng service:

* Lambda: serverless, dễ demo, chi phí thấp.
* API Gateway: tạo endpoint chuẩn.
* CloudWatch: log/metric/alarm đúng yêu cầu project.
* SNS: notification đơn giản.
* DynamoDB: lưu incident history serverless.
* S3: lưu runbook/report.
* Bedrock Agent: phần AI Agent trung tâm.

### 4.3. Triển khai & lab step-by-step

Rất dễ chia workshop:

1. Tạo IAM role.
2. Deploy demo Lambda.
3. Tạo API Gateway.
4. Tạo CloudWatch log group/metric filter/alarm.
5. Tạo S3 runbook.
6. Tạo DynamoDB table.
7. Tạo SNS topic.
8. Tạo Bedrock Agent.
9. Tạo action group Lambda.
10. Test lỗi.
11. Xem log/metric/alarm.
12. Hỏi agent.
13. Nhận incident report.
14. Cleanup.

### 4.4. Tài liệu workshop

Dễ có screenshot, code block, CLI command, hình kiến trúc, test cases, kết quả mong đợi.

### 4.5. Đóng góp cá nhân

Có nhiều điểm tùy biến:

* Thêm severity scoring.
* Thêm nhiều runbook.
* Thêm dashboard incident.
* Thêm EventBridge auto-trigger.
* Thêm cost estimation.
* Thêm AgentCore Observability nếu có thời gian.

---

## 12. Phạm vi nên làm để không quá rộng

Không nên làm “agent quản trị toàn bộ AWS account”. Quá rộng và rủi ro IAM.

Nên giới hạn thành:

> Agent chỉ phân tích một demo serverless application gồm API Gateway + Lambda + CloudWatch. Agent chỉ có quyền đọc log/metric cần thiết, đọc runbook từ S3, ghi incident vào DynamoDB và gửi SNS notification.

Đây là phạm vi đủ thực tế, đủ AWS, đủ an toàn và đủ để hoàn thành workshop.

---

## 13. Tên project đề xuất

Có thể chọn một trong các tên sau:

1. **CloudOps AI Agent on AWS**
2. **Serverless Incident Triage Agent**
3. **AWS Runbook Assistant Agent**
4. **AI-Powered CloudWatch Incident Analyzer**
5. **Trợ lý AI phân tích sự cố Serverless trên AWS**

Tên mình khuyên dùng trong báo cáo:

# **CloudOps AI Agent: Serverless Incident Triage & Runbook Assistant on AWS**

Nghe chuyên nghiệp, thể hiện rõ cloud + AI Agent + use-case thực tế.

---

## 14. Ba bài blog phù hợp để post

Rules yêu cầu 3 bài blog. Với project này, 3 bài rất tự nhiên:

1. **Amazon Bedrock Agents là gì và cách dùng Action Groups với AWS Lambda**
2. **Xây dựng monitoring cho serverless application bằng CloudWatch Logs, Metrics và Alarm**
3. **Tối ưu chi phí và bảo mật khi xây dựng AI Agent trên AWS**

Ba bài này vừa phục vụ báo cáo, vừa liên quan trực tiếp đến project.

---

## 15. Timeline 12 tuần gợi ý

| Tuần    | Công việc                                       |
| ------- | ----------------------------------------------- |
| Week 1  | Nghiên cứu rules, chọn đề tài, viết proposal    |
| Week 2  | Học Bedrock Agent, Lambda, CloudWatch, SNS      |
| Week 3  | Thiết kế kiến trúc, IAM policy, data flow       |
| Week 4  | Build demo serverless app API Gateway + Lambda  |
| Week 5  | Thêm CloudWatch Logs, metric filter, alarm      |
| Week 6  | Tạo S3 runbook và DynamoDB incident table       |
| Week 7  | Tạo Bedrock Agent và action group Lambda        |
| Week 8  | Kết nối agent với CloudWatch/DynamoDB/SNS       |
| Week 9  | Test incident flow end-to-end                   |
| Week 10 | Tối ưu IAM, cost, cleanup script                |
| Week 11 | Viết workshop song ngữ, thêm screenshot/code    |
| Week 12 | Hoàn thiện báo cáo, self-evaluation, demo video |

---

## 16. Phiên bản nâng cao nếu còn thời gian

Sau khi MVP chạy ổn, có thể thêm:

* **EventBridge auto-trigger**: alarm tự gọi workflow phân tích.
* **AgentCore Observability**: trace agent workflow, latency, token usage, error rate.
* **Dashboard incident**: static web trên S3/CloudFront.
* **Multi-runbook matching**: agent chọn runbook phù hợp theo lỗi.
* **Cost guardrail**: cảnh báo nếu Bedrock token usage vượt ngưỡng.
* **Human approval**: agent chỉ gửi đề xuất, không tự sửa resource.

---

## 17. Quyết định cuối cùng

Nên chọn:

# **CloudOps AI Agent: Serverless Incident Triage & Runbook Assistant on AWS**

Lý do chính:

* Bám sát rules nhất: AWS, serverless, monitoring, log, metric, alert, IAM, cleanup.
* Đủ yếu tố AI Agent hiện đại: agent có tool/action, phân tích nhiều bước, tạo báo cáo, gửi notification.
* Dễ triển khai MVP trong thời gian thực tập.
* Dễ viết workshop song ngữ với screenshot, code, kiến trúc, test case.
* Có chiều sâu để đạt điểm tốt nhưng không quá rộng như các agent business khác.

Nếu triển khai đúng, đây sẽ là project vừa “đúng đề”, vừa có tính xu hướng 2026, vừa chứng minh năng lực AWS thực tế.

[1]: https://rules.fcjuni.com/3-project/ "Quy định về project :: Báo cáo thực tập"
[2]: https://workshop-sample.fcjuni.com/ "Internship Report :: Internship Report"
[3]: https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025 "Gartner Predicts 40% of Enterprise Apps Will Feature Task-Specific AI Agents by 2026, Up from Less Than 5% in 2025"
[4]: https://www.langchain.com/state-of-agent-engineering "State of AI Agents"
[5]: https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/ai-business-trends-report-2026/ "Google Cloud’s Business Trends Report 2026: Key findings"
[6]: https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html "How Amazon Bedrock Agents works - Amazon Bedrock"
[7]: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html "Observe your agent applications on Amazon Bedrock AgentCore Observability - Amazon Bedrock AgentCore"
