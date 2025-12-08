---
title: "Week 5 Worklog"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu Tuần 5:

* **Lưu trữ Đối tượng (S3):** Nắm vững các lớp lưu trữ S3 (Standard, IA, Glacier), Chính sách vòng đời (Lifecycle), và Phiên bản (Versioning).
* **Lưu trữ Tệp (EFS):** Cấu hình Elastic File System để chia sẻ lưu trữ giữa nhiều EC2 instance.
* **Lưu trữ Lai (Hybrid):** Hiểu các khái niệm Storage Gateway (File, Volume, Tape Gateways).
* **Lưu trữ Web Tĩnh:** Triển khai một trang web tĩnh sử dụng S3.

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Cơ bản về S3 & Bảo mật (Bucket Policies) <br> - **Thực hành:** <br>&emsp; + Tạo S3 bucket & upload đối tượng <br>&emsp; + Cấu hình chặn truy cập công khai (Block Public Access) | 03/10/2025 | 03/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - S3 Versioning & Quy tắc Lifecycle <br> - **Thực hành:** <br>&emsp; + Bật tính năng versioning <br>&emsp; + Tạo quy tắc lifecycle chuyển dữ liệu sang Glacier | 04/10/2025 | 04/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Host Website tĩnh trên S3 <br> - **Thực hành:** <br>&emsp; + Host trang HTML/CSS tĩnh trên S3 <br>&emsp; + Cấu hình bucket policy cụ thể cho truy cập web | 05/10/2025 | 05/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Elastic File System (EFS) <br> - **Thực hành:** <br>&emsp; + Tạo EFS File System <br>&emsp; + Mount EFS vào hai Linux instance khác nhau cùng lúc | 06/10/2025 | 06/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Tổng quan Storage Gateway & Snow Family <br> - Ôn tập Lab Lưu trữ tuần này | 07/10/2025 | 07/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 5:

* Cấu hình S3 bucket với Versioning và Lifecycle policies để đảm bảo độ bền dữ liệu và tối ưu chi phí.
* Host một website tĩnh hoàn chỉnh trên S3 mà không cần quản lý máy chủ.
* Triển khai lưu trữ tệp chia sẻ bằng EFS, xác minh khả năng đọc/ghi đồng thời từ nhiều instance.
* Bảo mật dữ liệu S3 sử dụng Bucket Policy và Block Public Access.