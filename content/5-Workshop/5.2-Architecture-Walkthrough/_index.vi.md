---
title: "Hướng dẫn Chi tiết về Kiến trúc"
weight: 52
chapter: false
pre: " <b> 5.2. </b> "
---

![Architecture](/images/architecture.drawio.png)
<p align="center"><em>Hình minh họa: Triển khai AWS Jewelry Web (phỏng theo bố cục workshop).</em></p>

---

## 5.2.1 Người dùng & Frontend

- **Trình duyệt (Browser)**: Khách hàng duyệt danh mục, chi tiết sản phẩm, giỏ hàng, thanh toán.
- **CloudFront + S3 (Static Hosting)**: Bản dựng React được phân phối toàn cầu với TLS của ACM; Route 53 ánh xạ tên miền. CloudFront có quyền truy cập nguồn gốc (origin access) vào S3.
- **Cognito**: User Pool cấp JWTs (JSON Web Tokens) cho đăng ký/đăng nhập; React sử dụng các token này để gọi API.

Ý nghĩa: Phân phối toàn cầu nhanh chóng (mục tiêu <2 giây), SSL tiêu chuẩn, và token xác thực cho các lệnh gọi API được bảo vệ.

---

## 5.2.2 API & Dữ liệu

- **Lightsail API (.net)**: Thực hiện CRUD cho sản phẩm, giỏ hàng và điều phối việc tải lên hình ảnh. Chạy với quyền IAM role để đọc Secrets Manager.
- **Lightsail DB (MySQL/Postgres)**: Lưu trữ danh mục, người dùng, giỏ hàng/đơn đặt hàng. Riêng tư (Private) với instance API.
- **Secrets Manager**: Chứa mật khẩu DB và tên bucket (`DB_PASSWORD`, `APP_CONFIG`). API truy xuất khi khởi động/sử dụng lần đầu.
- **CloudWatch Logs**: Thu thập nhật ký truy cập/lỗi API; làm cơ sở cho các truy vấn vận hành và phân tích nhẹ.
- **S3 (Media Bucket)**: Bucket riêng tư (Private) cho hình ảnh sản phẩm. Quy trình tải lên sử dụng Presigned URLs từ API; CloudFront đọc hình ảnh.

Điểm nhấn bảo mật: HTTPS trên mọi đường truyền, quyền truy cập từ CloudFront đến S3 được kiểm soát, thông tin xác thực DB không bao giờ bị hardcode, IAM cấp quyền tối thiểu trên API role.

---

## 5.2.3 Danh tính & Quyền truy cập

- **Amazon Cognito**: Đăng ký/đăng nhập, cấp token; API xác thực JWT cho các route được bảo vệ.
- **Route 53 + ACM**: Tên miền + chứng chỉ TLS cho CloudFront; tùy chọn tên miền tùy chỉnh cho API nếu được công khai.
- **IAM**: Role của instance API chỉ giới hạn cho Secrets Manager + CloudWatch; chính sách S3 bucket cấp quyền đọc cho CloudFront; tải lên chỉ thông qua Presigned PUT.

---

## 5.2.4 Vận hành & Khả năng Quan sát

- **CloudWatch**: Nhật ký/chỉ số (logs/metrics) của API, cảnh báo (alarms) cho tỷ lệ lỗi/độ trễ.
- **Sao lưu/Phục hồi thảm họa (nhẹ nhàng)**: Ảnh chụp nhanh DB (DB snapshots) qua lịch trình Lightsail; tính năng lập phiên bản S3 (S3 versioning) là tùy chọn.
- **Cấu trúc Chi phí**: Lightsail cho chi phí hàng tháng dễ dự đoán; S3/CF/Cognito/SM/CW ở mức tối thiểu với lưu lượng truy cập đã nêu.

---

## 5.2.5 Các Giai đoạn Triển khai (6–12 tuần kể từ đề xuất)

1) **Đánh giá (Tuần 1)**: Yêu cầu, sơ đồ kiến trúc, lược đồ DB (DB schema), danh sách bí mật.
2) **Hạ tầng cơ sở (Tuần 2)**: Hosting S3, CloudFront+ACM, Route 53, thiết lập Lightsail API/DB, Cognito, các mục Secrets Manager, ghi log CloudWatch.
3) **Backend (Tuần 3)**: Truy xuất Secrets, tải lên bằng Presigned URL, CRUD, xác minh token Cognito, ghi log CloudWatch.
4) **Frontend (Tuần 4)**: UI cửa hàng React, UI đăng nhập Cognito, UI tải lên, tích hợp API, triển khai lên S3+CF.
5) **Kiểm thử & Go-live (Tuần 5)**: Tích hợp FE↔API↔S3↔DB, kiểm tra bảo mật (IAM/SM), kiểm thử E2E (đầu cuối).
6) **Bàn giao (Tuần 6)**: Quy trình cập nhật Secrets (Secrets update runbook), chuyển giao quyền sở hữu, hướng dẫn vận hành.