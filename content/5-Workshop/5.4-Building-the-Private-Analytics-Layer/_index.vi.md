---
title: "Xây dựng Lớp Phân tích Riêng tư (Private Analytics Layer)"
weight: 54
chapter: false
pre: " <b> 5.4. </b> "
---

## 5.4.1 Phương pháp phân tích tối giản (phù hợp phạm vi/chi phí)

- **Nguồn dữ liệu đáng tin cậy (Source of truth)**: Log API có cấu trúc của CloudWatch (sự kiện và chỉ số yêu cầu).
- **Lưu trữ**: Giữ log trong CloudWatch với thời gian lưu giữ hợp lý (ví dụ: 30–90 ngày).
- **Đường dẫn Xuất (Export path)**: Kích hoạt xuất từ CloudWatch → S3 để phân tích tùy chỉnh khi cần.
- **Truy vấn**: Sử dụng CloudWatch Log Insights cho các cái nhìn nhanh; sử dụng Athena trên log S3 đã xuất để phân tích sâu hơn.

Cách này giúp tránh cần có một DWH (Kho dữ liệu) riêng biệt trong khi vẫn cung cấp khả năng quan sát về hành vi và độ tin cậy.

---

## 5.4.2 ETL nhẹ tùy chọn (nếu cần)

- Thiết lập một **tác vụ xuất (export task)** (hàng ngày) từ CloudWatch Logs sang một S3 bucket (`logs/<yyyy>/<mm>/<dd>/`).
- Trong Athena, tạo một bảng ngoại vi (external table) trên JSON đã xuất; phân vùng theo ngày.
- Tạo một view `events` duy nhất với các trường chính: `eventName`, `userId`, `sessionId`, `productId`, `statusCode`, `latencyMs`, `timestamp`.
- Giữ chi phí thấp bằng cách:
  - Nén dữ liệu xuất (GZIP).
  - Loại bỏ các trường nhiễu có tính đa dạng cao (high-cardinality).
  - Hạn chế thời gian lưu giữ trong S3 (chu kỳ sống chuyển sang Glacier/hết hạn sau X ngày).

---

## 5.4.3 Bảo mật và Quyền truy cập

- **Không có các endpoint phân tích công khai**: dữ liệu xuất nằm trong một S3 bucket riêng tư.
- **Quyền truy cập tối thiểu IAM (IAM least privilege)**:
  - Role của instance API: quyền đọc Secrets Manager, quyền ghi CloudWatch logs, quyền put S3 cho dữ liệu xuất (nếu được sử dụng).
  - CloudFront chỉ đọc từ bucket hình ảnh; tải lên vẫn riêng tư thông qua Presigned PUT.
- **Bí mật (Secrets)**: Mật khẩu DB + tên bucket được lưu trữ trong Secrets Manager; xoay vòng thông qua quy trình (runbook).
- **Mã hóa**: HTTPS ở mọi nơi; bật mã hóa S3 bucket; bucket xuất là riêng tư.

---

## 5.4.4 Kiểm tra Vận hành

- Cảnh báo (Alarms) về tỷ lệ 5xx của API, độ trễ p95, lỗi xác thực.
- Xác thực định kỳ:
  - Các công việc xuất CloudWatch thành công (nếu được kích hoạt).
  - Xác minh JWT của Cognito vẫn vượt qua sau khi thay đổi cấu hình.
  - Quyền tải lên bằng Presigned URL vẫn được giới hạn phạm vi (content-type, key prefix).

---

## 5.4.5 Mở rộng trong Tương lai (ngoài phạm vi hiện tại)

- Giới thiệu một read-replica RDS nhỏ hoặc một DB phân tích chuyên dụng nếu các truy vấn tăng lên.
- Thêm Lambda transform để chuẩn hóa các sự kiện vào một bảng `events` (batch hàng ngày).
- Thêm công cụ BI (QuickSight) trên Athena hoặc DB phân tích để có dashboard phong phú hơn.