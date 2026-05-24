---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 5:

* Xây nền **resource governance** với AWS Tags và Resource Groups để tổ chức, truy vấn và tự động hoá thao tác trên nhiều tài nguyên.
* Củng cố **IAM least privilege** với tag-based access control cho EC2 và Permission Boundary để giới hạn quyền tối đa của user.
* Thực hành **mã hoá dữ liệu lúc nghỉ (encryption at rest)** cho Amazon S3 bằng AWS KMS, kết hợp AWS CloudTrail và Amazon Athena để audit việc sử dụng key.
* Làm quen với **AWS Security Hub** nhằm tập trung hoá security findings và liên tục đánh giá tuân thủ theo best practices của AWS.
* Tối ưu **vận hành và chi phí** bằng Lambda tự động start/stop EC2 và **Grafana** monitoring dashboard trên EC2.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Thực hành **Tags & Resource Groups**: <br>&emsp; + Gắn tag cho EC2/S3 và các tài nguyên khác <br>&emsp; + Tạo Resource Group theo tag query <br>&emsp; + Dùng group để quản lý và tự động hoá thao tác <br>&emsp; + Cleanup tài nguyên | 18/05/2026 | 18/05/2026 | [Tags & Resource Groups](https://000027.awsstudygroup.com/) |
| 3 | - Thực hành **IAM tag-based access control cho EC2**: <br>&emsp; + Tạo IAM policy với `Condition` theo resource tag <br>&emsp; + Tạo IAM role cho persona EC2 Administrator <br>&emsp; + Kiểm thử hành vi least-privilege <br> - Thực hành **IAM Permission Boundary**: <br>&emsp; + Tạo restriction policy làm boundary <br>&emsp; + Tạo IAM user gắn boundary <br>&emsp; + Xác minh effective permissions và cleanup | 19/05/2026 | 19/05/2026 | [IAM with Resource Tags](https://000028.awsstudygroup.com/) <br> [IAM Permission Boundary](https://000030.awsstudygroup.com/) |
| 4 | - Thực hành **Encrypt at rest với AWS KMS**: <br>&emsp; + Chuẩn bị IAM user/role cho lab <br>&emsp; + Tạo KMS Customer Managed Key (CMK) <br>&emsp; + Tạo S3 bucket và bật SSE-KMS <br>&emsp; + Cấu hình CloudTrail và truy vấn event KMS bằng Athena <br>&emsp; + Test chia sẻ object đã mã hoá và cleanup | 20/05/2026 | 20/05/2026 | [Encrypt at Rest với AWS KMS](https://000033.awsstudygroup.com/) |
| 5 | - Thực hành **AWS Security Hub**: <br>&emsp; + Xem các security standard được hỗ trợ <br>&emsp; + Bật Security Hub và các integration <br>&emsp; + Đọc security score và findings theo từng standard <br>&emsp; + Cleanup tài nguyên | 21/05/2026 | 21/05/2026 | [AWS Security Hub](https://000018.awsstudygroup.com/) |
| 6 | - Thực hành **Tối ưu chi phí EC2 với Lambda**: <br>&emsp; + Gắn tag instance theo lịch start/stop <br>&emsp; + Tạo IAM role cho Lambda <br>&emsp; + Viết Lambda function và trigger qua EventBridge <br>&emsp; + Kiểm thử start/stop và cleanup <br> - Thực hành **Grafana cơ bản trên EC2**: <br>&emsp; + Triển khai EC2 instance Linux <br>&emsp; + Cài đặt và cấu hình Grafana <br>&emsp; + Thêm data source và xây dashboard giám sát tài nguyên <br>&emsp; + Cleanup | 22/05/2026 | 22/05/2026 | [Optimize EC2 Cost với Lambda](https://000022.awsstudygroup.com/) <br> [Grafana Basic](https://000029.awsstudygroup.com/) |


### Kết quả đạt được tuần 5:

* Hiểu vai trò của **Tag** như metadata để tổ chức tài nguyên AWS theo mục đích, owner, environment hoặc cost center, và đã áp dụng convention tag thống nhất trên EC2 và S3.

* Tạo **Resource Group** dựa trên tag query để quản lý nhiều tài nguyên như một đơn vị logic duy nhất, đồng thời hiểu cách Resource Groups hỗ trợ automation xuyên service.

* Áp dụng **nguyên tắc least privilege** trong IAM bằng policy có `Condition` cấp quyền EC2 chỉ khi resource mang tag phù hợp, và xác minh hành vi qua role EC2 Administrator.

* Cấu hình **IAM Permission Boundary** để giới hạn quyền tối đa của user, quan sát được effective permissions là phần giao giữa identity policy và boundary, và áp dụng pattern này để giảm rủi ro privilege escalation.

* Tạo **AWS KMS Customer Managed Key (CMK)** với key policy phù hợp và dùng key này để bật **SSE-KMS** cho S3 bucket nhằm bảo vệ dữ liệu lúc nghỉ.

* Bật **AWS CloudTrail** để ghi lại event của KMS và S3, sau đó dùng **Amazon Athena** truy vấn log để audit ai đã dùng key, khi nào và trên object nào.

* Thực hành **chia sẻ object S3 đã mã hoá** giữa các IAM principal trong khi vẫn kiểm soát quyền truy cập key qua key policy/grant, và cleanup toàn bộ KMS, S3, CloudTrail, Athena artifact sau lab.

* Bật **AWS Security Hub** và xem qua các standard được hỗ trợ (ví dụ AWS Foundational Security Best Practices, CIS) để có cái nhìn tập trung về findings ưu tiên cao xuyên các service.

* Đọc **security score theo từng standard**, drill-down vào các control fail, và hiểu cách Security Hub gom findings từ GuardDuty, Inspector, Macie về một dashboard duy nhất.

* Xây dựng workflow **tối ưu chi phí EC2** bằng **AWS Lambda**: gắn tag schedule cho instance, tạo execution role với quyền `ec2:StartInstances`/`ec2:StopInstances`, viết hàm Lambda thao tác trên các instance được tag.

* Xác minh logic start/stop qua trigger thủ công và lịch tự động, đảm bảo chỉ instance được tag bị tác động, sau đó cleanup function, role và policy sau lab.

* Triển khai **EC2 instance** Linux làm Grafana host, cài đặt và cấu hình **Grafana** server, mở dashboard qua security group đúng quy ước.

* Kết nối Grafana với data source metric, xây **dashboard giám sát** cơ bản cho compute và hạ tầng, sau đó xoá EC2 và các tài nguyên hỗ trợ khi kết thúc lab.

* Củng cố thói quen **cleanup** sau mỗi workshop để giữ chi phí AWS dự đoán được và tài khoản thực hành gọn gàng.
