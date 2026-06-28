---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Đi sâu vào **AWS IAM nâng cao** — role, condition, Permission Boundary, Identity Center SSO — để áp dụng nguyên tắc least-privilege ở quy mô doanh nghiệp.
* Tìm hiểu và thực hành **bảo mật cấp mạng** với AWS WAF và VPC Endpoints (PrivateLink) nhằm bảo vệ workload khỏi internet công cộng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Thực hành **IAM Role & Condition**: <br>&emsp; + Ôn lại các khái niệm IAM (User, Group, Role, Policy) <br>&emsp; + Tạo IAM Group và IAM User cho admin EC2 và RDS <br>&emsp; + Tạo IAM Role với `Condition` (giới hạn theo địa chỉ IP và thời gian) <br>&emsp; + Kiểm tra hành vi assume-role và dọn dẹp tài nguyên | 01/06/2026 | 01/06/2026 | [IAM Role & Condition](https://000044.awsstudygroup.com/) |
| 3 | - Thực hành **IAM Permission Boundary**: <br>&emsp; + Hiểu sự khác biệt giữa Permission Boundary và Identity-based Policy <br>&emsp; + Tạo restriction policy và áp dụng làm Permission Boundary <br>&emsp; + Tạo IAM User bị giới hạn và kiểm tra effective permissions <br>&emsp; + Quan sát cơ chế ngăn chặn leo thang đặc quyền (privilege escalation) <br>&emsp; + Dọn dẹp tài nguyên | 02/06/2026 | 02/06/2026 | [IAM Permission Boundary](https://000030.awsstudygroup.com/) |
| 4 | - Thực hành **AWS IAM Identity Center (SSO)**: <br>&emsp; + Bật IAM Identity Center và thiết lập Identity Store <br>&emsp; + Tạo group và user trong Identity Store <br>&emsp; + Gán permission set cho group trên nhiều AWS account <br>&emsp; + Cấu hình AWS CLI truy cập qua Identity Center <br>&emsp; + Áp dụng time-based access control và Customer Managed Policies <br>&emsp; + Dùng Identity Store APIs quản lý danh tính theo chương trình <br>&emsp; + Dọn dẹp tài nguyên | 03/06/2026 | 03/06/2026 | [IAM Identity Center](https://000012.awsstudygroup.com/) |
| 5 | - Thực hành **AWS Web Application Firewall (WAF)**: <br>&emsp; + Ôn lại các khái niệm WAF: WebACL, Rule, Rule Group, IP Set <br>&emsp; + Chuẩn bị tài nguyên đích (ALB / CloudFront) <br>&emsp; + Tạo và gắn WebACL với managed rule và custom rule <br>&emsp; + Kiểm tra hành vi block/allow của WAF <br>&emsp; + Dọn dẹp tài nguyên | 04/06/2026 | 04/06/2026 | [AWS WAF](https://000026.awsstudygroup.com/) |
| 6 | - Thực hành **Truy cập S3 an toàn qua VPC Endpoints**: <br>&emsp; + Phân biệt Gateway Endpoint và Interface Endpoint (PrivateLink) <br>&emsp; + Tạo Gateway VPC Endpoint cho S3 và cập nhật route table <br>&emsp; + Tạo Interface VPC Endpoint cho S3 (PrivateLink) <br>&emsp; + Truy cập S3 từ môi trường on-premises qua Interface Endpoint <br>&emsp; + Cấu hình VPC Endpoint Policies để kiểm soát quyền truy cập (Bonus) <br>&emsp; + Dọn dẹp tài nguyên | 05/06/2026 | 05/06/2026 | [VPC Endpoints cho S3](https://000111.awsstudygroup.com/) |
| 7 | - Thực hành **AWS Security Hub**: <br>&emsp; + Xem xét các security standard (AWS FSBP, CIS, PCI DSS) <br>&emsp; + Bật Security Hub và các tích hợp liên kết <br>&emsp; + Phân tích security score và findings theo từng standard <br>&emsp; + Hiểu cơ chế tổng hợp từ GuardDuty, Inspector, Macie <br>&emsp; + Dọn dẹp tài nguyên | 06/06/2026 | 06/06/2026 | [AWS Security Hub](https://000018.awsstudygroup.com/) |

### Kết quả đạt được tuần 7:

* Củng cố **mô hình tin cậy IAM** bằng cách phân biệt rõ Identity-based Policy, Resource-based Policy và Trust Policy của role; áp dụng `Condition` block (giới hạn IP CIDR và khoảng thời gian `DateGreaterThan`/`DateLessThan`) để kiểm soát điều kiện assume-role.

* Tạo IAM Group và IAM User cho từng persona (EC2 admin, RDS admin), xác minh mỗi người chỉ thao tác được trong phạm vi dịch vụ được phân công, và thực hành toàn bộ luồng assume-role cùng với quá trình đánh giá condition.

* Hiểu và áp dụng **IAM Permission Boundary** như một lớp kiểm soát trần — giới hạn quyền tối đa cho user/role bất kể identity policy được gán là gì — đây là biện pháp chủ chốt để ngăn leo thang đặc quyền trong môi trường nhiều nhóm.

* Quan sát công thức giao quyền: *effective permissions = Identity Policy ∩ Permission Boundary*, và vận dụng mô hình này để ủy quyền quản lý IAM cho developer mà không lo rủi ro cấp quyền thừa.

* Thiết lập **AWS IAM Identity Center** (trước là AWS SSO) để cấp quyền truy cập liên kết tập trung vào nhiều AWS account từ một nguồn định danh duy nhất, loại bỏ việc phải tạo IAM user riêng trên từng account.

* Cấu hình permission set ánh xạ với group trong Identity Store, gán cho AWS account và truy cập qua AWS CLI bằng thông tin xác thực Identity Center — thực hành nguyên tắc Zero Trust ở cấp độ tổ chức.

* Áp dụng **time-based access control** trong Identity Center và quản lý danh tính theo chương trình qua **Identity Store APIs**, hiểu cách các khả năng này hỗ trợ luồng onboarding/offboarding tự động.

* Học các thành phần cốt lõi của **AWS WAF** — WebACL, Rule, Rule Group, IP Set, Managed Rule Group — và hiểu cách WAF tích hợp với CloudFront, ALB, API Gateway, AppSync như một lớp phòng thủ tầng 7 (Layer 7).

* Tạo WebACL với AWS Managed Rules và custom rate-limiting rule, gắn vào tài nguyên đích, xác nhận hành vi block/allow bằng cách gửi request kiểm thử.

* Phân biệt rõ hai loại **VPC Endpoint**: Gateway Endpoint (dựa trên route table, dành cho S3 và DynamoDB, không phát sinh chi phí thêm) và Interface Endpoint (dựa trên ENI, phân giải qua DNS, dành cho dịch vụ PrivateLink).

* Tạo **Gateway VPC Endpoint** cho S3, cập nhật route table liên kết và xác nhận traffic từ EC2 instance không còn đi qua internet công cộng khi truy cập S3.

* Tạo **Interface VPC Endpoint** (PrivateLink) cho S3 và cấu hình để chấp nhận kết nối từ môi trường on-premises, hiểu cách private DNS và security group kiểm soát quyền truy cập.

* Cấu hình **VPC Endpoint Policies** để kiểm soát quyền truy cập chi tiết ở cấp resource — lớp phòng thủ theo chiều sâu (defense-in-depth) giới hạn bucket S3 nào có thể được truy cập qua endpoint.

* Bật **AWS Security Hub** và xem dashboard tổng hợp findings, hiểu cách Security Hub kéo findings từ GuardDuty, Inspector, Macie và các nguồn khác vào một giao diện được ưu tiên theo các standard như AWS FSBP và CIS Benchmarks.

* Duy trì **kỷ luật clean-up** sau mỗi lab để tránh phát sinh chi phí không mong muốn và giữ tài khoản thực hành gọn gàng.
