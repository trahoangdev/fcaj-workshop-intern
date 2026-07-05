---
title: "Blog 6"
date: 2026-05-19
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---

# Cách Mạng Hóa Quy trình Phân tích Địa chất với Machine Learning và Amazon EKS

## Nguồn tham khảo

- AWS Architecture Blog: [How ALS GeoAnalytics LITHOLENS ™ revolutionizes core logging through machine learning with Amazon EKS](https://aws.amazon.com/vi/blogs/architecture/how-als-geoanalytics-litholens-revolutionizes-core-logging-through-machine-learning-with-amazon-eks/)
- Tác giả: Saransh Burman, Sriharsh Adari và Shervin Azad
- Ngày phát hành: 19/05/2026

## Tổng quan

Trong ngành khai khoáng, phân tích địa chất và core logging là bước quan trọng để xây dựng mô hình tài nguyên, hỗ trợ thiết kế mỏ và ra quyết định khai thác. Cách làm truyền thống thường cần chuyên gia địa chất kiểm tra mẫu lõi khoan trực tiếp tại hiện trường. Quy trình này tốn thời gian, phụ thuộc nhiều vào kinh nghiệm cá nhân và khó mở rộng khi dữ liệu hình ảnh ngày càng lớn.

Bài viết của AWS trình bày cách ALS GeoAnalytics triển khai nền tảng **LITHOLENS™** trên AWS để tự động hóa core logging bằng machine learning và computer vision. Điểm đáng chú ý là kiến trúc không chỉ tập trung vào mô hình ML, mà còn giải quyết bài toán vận hành: xử lý workload nặng, dùng GPU hiệu quả, tự động scale theo nhu cầu và giảm chi phí khi không có job.

## Vấn đề cần giải quyết

Một dự án khai khoáng có thể cần khoan hàng nghìn lỗ để hiểu cấu trúc thân quặng và tạo mô hình địa chất 3D. Quy trình truyền thống gặp nhiều hạn chế:

- Chuyên gia phải di chuyển đến khu vực xa để kiểm tra mẫu lõi khoan.
- Kết quả phân tích có thể khác nhau giữa các chuyên gia.
- Hình ảnh lịch sử chưa được khai thác hiệu quả.
- Mẫu vật lý có thể bị mất hoặc xuống cấp theo thời gian.
- Việc phụ thuộc vào một nhóm chuyên gia nhỏ tạo bottleneck.
- Dữ liệu không chuẩn hóa làm khó so sánh giữa các dự án.

ALS GeoAnalytics cần một hệ thống có thể xử lý hình ảnh lõi khoan độ phân giải cao, chạy nhiều mô hình machine learning/deep learning và vẫn tối ưu chi phí cho workload không đều.

## Machine learning trong LITHOLENS™

Bài viết mô tả nhiều thành phần ML trong nền tảng LITHOLENS™:

- **Color Extraction**: quét ảnh lõi khoan, trích xuất màu pixel và lưu kết quả vào Amazon S3.
- **Color Clustering**: dùng các thuật toán như K-Means hoặc Gaussian Mixture Model để nhóm màu và làm nổi bật biến đổi khoáng vật.
- **Percentage Report**: chia ảnh theo từng đoạn, ví dụ mỗi 20 cm, rồi tính tỷ lệ phân bố màu theo từng cluster.
- **RoQE Net**: mô hình deep learning để trích xuất tham số địa kỹ thuật như Rock Quality Designation (RQD) và alpha angles.
- **VeinNet và CobbleNet**: nhận diện các đặc điểm địa chất như veins, cobbles và lithological structures.

Điểm mình thấy quan trọng là ML ở đây không chỉ tạo ra kết quả phân loại, mà còn chuẩn hóa quy trình phân tích. Nhờ đó, kết quả giữa các dự án dễ so sánh hơn và ít phụ thuộc vào đánh giá thủ công.

## Kiến trúc giải pháp

![Kiến trúc LITHOLENS trên AWS](../../../images/3-BlogsTranslated/Blog6/architecture.png)

*Nguồn: AWS Architecture Blog - How ALS GeoAnalytics LITHOLENS revolutionizes core logging through machine learning with Amazon EKS*

ALS GeoAnalytics xây dựng LITHOLENS™ bằng kiến trúc hybrid, kết hợp serverless components với containerized workloads.

1. Người dùng gửi job phân tích thông qua Amazon API Gateway.
2. AWS Lambda xử lý API backend, nhận job parameters và cấu hình cần thiết cho EKS.
3. Amazon EKS chạy các container xử lý ML trên GPU-backed G6 instances.
4. Input data được đọc từ Amazon S3.
5. Logs và intermediate results được theo dõi trong quá trình xử lý.
6. Kết quả cuối cùng được lưu vào Amazon S3 hoặc Amazon RDS thông qua API.
7. Khi job hoàn tất, compute resources được tự động scale down để giảm chi phí.

Kiến trúc này tách rõ tầng API nhẹ và tầng compute nặng. Lambda phù hợp cho job submission, status check và result retrieval. EKS phù hợp cho model training/inference cần GPU và thời gian chạy dài hơn.

## Các dịch vụ AWS liên quan

| Dịch vụ | Vai trò trong giải pháp |
| --- | --- |
| Amazon API Gateway | Cung cấp unified REST API để gửi job và kiểm tra trạng thái |
| AWS Lambda | Xử lý API backend và điều phối job |
| Amazon EKS | Chạy container ML workloads trên GPU instances |
| Amazon EC2 G6 instances | Cung cấp GPU compute cho training và inference |
| Amazon S3 | Lưu ảnh lõi khoan, dữ liệu trung gian và một phần kết quả |
| Amazon RDS | Lưu metadata và kết quả có cấu trúc |
| Custom AMIs | Chứa sẵn dependencies và model artifacts để giảm startup time |

## Điểm kỹ thuật đáng chú ý

### Hybrid architecture

Nếu dùng toàn bộ serverless, Lambda sẽ không phù hợp cho workload ML nặng cần GPU và thời gian xử lý dài. Nếu dùng toàn bộ Kubernetes, API layer có thể phải chạy thường trực dù không có job. Cách kết hợp Lambda và EKS giúp hệ thống tận dụng điểm mạnh của từng dịch vụ.

### Scale to zero

Workload trong ngành địa chất không đều. Có lúc nhiều job được gửi cùng lúc, có lúc hệ thống gần như không hoạt động. Bài viết nhấn mạnh việc EKS containers/instances có thể tự động scale down khi không còn job, giúp tránh chi phí GPU chạy idle.

### Custom AMIs để giảm thời gian khởi động

ALS dùng custom Amazon Machine Images chứa sẵn dependencies và model artifacts, giúp giảm thời gian startup từ vài phút xuống dưới 30 giây. Đây là chi tiết rất quan trọng vì scale-to-zero chỉ hiệu quả nếu hệ thống có thể scale up đủ nhanh khi job mới đến.

### Business impact

Theo bài viết, LITHOLENS™ đã được dùng với 10 công ty khai khoáng trên hơn 40 dự án. Nền tảng giúp tăng tính nhất quán trong phân tích, giảm nhu cầu chuyên gia phải đến hiện trường, tăng khả năng theo dõi quyết định và hỗ trợ báo cáo theo thời gian thực.

## Điều mình học được

Trước khi đọc bài này, mình thường nghĩ bài toán ML chủ yếu nằm ở mô hình. Sau khi tìm hiểu, mình thấy với workload công nghiệp, kiến trúc vận hành quan trọng không kém. Một mô hình tốt nhưng chạy trên hạ tầng không tối ưu có thể rất tốn chi phí hoặc khó mở rộng.

Mình cũng thấy bài học rõ ràng về việc chọn đúng dịch vụ cho đúng phần việc. Lambda xử lý API và orchestration nhẹ. EKS xử lý compute nặng. S3 lưu dữ liệu lớn. RDS lưu kết quả có cấu trúc. Custom AMI và scale-to-zero giúp cân bằng giữa hiệu năng và chi phí.

## Kết luận

Bài viết này là một ví dụ thực tế về cách đưa machine learning vào ngành công nghiệp truyền thống bằng kiến trúc cloud-native. ALS GeoAnalytics không chỉ xây dựng mô hình nhận diện địa chất, mà còn thiết kế một nền tảng có thể mở rộng, xử lý dữ liệu lớn và tối ưu chi phí trên AWS.

Nếu áp dụng vào bài toán tương tự, mình nghĩ cần bắt đầu từ đặc điểm workload: dữ liệu lớn hay nhỏ, cần GPU hay không, job đều hay theo đợt, yêu cầu latency ra sao. Từ đó mới quyết định phần nào dùng serverless, phần nào dùng EKS, và cần tối ưu startup/scale-down như thế nào để tránh lãng phí tài nguyên.
