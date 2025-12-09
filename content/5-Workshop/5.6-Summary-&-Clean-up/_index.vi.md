---
title: "Tóm tắt & Dọn dẹp"
weight: 56
chapter: false
pre: " <b> 5.6. </b> "
---

## 5.6.1 Tóm tắt

Chúng ta đã lập kế hoạch và xác định phạm vi cho AWS Jewelry Web như một hệ thống thương mại điện tử an toàn, chú trọng chi phí:

- **Frontend**: React trên S3 + CloudFront với ACM TLS và tên miền Route 53.
- **Backend**: API Node.js trên Lightsail; DB trên Lightsail (MySQL/Postgres); Secrets Manager cho mật khẩu DB/tên bucket.
- **Danh tính**: Cognito User Pool cho đăng ký/đăng nhập và xác minh token API.
- **Media**: S3 bucket riêng tư; tải lên thông qua Presigned PUT; CloudFront đọc các đối tượng.
- **Khả năng quan sát**: CloudWatch structured logs cho API + các sự kiện kinh doanh; tùy chọn xuất sang S3/Athena để phân tích sâu hơn.

Tiêu chí thành công: Tải trang <2 giây qua CDN, API ổn định dưới lưu lượng truy cập dự kiến, thao tác DB an toàn, xác thực Cognito ổn định, tải lên an toàn và ghi log API đầy đủ.

---

## 5.6.2 Những điểm chính cần lưu ý

- Giữ bí mật ngoài mã nguồn: Secrets Manager + IAM cấp quyền tối thiểu trên API role.
- Hiệu suất từ biên: CloudFront + S3 + ACM; hạn chế tối đa các bước nhảy backend cho nội dung tĩnh.
- Độ tin cậy là ưu tiên hàng đầu: ghi lại mọi thứ có ý nghĩa (xác thực, tải lên, CRUD, sự kiện sản phẩm) và đặt cảnh báo cho ngưỡng lỗi/độ trễ.
- Tập trung vào chi phí: Lightsail cho chi phí dễ dự đoán; điều chỉnh thời gian lưu giữ CloudWatch; phân tích tùy chọn chỉ khi cần.

---

## 5.6.3 Danh sách kiểm tra Dọn dẹp

- S3 + CloudFront: gỡ bỏ phân phối (distribution) và các đối tượng bucket nếu trang web ngừng hoạt động.
- Cognito: xóa User Pool nếu không còn được sử dụng.
- Lightsail: chụp nhanh (snapshot) hoặc chấm dứt (terminate) các instance API/DB; giải phóng các IP tĩnh.
- Secrets Manager: xóa các bí mật `DB_PASSWORD` và `APP_CONFIG`.
- CloudWatch: xóa các nhóm log/cảnh báo; vô hiệu hóa mọi tác vụ xuất.
- Route 53/ACM: xóa các bản ghi/chứng chỉ không còn được sử dụng.