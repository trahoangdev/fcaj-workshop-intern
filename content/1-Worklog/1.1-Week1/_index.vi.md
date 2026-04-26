---
title: "Worklog Tuần 1"
date: 2026-04-20
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 1:

* Hoàn thành 5 workshop nền tảng của AWS Study Group trong giai đoạn khởi động.
* Xây nền kiến thức AWS theo 5 mảng: account, IAM, networking, compute và database.
* Thực hành đầy đủ quy trình triển khai và cleanup để tránh phát sinh chi phí không cần thiết.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Hoàn thành workshop **AWS Free Tier 2025** <br> - Tìm hiểu các nội dung trọng tâm: <br>&emsp;+ Free Plan vs Paid Plan <br>&emsp;+ Chiến lược nhận đủ $200 credit <br>&emsp;+ Các dịch vụ dễ làm hao hụt credit <br> - Tạo checklist quản lý chi phí cá nhân cho tuần đầu | 20/04/2026 | 20/04/2026 | [Workshop](https://000001.awsstudygroup.com/) <br> [AWS Free Tier Docs](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier.html) |
| 3 | - Hoàn thành workshop **IAM Access Control** <br> - **Thực hành:** <br>&emsp;+ Tạo IAM Group và IAM User <br>&emsp;+ Gán policy và kiểm tra quyền truy cập <br>&emsp;+ Tạo IAM Role và thực hiện switch role <br> - Tổng hợp ghi chú về nguyên tắc least privilege | 21/04/2026 | 21/04/2026 | [Workshop](https://000002.awsstudygroup.com/) <br> [IAM Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) |
| 4 | - Hoàn thành workshop **Amazon VPC and Site-to-Site VPN** <br> - **Thực hành:** <br>&emsp;+ Tạo VPC và các subnet cơ bản <br>&emsp;+ Cấu hình Security Group và NACL nền tảng <br>&emsp;+ Thực hiện luồng thiết lập Site-to-Site VPN <br> - Kiểm tra kết nối và dọn dẹp tài nguyên theo hướng dẫn | 22/04/2026 | 22/04/2026 | [Workshop](https://000003.awsstudygroup.com/) <br> [VPC Docs](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), [Site-to-Site VPN Docs](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html) |
| 5 | - Hoàn thành workshop **Introduction to Amazon EC2** <br> - **Thực hành:** <br>&emsp;+ Khởi tạo EC2 Windows Server và Amazon Linux <br>&emsp;+ Kết nối instance và thao tác vận hành cơ bản <br>&emsp;+ Deploy ứng dụng Node.js mẫu trên EC2 <br> - Ghi lại các lưu ý bảo mật và chi phí khi dùng EC2 | 23/04/2026 | 23/04/2026 | [Workshop](https://000004.awsstudygroup.com/) <br> [EC2 Docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) |
| 6 | - Hoàn thành workshop **Amazon RDS** <br> - **Thực hành:** <br>&emsp;+ Tạo DB instance trên Amazon RDS <br>&emsp;+ Kết nối ứng dụng với cơ sở dữ liệu <br>&emsp;+ Kiểm thử quy trình backup và restore <br> - Cleanup toàn bộ tài nguyên và rà soát chi phí phát sinh | 24/04/2026 | 24/04/2026 | [Workshop](https://000005.awsstudygroup.com/) <br> [RDS Docs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |


### Kết quả đạt được tuần 1:

* Hoàn thành đầy đủ 5 workshop nền tảng đã đặt ra trong tuần 1.

* Củng cố kiến thức và thực hành trên các mảng AWS quan trọng:
  * Quản lý tài khoản và chi phí (Free Tier 2025)
  * Quản trị định danh và phân quyền (IAM user, group, role)
  * Mạng và kết nối hybrid (VPC + Site-to-Site VPN)
  * Tài nguyên tính toán và triển khai ứng dụng (EC2 Windows/Linux)
  * Dịch vụ cơ sở dữ liệu quan hệ managed (Amazon RDS)

* Thực hành quy trình lab theo hướng end-to-end từ chuẩn bị môi trường, triển khai, kiểm tra đến cleanup.

* Nâng cao tư duy bảo mật với least privilege, role-based access và network segmentation.

* Cải thiện khả năng đọc tài liệu kỹ thuật và chuyển hóa thành các bước triển khai thực tế.

### Khó khăn gặp phải và cách xử lý:

* Khó khăn: Lượng kiến thức AWS mới trong tuần đầu khá lớn, dễ nhầm lẫn giữa các dịch vụ.
  Cách xử lý: Ghi chú theo từng nhóm (Account, IAM, Network, Compute, Database) và tổng hợp lại sau mỗi workshop.

* Khó khăn: Phân quyền IAM và cơ chế assume role ban đầu khó hình dung.
  Cách xử lý: Làm lại bài IAM nhiều lần, so sánh khác biệt giữa user, group và role để hiểu luồng cấp quyền.

* Khó khăn: Nguy cơ phát sinh chi phí ngoài ý muốn khi thực hành cloud.
  Cách xử lý: Luôn chạy bước cleanup sau mỗi lab và kiểm tra lại tài nguyên còn tồn tại trên AWS Console.

### Bài học rút ra:

* Làm cloud hiệu quả phải đi từ nền tảng chi phí và bảo mật.
* Thiết kế IAM và nguyên tắc least privilege cần áp dụng ngay từ đầu.
* Thực hành trọn vòng đời (tạo, kiểm thử, dọn dẹp) giúp hình thành thói quen vận hành tốt.

### Kế hoạch tuần 2:

* Tiếp tục thực hành các dịch vụ AWS cốt lõi ở mức chuyên sâu hơn.
* Bổ sung ảnh minh chứng và lệnh quan trọng trong từng bài để báo cáo rõ ràng hơn.
* Tăng tốc xử lý lỗi khi triển khai và cấu hình dịch vụ.


