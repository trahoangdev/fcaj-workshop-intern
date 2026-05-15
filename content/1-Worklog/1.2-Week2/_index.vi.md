---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 2:

* Tăng cường thực hành với các dịch vụ AWS về networking, identity, backup, migration, container, security và CI/CD.
* Hiểu cách cấu hình tài nguyên AWS thông qua AWS Management Console và AWS CLI.
* Thực hành triển khai ứng dụng bằng Docker, Amazon ECS và các quy trình CI/CD tự động.
* Tìm hiểu cách thiết kế kết nối mạng an toàn, có khả năng mở rộng bằng VPC Peering và AWS Transit Gateway.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | - Tìm hiểu kiến trúc hybrid DNS với Amazon Route 53 Resolver: <br>&emsp; + Xem lại inbound endpoint, outbound endpoint và resolver rules <br>&emsp; + Triển khai hạ tầng lab bằng CloudFormation <br>&emsp; + Cấu hình Microsoft AD và DNS forwarding giữa mô phỏng on-premises và AWS <br>&emsp; + Kiểm tra kết quả phân giải DNS <br> - Thực hành AWS CLI: <br>&emsp; + Cài đặt và cấu hình AWS CLI profile <br>&emsp; + Xem tài nguyên EC2, S3, IAM, VPC và SNS bằng CLI <br>&emsp; + Tạo tài nguyên EC2 bằng các lệnh CLI <br> - Thực hành IAM Identity Center: <br>&emsp; + Tạo AWS accounts trong AWS Organizations <br>&emsp; + Thiết lập organizational units, users, groups và permission sets <br>&emsp; + Kiểm tra quyền truy cập console và CLI theo permission đã gán | 27/04/2026 | 27/04/2026 | [Route 53 Resolver](https://000010.awsstudygroup.com/) <br> [AWS CLI](https://000011.awsstudygroup.com/) <br> [IAM Identity Center](https://000012.awsstudygroup.com/) |
| 3 | - Thực hành AWS Backup: <br>&emsp; + Tạo S3 bucket và triển khai tài nguyên lab <br>&emsp; + Tạo backup plan cho tài nguyên AWS <br>&emsp; + Cấu hình backup notifications <br>&emsp; + Kiểm thử restore từ bản backup và dọn dẹp tài nguyên <br> - Thực hành VM Import/Export: <br>&emsp; + Chuẩn bị môi trường VMware Workstation <br>&emsp; + Export máy ảo từ mô phỏng on-premises <br>&emsp; + Upload VM image lên Amazon S3 <br>&emsp; + Import VM vào AWS và triển khai EC2 instance từ AMI đã tạo | 28/04/2026 | 28/04/2026 | [AWS Backup](https://000013.awsstudygroup.com/) <br> [VM Import/Export](https://000014.awsstudygroup.com/) |
| 4 | - Thực hành triển khai ứng dụng bằng Docker: <br>&emsp; + Cài dependencies và chạy ứng dụng ở local <br>&emsp; + Cấu hình VPC, security group và IAM role để truy cập ECR <br>&emsp; + Khởi tạo và cấu hình Amazon RDS <br>&emsp; + Cấu hình EC2 instance để triển khai container <br>&emsp; + Deploy ứng dụng bằng Docker image và Docker Compose <br>&emsp; + Push container image lên Amazon ECR hoặc Docker Hub | 29/04/2026 | 29/04/2026 | [Docker Deployment](https://000015.awsstudygroup.com/) |
| 5 | - Thực hành triển khai ứng dụng trên Amazon ECS: <br>&emsp; + Tạo ECS cluster với AWS Fargate <br>&emsp; + Tạo backend và frontend task definitions <br>&emsp; + Cấu hình target groups và Application Load Balancer <br>&emsp; + Tạo backend ECS service với blue/green deployment <br>&emsp; + Tạo frontend ECS service với rolling deployment <br>&emsp; + Kiểm tra truy cập ứng dụng và trạng thái service | 30/04/2026 | 30/04/2026 | [Amazon ECS](https://000016.awsstudygroup.com/) |
| 6 | - Thực hành CI/CD deployment cho ECS: <br>&emsp; + Cấu hình GitLab pipeline và GitLab Runner <br>&emsp; + Cấu hình GitHub Actions workflow để deploy lên ECS <br>&emsp; + Tạo CodeBuild project cho frontend và backend <br>&emsp; + Kiểm tra kết quả deployment và theo dõi application logs <br> - Thực hành AWS Security Hub: <br>&emsp; + Enable Security Hub <br>&emsp; + Xem security standards và security score <br>&emsp; + Kiểm tra findings và cleanup tài nguyên <br> - Thực hành AWS networking: <br>&emsp; + Tạo VPC Peering connection và cập nhật route tables <br>&emsp; + Cấu hình cross-peer DNS resolution <br>&emsp; + Tạo Transit Gateway, attachments và route tables <br>&emsp; + Thêm Transit Gateway routes vào VPC route tables và test kết nối | 01/05/2026 | 01/05/2026 | [ECS CI/CD](https://000017.awsstudygroup.com/) <br> [Security Hub](https://000018.awsstudygroup.com/) <br> [VPC Peering](https://000019.awsstudygroup.com/) <br> [Transit Gateway](https://000020.awsstudygroup.com/) |


### Kết quả đạt được tuần 2:

* Hiểu kiến trúc **hybrid DNS** với **Amazon Route 53 Resolver**: inbound endpoint, outbound endpoint và resolver rules.

* Triển khai hạ tầng lab bằng **CloudFormation**, cấu hình **Microsoft AD** và DNS forwarding giữa mô phỏng on-premises và AWS.

* Kiểm tra **kết quả phân giải DNS** hai chiều và ghi nhận luồng query giữa môi trường on-premises và private hosted zone trên AWS.

* Cài đặt **AWS CLI**, cấu hình profile (access key, region, output format) và thực hành các lệnh cơ bản.

* Dùng CLI để **liệt kê và thao tác** tài nguyên EC2, S3, IAM, VPC và SNS; tạo tài nguyên EC2 bằng dòng lệnh.

* Hiểu **IAM Identity Center** (SSO) trong bối cảnh **AWS Organizations**: quản lý nhiều account từ một điểm đăng nhập.

* Tạo **AWS accounts**, **organizational units (OU)**, users, groups và **permission sets**; gán quyền truy cập theo vai trò.

* Kiểm tra đăng nhập **console** và **CLI** qua Identity Center với permission set đã cấu hình.

* Hiểu vai trò **AWS Backup** trong bảo vệ tập trung cho EBS, RDS, DynamoDB, EFS; tạo **backup plan** và gán resource.

* Cấu hình **thông báo backup** (SNS), thực hiện **restore testing** và **cleanup** tài nguyên lab Backup.

* Nắm **VM Import/Export**: export VM từ on-premises, upload image lên **S3**, import thành **AMI** và khởi chạy **EC2**.

* Chuẩn bị **VMware Workstation**, hoàn thành chuỗi export → upload → import theo workshop migration.

* Chạy ứng dụng **local** với dependencies; hiểu Dockerfile và quy trình đóng gói image.

* Cấu hình **VPC**, security group và **IAM role** cho EC2 truy cập **Amazon ECR**.

* Khởi tạo **Amazon RDS**, gắn database vào luồng ứng dụng container trên EC2.

* **Deploy** ứng dụng bằng Docker image và **Docker Compose**; **push image** lên ECR hoặc Docker Hub.

* Tạo **ECS cluster** với **AWS Fargate** (serverless container, không quản lý EC2 host).

* Định nghĩa **task definition** cho backend và frontend; cấu hình container port, image và biến môi trường.

* Thiết lập **Application Load Balancer**, **target group** và routing traffic tới ECS service.

* Triển khai backend service với **blue/green deployment** và frontend service với **rolling deployment**; kiểm tra truy cập ứng dụng và **Container Insights**.

* Cấu hình **GitLab pipeline** và **GitLab Runner** cho luồng build/deploy tự động.

* Thiết lập **GitHub Actions workflow** deploy lên ECS; tạo **CodeBuild project** cho frontend và backend.

* Theo dõi **kết quả deployment** và **application logs** sau mỗi lần chạy pipeline.

* Bật **AWS Security Hub**, xem **security standards**, security score và danh sách **findings**.

* Đánh giá findings theo các chuẩn như **AWS Foundational Security Best Practices**, **CIS** và **PCI DSS**; cleanup sau lab.

* Tạo **VPC Peering connection**, cập nhật **route tables** và cấu hình **cross-peer DNS resolution**.

* Triển khai **AWS Transit Gateway**: attachments, route tables, thêm route vào VPC và **test kết nối** đa VPC.

* Hoàn tất **cleanup** tài nguyên networking, Security Hub và CI/CD để tránh phát sinh chi phí.
