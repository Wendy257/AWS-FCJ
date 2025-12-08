---
title: "Week 11 Worklog"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu Tuần 11:

* **Logic Backend nâng cao:** Triển khai logic nghiệp vụ phức tạp (Giỏ hàng, Đơn hàng, Thanh toán).
* **Tích hợp API:** Kết nối dịch vụ backend với công cụ bên thứ 3 hoặc các dịch vụ AWS khác.
* **Tích hợp Lưu trữ:** Xử lý upload hình ảnh (Ảnh sản phẩm) sử dụng S3 Presigned URLs.
* **Truy vết & Gỡ lỗi:** Sử dụng AWS X-Ray để truy vết các request qua ứng dụng.

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - **Dự án Nhóm (Backend):** Xử lý hình ảnh <br> - **Thực hành:** <br>&emsp; + Triển khai S3 Presigned URLs để upload ảnh sản phẩm an toàn <br>&emsp; + Tích hợp thông báo sự kiện S3 (tùy chọn) | 10/11/2025 | 10/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - **Dự án Nhóm (Backend):** Logic Giỏ hàng & Đơn hàng <br> - **Thực hành:** <br>&emsp; + Triển khai API 'Thêm vào giỏ' (dùng Redis/ElastiCache hoặc DynamoDB) <br>&emsp; + Tạo các API Quản lý Đơn hàng | 11/11/2025 | 11/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Tích hợp AWS X-Ray <br> - **Thực hành:** <br>&emsp; + Gắn X-Ray SDK vào code backend <br>&emsp; + Trực quan hóa bản đồ dịch vụ (service map) và độ trễ các lệnh gọi API | 12/11/2025 | 12/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - **Dự án Nhóm (Backend):** Giả lập cổng thanh toán <br> - **Thực hành:** <br>&emsp; + Tạo endpoint giả lập xử lý thanh toán <br>&emsp; + Xử lý các trạng thái giao dịch (Pending -> Success/Failed) | 13/11/2025 | 13/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Review & Tối ưu hóa Code Backend <br> - **Thực hành:** <br>&emsp; + Refactor code để tăng hiệu năng <br>&emsp; + Đảm bảo tất cả API có tài liệu (Swagger/OpenAPI) | 14/11/2025 | 14/11/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 11:

* Triển khai xử lý file an toàn cho hình ảnh sản phẩm trang sức bằng S3.
* Phát triển các tính năng thương mại điện tử cốt lõi bao gồm Giỏ hàng và Xử lý Đơn hàng.
* Tích hợp AWS X-Ray để xác định điểm nghẽn và gỡ lỗi độ trễ trong các API backend.
* Hoàn thiện tài liệu API để tích hợp liền mạch với team Frontend.