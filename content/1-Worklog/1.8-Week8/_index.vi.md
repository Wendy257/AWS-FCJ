---
title: "Week 8 Worklog"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu Tuần 8:

* **Container hóa:** Hiểu các nguyên tắc cơ bản của Docker và đóng gói ứng dụng thành container.
* **Điều phối Container:** Làm chủ Amazon ECS (Elastic Container Service) để quản lý các tải công việc được container hóa.
* **Serverless Containers:** Sử dụng AWS Fargate để chạy container mà không cần quản lý máy chủ.
* **Quản lý Registry:** Lưu trữ và quản lý các container image sử dụng Amazon ECR (Elastic Container Registry).

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Cơ bản về Docker <br> - **Thực hành:** <br>&emsp; + Cài đặt Docker <br>&emsp; + Build một Docker image cho ứng dụng backend đơn giản (Python/Node.js) | 20/10/2025 | 20/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - Amazon ECR (Elastic Container Registry) <br> - **Thực hành:** <br>&emsp; + Tạo repository trong ECR <br>&emsp; + Push Docker image từ máy local lên ECR | 21/10/2025 | 21/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Các khái niệm Amazon ECS (Elastic Container Service) <br> - **Thực hành:** <br>&emsp; + Định nghĩa Task Definitions <br>&emsp; + Tạo một ECS Cluster | 22/10/2025 | 22/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - AWS Fargate so với EC2 Launch Type <br> - **Thực hành:** <br>&emsp; + Khởi chạy một Fargate task sử dụng image từ ECR <br>&emsp; + Public service thông qua Load Balancer | 23/10/2025 | 23/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Khởi động Dự án Nhóm (Backend):** <br>&emsp; + Định nghĩa kiến trúc Backend cho "Web bán trang sức" <br>&emsp; + Chọn Tech Stack (ví dụ: Express.js/Django chạy trên ECS hoặc Lambda) | 24/10/2025 | 24/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 8:

* Đóng gói thành công ứng dụng backend sử dụng Docker.
* Thiết lập kho lưu trữ container riêng tư trên AWS ECR và quản lý các phiên bản image.
* Triển khai dịch vụ container có tính sẵn sàng cao sử dụng Amazon ECS và Fargate.
* Xác định kiến trúc backend ban đầu cho dự án Cửa hàng Trang sức (Quyết định chọn Microservices hay Monolith).