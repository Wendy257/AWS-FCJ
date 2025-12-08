---
title: "Week 9 Worklog"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu Tuần 9:

* **Nền tảng CI/CD:** Hiểu về các đường ống Tích hợp liên tục (CI) và Triển khai liên tục (CD).
* **Quản lý mã nguồn:** Sử dụng AWS CodeCommit (hoặc tích hợp GitHub) để kiểm soát phiên bản.
* **Tự động hóa Build:** Cấu hình AWS CodeBuild để biên dịch mã và chạy kiểm thử.
* **Tự động hóa Triển khai:** Sử dụng AWS CodeDeploy để tự động phát hành lên các dịch vụ tính toán.
* **Điều phối đường ống:** Kết nối mọi thứ với AWS CodePipeline.

### Các công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Giới thiệu về AWS Code Services (CodeCommit) <br> - **Thực hành:** <br>&emsp; + Thiết lập repository cho Backend dự án Trang sức <br>&emsp; + Push bộ khung code (skeleton) ban đầu | 27/10/2025 | 27/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - AWS CodeBuild <br> - **Thực hành:** <br>&emsp; + Tạo file `buildspec.yml` <br>&emsp; + Chạy job build để cài đặt dependencies và chạy unit tests | 28/10/2025 | 28/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - AWS CodeDeploy <br> - **Thực hành:** <br>&emsp; + Tạo file `appspec.yml` <br>&emsp; + Triển khai một bản sửa đổi lên EC2 test hoặc Lambda | 29/10/2025 | 29/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - AWS CodePipeline <br> - **Thực hành:** <br>&emsp; + Tạo pipeline kích hoạt khi có Git push -> Build -> Deploy | 30/10/2025 | 30/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Dự án Nhóm (Backend):** <br>&emsp; + Khởi tạo đường ống CI/CD cho API Trang sức <br>&emsp; + Đảm bảo mọi commit đều kích hoạt kiểm tra build | 31/10/2025 | 31/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được trong Tuần 9:

* Thiết lập đường ống CI/CD hoàn toàn tự động cho phát triển backend.
* Tích hợp quản lý mã nguồn với hệ thống build và deploy để giảm thiểu lỗi thủ công.
* Cấu hình kiểm thử tự động trong giai đoạn build để đảm bảo chất lượng code cho dự án nhóm.