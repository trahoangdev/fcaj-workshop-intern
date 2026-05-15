---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 4:

* Củng cố kiến thức data protection với AWS Backup, bao gồm backup plan, SNS notifications và restore testing.
* Khám phá **Amazon DynamoDB** — dịch vụ NoSQL serverless và cách đưa bảng vào chiến lược backup.
* Thực hành migration và portability máy ảo với VM Import/Export qua VMware Workstation, Amazon S3 và EC2.
* Tìm hiểu hybrid file storage với AWS Storage Gateway (File Gateway) và Windows file share được quản lý bởi Amazon FSx.
* Nâng cao kỹ năng Amazon S3 về static website hosting, CloudFront, versioning và replication đa region.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Khám phá **Amazon DynamoDB**: <br>&emsp; + Tìm hiểu mô hình bảng, partition key và sort key <br>&emsp; + So sánh on-demand và provisioned capacity <br>&emsp; + Thao tác CRUD cơ bản trên console và quan sát metric <br> - Thực hành AWS Backup: <br>&emsp; + Tạo S3 bucket và triển khai hạ tầng lab <br>&emsp; + Tạo backup plan (gồm DynamoDB và các tài nguyên khác) <br>&emsp; + Cấu hình SNS notifications <br>&emsp; + Kiểm thử restore và cleanup tài nguyên | 11/05/2026 | 11/05/2026 | [AWS Backup](https://000013.awsstudygroup.com/) <br> [DynamoDB Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) |
| 3 | - Thực hành VM Import/Export: <br>&emsp; + Chuẩn bị môi trường VMware Workstation <br>&emsp; + Export máy ảo từ on-premises <br>&emsp; + Upload VM image lên Amazon S3 và import vào AWS <br>&emsp; + Triển khai EC2 instance từ AMI <br>&emsp; + Export instance/AMI về on-premises và cleanup | 12/05/2026 | 12/05/2026 | [VM Import/Export](https://000014.awsstudygroup.com/) |
| 4 | - Thực hành AWS Storage Gateway (File Gateway): <br>&emsp; + Chuẩn bị S3 bucket và EC2 cho Storage Gateway <br>&emsp; + Tạo Storage Gateway và file share <br>&emsp; + Mount file share lên máy on-premises <br>&emsp; + Cleanup tài nguyên | 13/05/2026 | 13/05/2026 | [AWS Storage Gateway](https://000024.awsstudygroup.com/) |
| 5 | - Thực hành Amazon FSx for Windows File Server: <br>&emsp; + Tạo environment và file system Multi-AZ (SSD/HDD) <br>&emsp; + Tạo file share và kiểm thử hiệu năng <br>&emsp; + Bật deduplication, shadow copies và user quotas <br>&emsp; + Scale throughput/storage và xóa environment | 14/05/2026 | 14/05/2026 | [Amazon FSx](https://000025.awsstudygroup.com/) |
| 6 | - Thực hành Amazon S3 cơ bản và static website hosting: <br>&emsp; + Tạo bucket, bật static website và cấu hình public access <br>&emsp; + Tăng tốc website bằng CloudFront và bật versioning <br>&emsp; + Move object và cấu hình replication đa region <br>&emsp; + Xem notes, best practices và cleanup | 15/05/2026 | 15/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |


### Kết quả đạt được tuần 4:

* Khám phá **Amazon DynamoDB** — hiểu đặc điểm NoSQL serverless, mô hình bảng (table, partition key, sort key) và các chế độ capacity (on-demand vs provisioned).

* Thực hành thao tác cơ bản trên DynamoDB Console: tạo bảng, thêm/sửa/xóa item và theo dõi metric đọc/ghi để làm quen vận hành dịch vụ.

* Hiểu vai trò của **AWS Backup** trong việc tập trung hóa và tự động hóa bảo vệ dữ liệu cho các tài nguyên như EBS, RDS, **DynamoDB** và EFS.

* Đã triển khai hạ tầng lab, tạo **backup plan**, gán resource (bao gồm bảng DynamoDB) vào plan và thiết lập lịch backup theo chính sách định sẵn.

* Cấu hình **Amazon SNS** để nhận thông báo khi quá trình backup hoặc restore hoàn tất, giúp theo dõi trạng thái job tập trung.

* Thực hiện **restore testing** từ bản backup, xác minh dữ liệu có thể phục hồi đúng kỳ vọng trước khi áp dụng trong môi trường thực tế.

* Hoàn tất **cleanup** tài nguyên lab sau khi thực hành để tránh phát sinh chi phí không cần thiết.

* Nắm được khái niệm **VM Import/Export** và các tình huống sử dụng: migration workload lên cloud, backup VM on-premises và disaster recovery.

* Chuẩn bị môi trường **VMware Workstation**, export máy ảo từ on-premises và upload image lên **Amazon S3** làm bước trung gian cho import.

* Import VM image vào AWS, tạo **AMI** và triển khai **EC2 instance** từ AMI đã import để kiểm chứng workload chạy được trên cloud.

* Thực hành chiều ngược lại: cấu hình **S3 bucket ACL**, export instance/AMI từ AWS về on-premises và dọn dẹp tài nguyên sau lab.

* Hiểu cách **AWS Storage Gateway (File Gateway)** kết nối lưu trữ file on-premises với object storage trên S3 theo mô hình hybrid.

* Đã tạo **S3 bucket**, EC2 làm gateway host, khởi tạo Storage Gateway và **file share** theo hướng dẫn workshop.

* **Mount file share** lên máy on-premises, kiểm tra đọc/ghi file qua giao thức SMB/NFS tùy cấu hình lab và xác nhận dữ liệu đồng bộ về S3.

* Hiểu kiến trúc **Amazon FSx for Windows File Server**, bao gồm file server, lưu trữ phía sau, VPC, ENI và cơ chế replication đa AZ.

* Triển khai file system **Multi-AZ** (SSD và HDD), tạo **file share** mới và kiểm thử hiệu năng truy cập từ môi trường Windows.

* Bật và quan sát các tính năng quản trị: **data deduplication**, **shadow copies**, quản lý phiên người dùng/file đang mở, **storage quotas** và **Continuous Access** share.

* Thực hành **scale throughput** và **scale storage capacity**, theo dõi metric hiệu năng trước khi xóa environment và cleanup.

* Nắm vững các khái niệm cốt lõi của **Amazon S3**: bucket, object, durability, use case (website, backup, data lake, analytics).

* Triển khai **static website hosting** trên S3, cấu hình **Block Public Access**, public object và kiểm tra truy cập website.

* Tăng tốc phân phối nội dung bằng **Amazon CloudFront**, đồng thời siết quyền truy cập public trực tiếp vào bucket theo mô hình an toàn hơn.

* Bật **bucket versioning**, thực hiện **move object** giữa các prefix/bucket và cấu hình **cross-region replication (CRR)**.

* Tổng hợp **notes & best practices** về chi phí, bảo mật và vận hành S3; hoàn tất cleanup toàn bộ bucket, distribution và tài nguyên liên quan.
