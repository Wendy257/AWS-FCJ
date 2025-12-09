---
title: "Trực quan hóa Dữ liệu Phân tích với Bảng điều khiển Shiny (Visualizing Analytics with Shiny Dashboards)"
weight: 55
chapter: false
pre: " <b> 5.5. </b> "
---

## 5.5.1 Lộ trình trực quan hóa thực dụng

Với phạm vi dự án, hãy bắt đầu bằng những gì đã có sẵn:

- **CloudWatch Log Insights**: các truy vấn đã lưu cho độ trễ, 5xx/4xx, lỗi xác thực, số lượng sự kiện sản phẩm (`product_view`, `add_to_cart`, `checkout_*`).
- **Athena (tùy chọn, nếu bật tính năng xuất)**: chạy SQL trên các log JSON đã xuất để xây dựng các phễu đơn giản và các chế độ xem mức độ phổ biến của sản phẩm.
- **QuickSight (tùy chọn)**: bảng điều khiển nhẹ nhàng trên Athena; giữ kích thước SPICE nhỏ.

---

## 5.5.2 Các truy vấn CloudWatch được đề xuất

- **Độ tin cậy**: tỷ lệ lỗi, độ trễ p95 theo tuyến đường (route); các endpoint lỗi nhiều nhất; lỗi xác thực.
- **Hành vi**: đếm theo `eventName`; mức độ phổ biến của sản phẩm (`productId`, `category`); tỷ lệ hoàn thành thanh toán.
- **Tải lên**: thành công so với thất bại đối với Presigned PUT (lọc theo `statusCode` và `path`).

Lưu các truy vấn và ghim vào một bảng điều khiển CloudWatch để có cái nhìn nhanh về hoạt động.

---

## 5.5.3 Nếu sử dụng Athena/QuickSight

1) Xuất log CloudWatch sang S3 hàng ngày.
2) Tạo một bảng ngoại vi (external table) trên JSON; phân vùng theo ngày.
3) Các chỉ số ví dụ:
   - Sự kiện theo thời gian, hỗn hợp sự kiện theo `userLoginState`.
   - Lượt xem sản phẩm so với thêm vào giỏ hàng so với hoàn thành thanh toán.
   - Tỷ lệ lỗi và độ trễ p95 theo tuyến đường (route).
4) Trong QuickSight, xây dựng 3 hình ảnh trực quan:
   - Các ô KPI (tổng số sự kiện, tỷ lệ lỗi, độ trễ p95).
   - Phễu (xem sản phẩm → thêm vào giỏ hàng → bắt đầu thanh toán → hoàn thành thanh toán).
   - Các sản phẩm hàng đầu (lượt xem và thêm vào giỏ hàng).

---

## 5.5.4 Quyền truy cập và bảo mật

- Giữ bucket xuất riêng tư; giới hạn quyền truy cập Athena/QuickSight cho quản trị viên dự án.
- Sử dụng ranh giới quyền IAM (IAM permission boundaries): quyền chỉ đọc cho nhà phân tích, quyền ghi cho vận hành (ops) chỉ khi cần.
- Không có các endpoint bảng điều khiển công khai; chia sẻ qua truy cập console AWS hoặc PDF đã xuất nếu được yêu cầu.