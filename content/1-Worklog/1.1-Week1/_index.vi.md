---
title: "Nhật ký Tuần 1"

weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---




### Các nhiệm vụ trong tuần:
| Ngày | Nhiệm vụ | Tài liệu tham khảo |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| 1 | I. Giới thiệu về Điện toán đám mây <br> &emsp; - Định nghĩa: Điện toán đám mây là việc cung cấp tài nguyên CNTT theo yêu cầu qua Internet với mô hình trả theo mức sử dụng. <br> &emsp; - Lợi ích chính: Tối ưu chi phí, tăng tốc/linh hoạt, dễ mở rộng và phạm vi toàn cầu. <br> &emsp; - Điểm khác biệt của AWS: Vị thế dẫn đầu thị trường, cam kết giảm giá (quy mô kinh tế), và phương thức lấy khách hàng làm trung tâm (Leadership Principles). | <https://youtu.be/b9pK1oG534Q?si=gR7O6gRMBqNt5u0R> |
| 2 | II. Cơ sở hạ tầng toàn cầu của AWS <br> &emsp; - Thành phần chính: Trung tâm dữ liệu, Availability Zones (AZ) (tách biệt lỗi), và Regions (tập hợp ≥ 3 AZ). <br> &emsp; - Edge Locations (POP): Trung tâm phụ để giảm độ trễ cho dịch vụ như CloudFront (CDN), WAF và Route 53 (DNS). <br> &emsp; - Yếu tố chọn Region: Độ trễ (ví dụ: Singapore cho Việt Nam), tính sẵn có dịch vụ (tính năng mới thường ở các region US trước), và chi phí. | |
| 3 | III. Công cụ quản lý AWS <br> &emsp; - AWS Management Console: Giao diện web; tầm quan trọng của việc bảo mật và hạn chế sử dụng Root User so với người dùng IAM thông thường. <br> &emsp; - AWS CLI: Công cụ mã nguồn mở để tương tác với dịch vụ AWS qua lệnh (tương đương yêu cầu API). <br> &emsp; - AWS SDK: Thư viện giúp đơn giản hóa phát triển ứng dụng bằng cách xử lý vòng đời API (chứng thực, retry, chuyển đổi dữ liệu, ...). | |
| 4 | IV. Tối ưu chi phí trên AWS <br> &emsp; - Mô hình định giá: On-Demand, Reserved Instances / Savings Plans (cam kết dài hạn), và Spot Instances (tài nguyên tạm thời). <br> &emsp; - Thực hành: Xóa tài nguyên không dùng, lên lịch tắt tự động cho tài nguyên không hoạt động 24/7, và tận dụng dịch vụ Serverless. <br> &emsp; - Công cụ quản lý chi phí: Sử dụng AWS Budgets để cảnh báo và hành động tự động, và dùng Cost Allocation Tags để theo dõi chi tiêu theo bộ phận/ứng dụng. | |
| 5 | V. Làm việc với Hỗ trợ AWS <br> &emsp; - Các gói hỗ trợ: Tổng quan 4 cấp chính — Basic, Developer, Business và Enterprise. <br> &emsp; - Tăng cấp (Escalation): Có thể nâng cấp tạm thời gói hỗ trợ để đẩy nhanh xử lý sự cố nghiêm trọng. <br> &emsp; - Công cụ: Sử dụng AWS Pricing Calculator để ước tính chi phí. | |

### Tuần 1 — Thành tựu

- Hiểu các khái niệm cốt lõi về điện toán đám mây và giá trị của AWS (lợi ích và điểm khác biệt).
- Xác định được các thành phần hạ tầng toàn cầu của AWS (Regions, Availability Zones, Edge Locations) và tiêu chí chọn region.
- Tạo và cấu hình tài khoản AWS Free Tier; cài đặt và cấu hình AWS CLI.
- Nắm được cách sử dụng AWS Management Console, AWS CLI và vai trò của AWS SDK trong phát triển ứng dụng.
- Hiểu các kỹ thuật tối ưu chi phí và công cụ liên quan (Pricing Calculator, Budgets, Cost Allocation Tags).
- Nắm được các gói hỗ trợ AWS và phương án tăng cấp khi cần xử lý sự cố sản xuất.

### Tuần 1 — Khó khăn / Thách thức

- Ban đầu chưa quen với AWS CLI và các thực hành tốt về IAM (tránh dùng Root user, quản lý khóa truy cập an toàn).
- Khó khăn khi chọn region tối ưu do cần cân bằng giữa độ trễ, chi phí và tính sẵn có dịch vụ.
- Hiểu rõ các mô hình định giá (On-Demand, Reserved/Savings Plans, Spot) và dự đoán chi phí chính xác là phức tạp.
- Thời gian thực hành hạn chế, chưa đủ để đi sâu vào tất cả nội dung.
- Cần đảm bảo quản lý khóa truy cập và cấu hình tài khoản an toàn theo best practices.
