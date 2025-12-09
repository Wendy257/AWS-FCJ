---
title: "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Workshop AWS Jewelry Web

![Architecture](/images/architecture.drawio.png)
<p align="center"><em>Hình minh họa: Kiến trúc AWS Jewelry Web được đơn giản hóa (CloudFront + S3, Lightsail API/DB, Cognito, Secrets, CloudWatch).</em></p>

#### Tổng quan

Workshop này ghi lại dự án AWS Jewelry Web: một hệ thống thương mại điện tử trang sức an toàn, chú trọng chi phí, sử dụng các dịch vụ được quản lý của AWS.

- **Frontend**: Ứng dụng một trang React (React SPA) trên **S3 + CloudFront** với ACM TLS và tên miền Route 53.
- **Backend**: Instance **Lightsail** chạy API .net; **Lightsail MySQL/Postgres** cho dữ liệu.
- **Danh tính**: **Amazon Cognito** User Pool cho đăng ký/đăng nhập và xác minh JWT trên API.
- **Media**: S3 bucket **riêng tư** cho hình ảnh sản phẩm; tải lên qua **Presigned PUT**; CloudFront đọc các đối tượng.
- **Bí mật & Khả năng quan sát**: **AWS Secrets Manager** cho mật khẩu DB và cấu hình bucket; **CloudWatch Logs** cho các sự kiện API/kinh doanh.

Mục tiêu thiết kế:

- Tải trang <2 giây trên toàn cầu qua CloudFront.
- API ổn định dưới lưu lượng truy cập dự kiến (<100k yêu cầu/tháng).
- Thao tác DB an toàn; không hardcode bí mật (chỉ Secrets Manager).
- Tải lên an toàn; ghi log API đầy đủ cho vận hành và phân tích cơ bản.

#### Sơ đồ Nội dung

1. [5.1. Mục tiêu & Phạm vi](5.1-Objectives-&Scope/) 
2. [5.2. Hướng dẫn Chi tiết về Kiến trúc](5.2-Architecture-Walkthrough/)  
3. [5.3. Triển khai Ghi nhận Clickstream](5.3-Implementing-Clickstream-Ingestion/)
4. [5.4. Xây dựng Lớp Phân tích Riêng tư](5.4-Building-Private-Analytics-Layer/)
5. [5.5. Trực quan hóa Dữ liệu Phân tích với Bảng điều khiển Shiny](5.5-Visualizing-Analytics-with-Shiny-Dashboards/) 
6. [5.6. Tóm tắt & Dọn dẹp](5.6-Summary-&-Cleanup/)