---
title: "Week 12 Worklog"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu Tuần 12:

* **Triển khai cuối cùng:** Triển khai backend đã sẵn sàng cho production (sản xuất) cho Cửa hàng Trang sức.
* **Phân phối nội dung:** Tăng tốc API và phân phối nội dung sử dụng Amazon CloudFront.
* **Quản lý DNS:** Cấu hình tên miền tùy chỉnh sử dụng Amazon Route 53.
* **Trình diễn dự án cuối khóa:** Chuẩn bị hạ tầng backend cho buổi demo cuối cùng.

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Amazon Route 53 & CloudFront <br> - **Thực hành:** <br>&emsp; + Cấu hình Route 53 cho tên miền tùy chỉnh (nếu có) <br>&emsp; + Thiết lập CloudFront distribution cho ảnh S3 và tăng tốc API | 17/11/2025 | 17/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - **Dự án Nhóm:** Triển khai Production <br> - **Thực hành:** <br>&emsp; + Kích hoạt đường ống "Release" trong CodePipeline <br>&emsp; + Xác minh tính ổn định của môi trường production | 18/11/2025 | 18/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - **Dự án Nhóm:** Kiểm thử tải & Xác minh Auto Scaling <br> - **Thực hành:** <br>&emsp; + Giả lập lưu lượng truy cập để test chính sách Auto Scaling <br>&emsp; + Giám sát tỷ lệ lỗi trong CloudWatch | 19/11/2025 | 19/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - **Dự án Nhóm:** Kiểm thử tích hợp (Backend + Frontend) <br> - **Thực hành:** <br>&emsp; + Hỗ trợ team frontend trong việc tích hợp cuối cùng <br>&emsp; + Sửa các lỗi (bug) phút chót | 20/11/2025 | 20/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Thuyết trình Dự án cuối khóa:** <br>&emsp; + Chuẩn bị sơ đồ kiến trúc (tập trung vào Backend) <br>&emsp; + Demo chức năng API và quy trình CI/CD | 21/11/2025 | 21/11/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 12:

* Triển khai thành công backend Web bán trang sức lên môi trường production.
* Cấu hình CloudFront CDN để giảm độ trễ cho người dùng toàn cầu khi truy cập hình ảnh sản phẩm.
* Xác thực độ tin cậy của hệ thống thông qua kiểm thử tải và kiểm tra khả năng Auto Scaling.
* Hoàn tất việc tích hợp với frontend và bàn giao một dự án thương mại điện tử hoạt động đầy đủ.