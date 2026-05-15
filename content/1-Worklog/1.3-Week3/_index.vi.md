---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 3:

* Thực hành data protection với AWS Backup, bao gồm backup plan, notification và restore testing.
* Tìm hiểu hybrid file storage với AWS Storage Gateway (File Gateway).
* Nắm các kiến thức cơ bản về Amazon S3 và static website hosting, cùng các tính năng như versioning và replication.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Thực hành AWS Backup: <br>&emsp; + Triển khai hạ tầng lab <br>&emsp; + Tạo backup plan <br>&emsp; + Cấu hình SNS notifications <br>&emsp; + Kiểm thử restore và cleanup | 04/05/2026 | 04/05/2026 | [AWS Backup](https://000013.awsstudygroup.com/) |
| 3 | - Thực hành AWS Storage Gateway (File Gateway): <br>&emsp; + Chuẩn bị môi trường <br>&emsp; + Tạo Storage Gateway và file share <br>&emsp; + Mount file share lên máy on-premises <br>&emsp; + Cleanup tài nguyên | 05/05/2026 | 05/05/2026 | [AWS Storage Gateway](https://000024.awsstudygroup.com/) |
| 4 | - Thực hành Amazon S3 cơ bản và static website hosting: <br>&emsp; + Tạo bucket và bật static website <br>&emsp; + Cấu hình public access block và public object <br>&emsp; + Test website | 06/05/2026 | 06/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |
| 5 | - Tiếp tục với Amazon S3: <br>&emsp; + Tăng tốc static website bằng CloudFront <br>&emsp; + Bật bucket versioning <br>&emsp; + Move object | 07/05/2026 | 07/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |
| 6 | - Hoàn thiện lab Amazon S3: <br>&emsp; + Cấu hình replication đa region <br>&emsp; + Xem notes và best practices <br>&emsp; + Cleanup tài nguyên | 08/05/2026 | 08/05/2026 | [Amazon S3](https://000057.awsstudygroup.com/) |


### Kết quả đạt được tuần 3:

* Hiểu vai trò của **AWS Backup** trong việc tập trung hóa và tự động hóa bảo vệ dữ liệu cho các tài nguyên như EBS, RDS, DynamoDB và EFS.

* Triển khai hạ tầng lab, tạo **backup plan**, gán resource vào plan và thiết lập lịch backup theo chính sách workshop.

* Cấu hình **Amazon SNS** để nhận thông báo khi job backup hoặc restore hoàn tất, giúp theo dõi trạng thái tập trung.

* Thực hiện **restore testing** từ bản backup và xác minh dữ liệu phục hồi đúng kỳ vọng.

* Hoàn tất **cleanup** tài nguyên lab AWS Backup để tránh phát sinh chi phí.

* Hiểu mô hình **hybrid storage** với **AWS Storage Gateway (File Gateway)** — kết nối lưu trữ file on-premises với object storage trên S3.

* Chuẩn bị môi trường lab (S3 bucket, EC2 gateway host), khởi tạo **Storage Gateway** và tạo **file share**.

* **Mount file share** lên máy on-premises, kiểm tra thao tác đọc/ghi file và xác nhận dữ liệu được đồng bộ về S3.

* Dọn dẹp đầy đủ tài nguyên Storage Gateway sau khi hoàn thành lab.

* Nắm các khái niệm cốt lõi của **Amazon S3**: bucket, object, durability (11 nines) và các use case phổ biến (website, backup, data lake).

* Tạo **S3 bucket**, bật **static website hosting**, cấu hình **Block Public Access** và public object theo yêu cầu lab.

* Kiểm tra truy cập website qua endpoint S3 và ghi nhận luồng phục vụ nội dung tĩnh từ object storage.

* Triển khai **Amazon CloudFront** để tăng tốc phân phối static website, đồng thời hạn chế truy cập public trực tiếp vào bucket.

* Bật **bucket versioning** để bảo vệ object khỏi ghi đè hoặc xóa nhầm; thực hành **move object** giữa prefix/bucket.

* Cấu hình **cross-region replication (CRR)** để sao chép object sang region khác, phục vụ DR và tuân thủ dữ liệu.

* Tổng hợp **notes & best practices** về chi phí, bảo mật và vận hành S3; hoàn tất cleanup bucket, distribution và tài nguyên liên quan.
