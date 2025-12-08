---
title: "Week 10 Worklog"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu Tuần 10:

* **Cơ sở hạ tầng dưới dạng mã (IaC):** Học cách cấp phát tài nguyên bằng mã (CloudFormation).
* **Quản lý cấu hình:** Sử dụng AWS Systems Manager (Parameter Store) để quản lý các bí mật cấu hình.
* **Xác thực Cognito:** Triển khai luồng đăng ký và đăng nhập cho ứng dụng web.
* **Phát triển Backend dự án:** Bắt đầu triển khai core API cho Cửa hàng Trang sức.

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Cơ bản về AWS CloudFormation <br> - **Thực hành:** <br>&emsp; + Viết template YAML để khởi chạy VPC và Database cho dự án | 03/11/2025 | 03/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - AWS Systems Manager (Parameter Store) <br> - **Thực hành:** <br>&emsp; + Lưu trữ thông tin đăng nhập database an toàn <br>&emsp; + Lấy bí mật trong code backend thông qua lập trình | 04/11/2025 | 04/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Amazon Cognito (User Pools) <br> - **Thực hành:** <br>&emsp; + Tạo User Pool cho Khách hàng mua trang sức <br>&emsp; + Triển khai các API endpoint Đăng ký/Đăng nhập | 05/11/2025 | 05/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - **Dự án Nhóm (Backend):** Thiết kế Database <br> - **Thực hành:** <br>&emsp; + Thiết kế Schema cho 'Sản phẩm' và 'Danh mục' <br>&emsp; + Cấp phát bảng RDS hoặc DynamoDB qua CloudFormation | 06/11/2025 | 06/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Dự án Nhóm (Backend):** Product API <br> - **Thực hành:** <br>&emsp; + Phát triển các API CRUD cho Sản phẩm Trang sức (GET/POST /products) <br>&emsp; + Kiểm thử với Postman | 07/11/2025 | 07/11/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 10:

* Cấp phát cơ sở hạ tầng nền tảng cho dự án (Mạng + DB) bằng CloudFormation templates.
* Bảo mật cấu hình backend bằng Systems Manager Parameter Store thay vì fix cứng trong code.
* Triển khai xác thực người dùng an toàn bằng Amazon Cognito cho ứng dụng Cửa hàng Trang sức.
* Bàn giao các API "Danh mục sản phẩm" ban đầu cho team Frontend sử dụng.