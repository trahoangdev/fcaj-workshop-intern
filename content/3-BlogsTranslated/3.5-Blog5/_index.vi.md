---
title: "Blog 5"
date: 2025-02-11
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
---

# Xây dựng Cơ chế Retry Tùy chỉnh Không Máy chủ cho Stateless Queue Consumer

## Nguồn tham khảo

- AWS Architecture Blog: [Create a serverless custom retry mechanism for stateless queue consumers](https://aws.amazon.com/vi/blogs/architecture/create-a-serverless-custom-retry-mechanism-for-stateless-queue-consumers/)
- Tác giả: Kaizad Wadia
- Ngày phát hành: 11/02/2025

## Tổng quan

Trong kiến trúc serverless, một pattern rất phổ biến là AWS Lambda đọc message từ Amazon SQS rồi gọi downstream service hoặc external API. Khi downstream service lỗi tạm thời hoặc bị rate limit, hệ thống cần retry để không mất message. Tuy nhiên, retry quá nhanh có thể làm downstream service quá tải hơn, còn retry không kiểm soát có thể làm message nhanh chóng rơi vào Dead-Letter Queue (DLQ).

Bài viết của AWS đề xuất một pattern dùng **Amazon EventBridge Scheduler** để tạo cơ chế retry tùy chỉnh cho stateless queue consumers. Ý tưởng chính là đưa phần "chờ đến lần retry tiếp theo" ra khỏi Lambda. Lambda chỉ xử lý message và quyết định thời điểm retry, còn EventBridge Scheduler chịu trách nhiệm gọi lại SQS vào thời điểm tương lai.

## Vấn đề cần giải quyết

SQS và Lambda đã có cơ chế retry mặc định, nhưng không phải lúc nào cũng đủ linh hoạt. Một số tình huống thực tế cần kiểm soát retry chi tiết hơn:

- Downstream API trả về HTTP 429 do rate limit.
- External service bị outage ngắn hạn và cần chờ lâu hơn trước khi retry.
- Mỗi loại lỗi cần retry interval khác nhau.
- Một số lỗi nên retry, một số lỗi nên đưa thẳng vào DLQ.
- Hệ thống cần exponential backoff hoặc linear backoff tùy use case.
- Lambda không nên sleep để chờ, vì vẫn bị tính chi phí trong thời gian idle.

Nếu workflow state đã được quản lý bởi Step Functions thì có thể dùng built-in retry/backoff của Step Functions. Nhưng với stateless queue consumer, bài viết đưa ra một cách nhẹ hơn: dùng EventBridge Scheduler như một scheduler bên ngoài.

## Kiến trúc giải pháp

![Kiến trúc custom retry với EventBridge Scheduler](/fcaj-workshop-intern/images/3-BlogsTranslated/Blog5/architecture.png)

*Nguồn: AWS Architecture Blog - Create a serverless custom retry mechanism for stateless queue consumers*

Luồng xử lý chính gồm các bước:

1. Lambda nhận message từ Amazon SQS.
2. Lambda xử lý message và gọi downstream service.
3. Nếu gặp lỗi cần retry, Lambda catch lỗi thay vì để invocation fail theo cơ chế mặc định.
4. Lambda tính thời điểm retry tiếp theo dựa trên error type, số lần retry trước đó hoặc backoff policy.
5. Lambda tạo một schedule trong Amazon EventBridge Scheduler.
6. Đến thời điểm đã định, EventBridge Scheduler gửi message trở lại SQS.
7. Lambda xử lý lại message khi message xuất hiện trong queue.
8. Nếu số lần retry vượt giới hạn hoặc lỗi không nên retry, message được đưa vào DLQ.

Điểm quan trọng là message không nằm trong Lambda trong thời gian chờ. Hệ thống không phải trả chi phí compute cho việc idle, đồng thời vẫn kiểm soát được thời điểm retry.

## Các dịch vụ AWS liên quan

| Dịch vụ | Vai trò trong giải pháp |
| --- | --- |
| Amazon SQS | Lưu message chính và cung cấp DLQ cho message thất bại |
| AWS Lambda | Consumer xử lý message, phát hiện lỗi và tạo retry schedule |
| Amazon EventBridge Scheduler | Lên lịch đưa message trở lại SQS trong tương lai |
| AWS IAM | Cấp quyền Lambda tạo schedule, pass role và gửi message vào DLQ |
| Amazon CloudWatch | Theo dõi logs, metrics, retry behavior và DLQ usage |
| AWS PrivateLink | Cho phép Lambda trong VPC gọi EventBridge Scheduler riêng tư khi cần |

## Điểm kỹ thuật đáng chú ý

### Retry timing linh hoạt hơn

Với pattern này, delay của mỗi message có thể được tính riêng. Ví dụ lỗi HTTP 429 có thể chờ 5 phút, lỗi timeout có thể retry sau 30 giây, còn lỗi validation thì đưa thẳng vào DLQ. Cách này linh hoạt hơn việc chỉ dùng visibility timeout chung cho toàn bộ queue.

### Theo dõi retry bằng message attributes

Bài viết đề xuất dùng SQS message attributes hoặc dữ liệu trong message body để lưu retry attempts. Mỗi lần retry, Lambda cập nhật thông tin số lần retry hoặc timestamp. Nếu vượt quá retry limit, message không được schedule tiếp mà được đưa vào DLQ.

### Idempotency và partial failures

Một rủi ro lớn là partial failure. Ví dụ Lambda đã ghi dữ liệu vào database thành công, nhưng gọi external API thất bại. Khi retry, toàn bộ message có thể được xử lý lại từ đầu. Vì vậy, consumer nên được thiết kế idempotent: có thể chạy lại cùng một message mà không tạo dữ liệu trùng hoặc trạng thái sai.

### Scheduler không phải đồng hồ tuyệt đối

EventBridge Scheduler có độ phân giải ở mức phút và có thêm độ trễ giữa scheduler, SQS và Lambda. Vì vậy, pattern này phù hợp với retry có độ trễ tính bằng phút hoặc dài hơn, không phù hợp cho các workflow yêu cầu retry chính xác từng giây.

### Monitoring và bảo mật

Hệ thống cần theo dõi Lambda errors, duration, invocation count, số lượng schedule được tạo, số message trong DLQ và pattern retry bất thường. Về bảo mật, Lambda role chỉ nên có quyền tối thiểu để tạo schedule, pass scheduler role, đọc SQS và gửi message vào DLQ.

## Điều mình học được

Trước khi đọc bài này, mình thường nghĩ retry của SQS/Lambda hoặc Step Functions là hai lựa chọn chính. Sau khi tìm hiểu, mình thấy EventBridge Scheduler tạo ra một lựa chọn trung gian rất hợp lý: nhẹ hơn Step Functions nhưng linh hoạt hơn retry mặc định.

Mình cũng thấy bài học quan trọng là không nên trả tiền cho thời gian chờ trong Lambda. Nếu hệ thống chỉ cần "thử lại sau", việc chờ nên được giao cho scheduler, còn Lambda chỉ nên chạy khi có việc cần xử lý thật sự.

## Kết luận

Bài viết này đưa ra một pattern serverless thực tế để xử lý retry cho stateless queue consumers. Bằng cách kết hợp Amazon SQS, AWS Lambda, EventBridge Scheduler và DLQ, hệ thống có thể kiểm soát retry timing linh hoạt hơn, giảm áp lực lên downstream service và tránh chi phí compute không cần thiết.

Nếu áp dụng vào production, mình nghĩ cần bắt đầu từ việc phân loại lỗi rõ ràng, đặt retry limit hợp lý, thiết kế consumer idempotent, theo dõi DLQ sát sao và kiểm tra IAM permissions để scheduler chỉ có đúng quyền gửi message trở lại queue.
