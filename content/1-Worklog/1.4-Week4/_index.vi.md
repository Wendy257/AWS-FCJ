---
title: "Week 4 Worklog"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4:

* **Năng lực cốt lõi EC2:** Định nghĩa các khái niệm EC2 bao gồm Loại Instance, AMI, và Hypervisor (Nitro).
* **Phân biệt Lưu trữ:** Phân biệt giữa EBS (bền vững, hỗ trợ snapshot) và Instance Store (tạm thời, IOPS cao).
* **Tự động hóa Instance:** Sử dụng User Data cho các script khởi động và Metadata để lấy thông tin instance.
* **Mở rộng & Tối ưu chi phí:** Triển khai Auto Scaling cho tính sẵn sàng cao và chọn Phương án giá phù hợp (On-demand, Reserved, Spot).
* **Sử dụng Lightsail:** Nhận biết Amazon Lightsail là giải pháp chi phí thấp, giá cố định cho các tải công việc nhẹ.

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Các loại EC2 Instance & Tùy chọn mua (Spot, Reserved) <br> - **Thực hành:** <br>&emsp; + Khởi chạy EC2 sử dụng các họ instance khác nhau | 28/09/2025 | 28/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - EBS Volume & Snapshot <br> - **Thực hành:** <br>&emsp; + Tạo và gắn EBS volume <br>&emsp; + Thay đổi kích thước EBS volume mà không cần tắt máy (downtime) | 29/09/2025 | 29/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - AMI & User Data <br> - **Thực hành:** <br>&emsp; + Tạo AMI tùy chỉnh từ một instance <br>&emsp; + Chạy instance với User Data script (cài đặt Apache) | 30/09/2025 | 30/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Elastic Load Balancing (ALB) & Auto Scaling <br> - **Thực hành:** <br>&emsp; + Cấu hình Application Load Balancer <br>&emsp; + Tạo Auto Scaling Group với Target Tracking | 01/10/2025 | 01/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Giới thiệu về Lightsail <br> - Ôn tập use case: EC2 vs Lightsail | 02/10/2025 | 02/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
### Kết quả đạt được trong Tuần 4:

* Khởi chạy và quản lý EC2 instance với quá trình cài đặt tự động qua User Data.
* Thực hiện các thao tác với EBS volume bao gồm gắn, mở rộng dung lượng và tạo snapshot.
* Xây dựng kiến trúc Tính sẵn sàng cao (High Availability) sử dụng Auto Scaling Group và Application Load Balancer.
* Đánh giá được khi nào nên sử dụng Spot Instance so với On-Demand để tiết kiệm chi phí.