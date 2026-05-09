---
title: "Worklog Tuần 2"
date: 2026-04-27
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

* Hiểu cách Amazon Route 53 Resolver hỗ trợ hybrid DNS giữa hệ thống DNS on-premises và private DNS zone trên AWS.

* Đã cài đặt, cấu hình và thực hành sử dụng AWS CLI để tương tác với các dịch vụ như EC2, S3, SNS, IAM và VPC.

* Hiểu cách IAM Identity Center hỗ trợ quản lý users, groups, permission sets và quyền truy cập nhiều AWS account từ một nơi tập trung.

* Nắm được các khái niệm trong AWS Backup như backup plan, restore testing, notification và cleanup để tránh phát sinh chi phí không cần thiết.

* Tìm hiểu VM Import/Export và các tình huống sử dụng cho migration, backup và disaster recovery.

* Thực hành triển khai ứng dụng container với Docker, Docker Compose, Docker Hub, Amazon ECR, Amazon RDS và Amazon EC2.

* Hiểu các thành phần chính của Amazon ECS:
  * ECS cluster
  * Task definition
  * ECS service
  * Application Load Balancer
  * Target group
  * Container Insights

* Hiểu các lựa chọn CI/CD cho triển khai ECS bằng GitLab CI/CD, GitHub Actions và AWS CodeBuild.

* Tìm hiểu cách AWS Security Hub tổng hợp security findings và đánh giá compliance theo các chuẩn như AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark và PCI DSS.

* Thực hành các mô hình kết nối mạng với VPC Peering và AWS Transit Gateway, bao gồm route tables, attachments và các bước cleanup.
