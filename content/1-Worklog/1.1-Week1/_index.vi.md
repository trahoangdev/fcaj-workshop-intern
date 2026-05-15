---
title: "Worklog Tuần 1"
date: 2024-01-01
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

* Hoàn thành đầy đủ **5 workshop nền tảng** AWS Study Group trong tuần khởi động thực tập.

* Hiểu cấu trúc **AWS Free Tier 2025**, phân biệt Free Plan và Paid Plan, và các điều kiện để nhận đủ **$200 credit**.

* Nắm các dịch vụ dễ làm **hao hụt credit** (EC2, NAT Gateway, Load Balancer, v.v.) và lập **checklist quản lý chi phí** cá nhân cho tuần đầu.

* Hiểu mô hình **IAM**: user, group, role, policy và nguyên tắc **least privilege** trong phân quyền truy cập AWS.

* Đã tạo **IAM Group**, **IAM User**, gán policy và **kiểm tra quyền** truy cập trên console.

* Tạo **IAM Role**, thực hiện **switch role** và ghi nhận khác biệt giữa quyền gán trực tiếp cho user và quyền thông qua role.

* Hiểu khái niệm **Amazon VPC**: CIDR, subnet public/private, route table và cách cô lập tài nguyên trong mạng ảo.

* Cấu hình **Security Group** và **NACL** ở mức nền tảng để kiểm soát traffic vào/ra theo từng lớp bảo mật.

* Thực hiện luồng thiết lập **Site-to-Site VPN** giữa môi trường mô phỏng on-premises và VPC trên AWS; **kiểm tra kết nối** và cleanup theo hướng dẫn lab.

* Nắm các khái niệm **EC2**: instance type, AMI, key pair, security group và cách kết nối instance.

* Khởi tạo **EC2 Windows Server** và **Amazon Linux**, thực hiện kết nối và các thao tác vận hành cơ bản trên từng loại OS.

* **Deploy ứng dụng Node.js mẫu** trên EC2, ghi nhận các lưu ý về bảo mật (mở port, SSH/RDP) và chi phí khi instance chạy liên tục.

* Hiểu **Amazon RDS** là dịch vụ database quan hệ được quản lý, bao gồm provisioning, backup tự động và high availability cơ bản.

* Tạo **RDS DB instance**, **kết nối ứng dụng** với database và **kiểm thử backup/restore** theo quy trình workshop.

* Thực hành quy trình lab **end-to-end**: chuẩn bị môi trường → triển khai → kiểm tra → **cleanup** và rà soát chi phí trên Billing Console sau mỗi ngày lab.

* Hình thành thói quen đọc tài liệu workshop/AWS Docs và chuyển thành các bước triển khai có thể kiểm chứng trên console.

