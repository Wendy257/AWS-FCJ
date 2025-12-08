---
title: "Week 6 Worklog"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu Tuần 6:

* **Cơ sở dữ liệu NoSQL (DynamoDB):** Hiểu về Table, Item, Attribute, và Primary Key (Partition vs Composite).
* **Caching:** Giới thiệu về ElastiCache (Redis/Memcached) để tăng hiệu năng.
* **Di chuyển Database:** Tổng quan về DMS (Database Migration Service).

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Dịch blogs  | 08/10/2025 | 08/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - Tính sẵn sàng cao của RDS (Multi-AZ vs Read Replicas) <br> - **Thực hành:** <br>&emsp; + Mô phỏng failover (lý thuyết hoặc lab) | 09/10/2025 | 09/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Nền tảng Amazon DynamoDB <br> - **Thực hành:** <br>&emsp; + Tạo bảng DynamoDB <br>&emsp; + Put và Get item bằng AWS CLI | 10/10/2025 | 10/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - ElastiCache & DAX (DynamoDB Accelerator) <br> - Học các use case cho chiến lược caching | 11/10/2025 | 11/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - So sánh RDS vs. DynamoDB vs. Redshift <br> - Ôn tập Lab Database tuần này | 12/10/2025 | 12/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 6:

* Triển khai Cơ sở dữ liệu Quan hệ được quản lý (RDS) và thiết lập kết nối an toàn từ EC2 instance.
* Tạo bảng NoSQL DynamoDB và thực hiện các thao tác CRUD cơ bản.
* Hiểu sự khác biệt kiến trúc giữa Multi-AZ (cho phục hồi thảm họa) và Read Replicas (cho mở rộng đọc).
* Biết khi nào nên chọn NoSQL thay vì CSDL Quan hệ dựa trên tính linh hoạt của schema và nhu cầu mở rộng.