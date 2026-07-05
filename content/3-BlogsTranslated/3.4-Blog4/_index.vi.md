---
title: "Blog 4"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Xây dựng Tầng Tìm kiếm Người dùng Có khả năng Mở rộng trên Amazon Cognito

## Nguồn tham khảo

- AWS Architecture Blog: [Building a scalable user search layer on top of Amazon Cognito](https://aws.amazon.com/vi/blogs/architecture/building-a-scalable-user-search-layer-on-top-of-amazon-cognito/)
- Tác giả: Philip Chen và Varun Selvaraj
- Ngày phát hành: 01/06/2026

## Tổng quan

Amazon Cognito là dịch vụ mạnh cho xác thực và quản lý người dùng. Tuy nhiên, khi ứng dụng phát triển lớn hơn, nhu cầu tìm kiếm người dùng thường vượt ra ngoài những gì API mặc định như `ListUsers` có thể xử lý tốt. Ví dụ, đội hỗ trợ có thể cần tìm người dùng bằng một phần email, lọc theo nhóm, kiểm tra nhiều custom attributes hoặc cần fuzzy search với thời gian phản hồi dưới một giây.

Bài viết của AWS đề xuất một cách tiếp cận rõ ràng: không ép Cognito trở thành search engine, mà xây một tầng tìm kiếm riêng bên trên Cognito bằng AWS Lambda, Amazon DynamoDB, DynamoDB Streams và Amazon OpenSearch Serverless. Cognito tiếp tục làm nhiệm vụ identity provider, còn OpenSearch đảm nhận các truy vấn tìm kiếm phức tạp.

## Vấn đề cần giải quyết

Với các use case đơn giản, `ListUsers` có thể đủ dùng. Nhưng trong môi trường production có số lượng người dùng lớn, yêu cầu tìm kiếm thường phức tạp hơn:

- Tìm kiếm theo một phần email, tên hoặc số điện thoại.
- Tìm người dùng theo group membership hoặc app client.
- Lọc đồng thời theo nhiều thuộc tính tiêu chuẩn và custom attributes.
- Hỗ trợ exact match, prefix match và fuzzy search.
- Trả kết quả nhanh, có pagination và phù hợp với frontend/API.
- Đồng bộ dữ liệu gần thời gian thực khi người dùng đăng ký, đăng nhập hoặc được admin cập nhật.

Điểm khó nằm ở việc giữ search index luôn đồng bộ với Cognito. Nếu chỉ chạy batch job định kỳ, dữ liệu có thể bị trễ. Nếu chỉ dùng Cognito Lambda triggers, một số thao tác admin trong console hoặc CLI có thể không được bắt đầy đủ. Vì vậy, bài viết kết hợp nhiều nguồn sự kiện để giảm rủi ro lệch dữ liệu.

## Kiến trúc giải pháp

![Kiến trúc tầng tìm kiếm người dùng trên Amazon Cognito](../../../images/3-BlogsTranslated/Blog4/architecture.png)

*Nguồn: AWS Architecture Blog - Building a scalable user search layer on top of Amazon Cognito*

Giải pháp được chia thành hai luồng chính: ingestion flow và search flow.

### Ingestion flow

Luồng ingestion chịu trách nhiệm lấy dữ liệu từ Cognito, lưu vào DynamoDB và cập nhật OpenSearch index.

1. Người dùng đăng ký hoặc đăng nhập thông qua Amazon Cognito.
2. Cognito Lambda triggers như **Post-confirmation** và **Pre-token generation** được gọi để tạo hoặc cập nhật bản ghi người dùng trong DynamoDB.
3. DynamoDB Streams phát hiện thay đổi trên user table.
4. Lambda xử lý stream và index dữ liệu vào Amazon OpenSearch Serverless.
5. Với các thay đổi do admin thực hiện, AWS CloudTrail ghi lại Cognito Admin API call.
6. Amazon EventBridge bắt event từ CloudTrail và gọi Lambda để đọc trạng thái mới nhất từ Cognito, sau đó cập nhật DynamoDB và OpenSearch.

Việc thêm CloudTrail và EventBridge là điểm quan trọng vì không phải mọi thay đổi người dùng đều đi qua authentication flow. Nếu admin tạo, sửa, disable user hoặc thay đổi group membership, kiến trúc vẫn có thể đồng bộ dữ liệu vào search index.

### Search flow

Luồng search tách biệt khỏi Cognito API mặc định.

1. Người dùng đã xác thực gửi truy vấn từ UI.
2. Amazon API Gateway nhận request và dùng Cognito authorizer để kiểm tra JWT token.
3. Search Lambda nhận tham số tìm kiếm.
4. Lambda assume role read-only để truy vấn OpenSearch Serverless.
5. Kết quả được format và trả về frontend theo dạng phân trang.

Cách này giúp search API có thể hỗ trợ fuzzy matching, complex filtering và sub-second response time mà không làm Cognito phải xử lý các truy vấn không thuộc nhiệm vụ chính của nó.

## Các dịch vụ AWS liên quan

| Dịch vụ | Vai trò trong giải pháp |
| --- | --- |
| Amazon Cognito | Quản lý người dùng, xác thực và phát hành JWT token |
| AWS Lambda | Xử lý Cognito triggers, CloudTrail events, DynamoDB Streams và search API |
| Amazon DynamoDB | Lưu user profile đã chuẩn hóa để làm nguồn dữ liệu trung gian |
| DynamoDB Streams | Phát hiện thay đổi dữ liệu và kích hoạt quá trình indexing |
| Amazon OpenSearch Serverless | Lưu search index và xử lý truy vấn nâng cao |
| AWS CloudTrail | Ghi lại các Cognito Admin API calls |
| Amazon EventBridge | Bắt event từ CloudTrail và kích hoạt Lambda đồng bộ |
| Amazon API Gateway | Cung cấp REST API cho frontend search |

## Điểm kỹ thuật đáng chú ý

### Tách command và query

Điểm mình thấy quan trọng nhất là kiến trúc này tách rõ hai trách nhiệm. Cognito xử lý command liên quan đến identity như đăng ký, đăng nhập, cập nhật user và group. OpenSearch xử lý query như fuzzy search, prefix search và filtering.

Cách tách này giống tư duy CQRS ở mức vừa đủ: không thay đổi hệ thống identity gốc, nhưng tạo một read model tối ưu cho truy vấn.

### Đồng bộ dữ liệu qua nhiều đường

Nếu chỉ dựa vào Post-confirmation trigger, hệ thống có thể bỏ sót các thay đổi admin. Nếu chỉ dựa vào batch sync, dữ liệu bị chậm. Bài viết kết hợp Cognito triggers, CloudTrail, EventBridge và DynamoDB Streams để giảm khoảng trống giữa trạng thái thật trong Cognito và dữ liệu trong OpenSearch.

### Lưu ý về Cognito Lambda triggers

Bài gốc nhắc rằng Cognito Lambda triggers có timeout giới hạn. Vì vậy, trigger function không nên xử lý quá nhiều logic nặng. Cách hợp lý là ghi dữ liệu cần thiết vào DynamoDB nhanh nhất có thể, sau đó để DynamoDB Streams và Lambda khác xử lý indexing bất đồng bộ.

## Điều mình học được

Trước khi đọc bài này, mình thường xem Cognito là nơi vừa quản lý người dùng vừa tìm kiếm người dùng. Sau khi tìm hiểu, mình thấy cách đúng hơn là để Cognito làm tốt nhiệm vụ identity, còn nhu cầu search nâng cao nên được đưa sang một hệ thống search chuyên dụng.

Mình cũng học được rằng đồng bộ dữ liệu người dùng không đơn giản như chỉ nghe một trigger. Trong thực tế, user có thể thay đổi qua nhiều đường: đăng ký, đăng nhập, admin console, CLI hoặc API. Một kiến trúc production cần tính đến tất cả các đường thay đổi đó.

## Kết luận

Bài viết này cho thấy cách mở rộng Amazon Cognito bằng một tầng tìm kiếm riêng, sử dụng DynamoDB làm dữ liệu trung gian và OpenSearch Serverless làm search index. Kiến trúc này giúp hỗ trợ fuzzy search, complex filtering, pagination và sub-second response time ở quy mô lớn.

Nếu áp dụng vào hệ thống thực tế, mình nghĩ các bước quan trọng là xác định rõ thuộc tính nào cần search, thiết kế data model trong DynamoDB, tối ưu OpenSearch index, xử lý đầy đủ các nguồn thay đổi từ Cognito, và giới hạn quyền IAM theo nguyên tắc least privilege cho từng Lambda.
