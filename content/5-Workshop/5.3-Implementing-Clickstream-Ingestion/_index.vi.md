---
title: "Triển khai Ghi nhận Clickstream (Clickstream Ingestion)"
weight: 53
chapter: false
pre: " <b> 5.3. </b> "
---

## 5.3.1 "Ghi nhận" có ý nghĩa gì đối với AWS Jewelry Web

Chúng tôi giữ cho việc ghi nhận nhẹ nhàng bằng cách sử dụng API Lightsail hiện có và log có cấu trúc của CloudWatch, thay vì sử dụng một pipeline clickstream riêng biệt.

- Frontend React phát ra các sự kiện sản phẩm chính: `page_view` (xem trang), `product_view` (xem sản phẩm), `add_to_cart` (thêm vào giỏ hàng), `checkout_start` (bắt đầu thanh toán), `checkout_complete` (hoàn tất thanh toán).
- Các sự kiện được POST đến **Lightsail API**, API này sẽ xác thực và ghi chúng dưới dạng JSON vào **CloudWatch Logs**.
- Hình ảnh sản phẩm được tải lên thông qua **Presigned URLs** được tạo bởi API; CloudFront đọc hình ảnh từ S3 bucket riêng tư (private).

Điều này đáp ứng nhu cầu quan sát (observability) trong khi vẫn nằm trong giới hạn chi phí/phạm vi của dự án.

---

## 5.3.2 Lược đồ Sự kiện/Log (đề xuất)

- Danh tính (Identity): `userId` (nếu đã đăng nhập), `clientId`, `sessionId`, `userLoginState` (trạng thái đăng nhập người dùng), `identitySource` (nguồn danh tính).
- Sự kiện (Event): `eventName` (tên sự kiện), `eventTimestamp` (dấu thời gian sự kiện).
- Ngữ cảnh Sản phẩm (Product context): `productId`, `name` (tên), `category` (danh mục), `brand` (thương hiệu), `price` (giá), `discountPrice` (giá chiết khấu), `urlPath` (đường dẫn URL).
- Yêu cầu/Meta: `path` (đường dẫn), `method` (phương thức), `statusCode` (mã trạng thái), `latencyMs` (độ trễ tính bằng mili giây), `userAgent`, `sourceIp` (IP nguồn), `requestId` (ID yêu cầu).

Lưu trữ dưới dạng JSON có cấu trúc trong CloudWatch để xuất/truy vấn sau này.

---

## 5.3.3 Trách nhiệm của API

- `/events`: chấp nhận các payload sự kiện đã được xác thực; từ chối các loại không xác định.
- Tải lên bằng Presigned URL: cấp các URL PUT với các ràng buộc về loại nội dung (content-type) và khóa (key).
- Bí mật (Secrets): truy xuất `DB_PASSWORD` và `APP_CONFIG` (tên bucket) từ **Secrets Manager**; không hardcode bất kỳ bí mật nào.
- Ghi log: ghi các mục CloudWatch có cấu trúc cho thành công/thất bại xác thực, CRUD, tải lên, và các sự kiện kinh doanh.

---

## 5.3.4 Kết nối Frontend (Frontend Wiring)

- Sử dụng JWT của Cognito cho các lệnh gọi đã xác thực; xử lý khách truy cập bằng `clientId` + `sessionId` được tạo.
- Lưu trữ liên tục `clientId` (localStorage) và `sessionId` (sessionStorage) với thời gian chờ nhàn rỗi (idle timeout).
- POST không chặn (non-blocking) và không cần phản hồi (fire-and-forget) cho mỗi sự kiện; việc thử lại là tùy chọn nhưng không được chặn UI.
- Quy trình tải lên hình ảnh: yêu cầu Presigned URL → PUT tệp lên S3 → gửi metadata/key trở lại API.

---

## 5.3.5 Danh sách kiểm tra Xác thực

1. Kích hoạt các sự kiện (duyệt, xem sản phẩm, thêm vào giỏ hàng, bắt đầu/hoàn tất thanh toán).
2. Xác nhận API trả về 2xx; xác minh việc tải lên S3 thành công qua Presigned URL.
3. Trong CloudWatch Logs, xác minh các sự kiện có cấu trúc với các trường danh tính/sản phẩm.
4. Xác nhận việc đọc Secrets Manager (không hardcode thông tin xác thực/bucket DB).
5. Xác minh CORS + HTTPS từ đầu đến cuối; đảm bảo CloudFront có thể đọc bucket hình ảnh, việc tải lên là riêng tư.