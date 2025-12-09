---
title: "Bản đề xuất"
 
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# **Nền tảng thương mại điện tử trang sức** 

## **Hệ thống bán hàng trực tuyến trên nền tảng đám mây sử dụng React, .NET và MySQL trên AWS Lightsail** 

# AWS First Cloud AI Journey – **Project Plan**

# T1VN – FPT University – Video Share

# 09/12/2025

# Mục lục

**[1 BỐI CẢNH VÀ ĐỘNG LỰC 3](#background-and-motivation)**

[1.1 TÓM TẮT ĐIỀU HÀNH 3](#executive-summary)

[1.2 TIÊU CHÍ THÀNH CÔNG CỦA DỰ ÁN 3](#heading=)

[1.3 GIẢ ĐỊNH 3](#heading=)

[**2 SƠ ĐỒ KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC 4**](#heading=)

[2.1 SƠ ĐỒ KIẾN TRÚC KỸ THUẬT 4](#technical-architecture-diagram)

[2.2 KẾ HOẠCH KỸ THUẬT 4](#technical-plan)

[2.3 KẾ HOẠCH DỰ ÁN 5](#heading=)

[2.4 NHỮNG CÂN NHẮC VỀ AN NINH 5](#security-considerations)

[**3 HOẠT ĐỘNG VÀ SẢN PHẨM GIAO 6**](#hoạt-động-và-sản-phẩm)

[3.1 HOẠT-ĐỘNG VÀ SẢN PHẨM GIAO 6](#hoạt-động-và-sản-phẩm-1)

[3.2 NGOÀI PHẠM VI 8](#ngoài-phạm-vi)

[3.3 ĐƯỜNG DẪN ĐẾN SẢN XUẤT 8](#đường-dẫn-đến-sản-xuất)

[**4 PHÂN TÍCH CHI PHÍ DỰ KIẾN CỦA AWS THEO DỊCH VỤ 9**](#phân-tích-chi-phí-aws-dự-đoán-theo-dịch-vụ)

[**5 NHÓM 10**](#nhóm)

[**6 TÀI NGUYÊN & ƯỚC TÍNH CHI PHÍ 10**](#tài-nguyên-&-ước-tính-chi-phí)

[**7 CHẤP NHẬN 11**](#acceptance)

# **BỐI CẢNH VÀ ĐỘNG LỰC**

## 1. **TÓM TẮT TỔNG QUAN**

Dự án Web Trang sức AWS bao gồm việc phát triển một Nền tảng Thương mại Điện tử Trang sức toàn diện. Kiến trúc hệ thống bao gồm cơ sở hạ tầng backend và cơ sở dữ liệu được lưu trữ trên AWS Lightsail, kết hợp với frontend dựa trên React được triển khai thông qua Amazon S3 và CloudFront. Kiến trúc này được thiết kế để đảm bảo khả năng mở rộng, bảo mật cao và tối ưu hóa chi phí vận hành bằng cách tận dụng các dịch vụ đám mây thiết yếu nhưng hiệu quả cao.

**Hệ thống cung cấp các tính năng chính như:**

* Quản lý sản phẩm trang sức.

* Khả năng tải lên hình ảnh sản phẩm.

* Chức năng giỏ hàng.

* Đăng ký và xác thực người dùng thông qua AWS Cognito.

* Các hoạt động API backend trên Lightsail với lưu trữ dữ liệu MySQL/Postgres.

* Tăng tốc phân phối nội dung và xử lý SSL tiêu chuẩn quốc tế thông qua CDN.

## 2.**TIÊU CHÍ THÀNH CÔNG CỦA DỰ ÁN**

* **Hiệu suất:** Thời gian tải trang web dưới 2 giây khi truy cập quốc tế, được hỗ trợ bởi CloudFront CDN.
* **Độ ổn định:** Phần backend hoạt động ổn định trên Lightsail trong điều kiện lưu lượng truy cập thực tế.
* **Tính toàn vẹn dữ liệu:** Bảo mật các hoạt động cơ sở dữ liệu với tốc độ truy xuất nhanh chóng.
* **Quản lý người dùng:** Quản lý người dùng ổn định và bảo mật thông qua Amazon Cognito.
* **Bảo mật:** Quy trình tải lên hình ảnh an toàn thông qua Amazon S3.
* **Giám sát:** Ghi nhật ký API toàn diện thông qua Amazon CloudWatch.

## 3.**GIẢ ĐỊNH**

* **Lưu lượng truy cập:** Mức lưu lượng truy cập trung bình (dưới 100.000 yêu cầu/tháng).
* **Khả năng mở rộng:** Không yêu cầu cấu hình tự động mở rộng nâng cao.
* **Tên miền:** Tên miền đã được mua trước hoặc sẽ được mua thông qua Amazon Route 53.

* **Năng lực:** Đội ngũ phát triển có trình độ thành thạo về Node.js và React.

# **SƠ ĐỒ KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC**

## 1.**SƠ ĐỒ KIẾN TRÚC KỸ THUẬT**

![Kiến trúc](/images/imageworkshop.png)

## 2.**KẾ HOẠCH KỸ THUẬT**

* **Giao diện người dùng:** Được lưu trữ trên Amazon S3 với Amazon CloudFront CDN và HTTPS được kích hoạt thông qua AWS Certificate Manager (ACM).

* **API Backend:** Môi trường thời gian chạy .NET trên AWS Lightsail.

* **Cơ sở dữ liệu:** MySQL/PostgreSQL được lưu trữ trên AWS Lightsail.

* **Xác thực:** Nhóm người dùng Amazon Cognito.

* **Lưu trữ hình ảnh:** Amazon S3.

* **Ghi nhật ký:** Amazon CloudWatch.

* **Quản lý bí mật:** AWS Secrets Manager.

## 3.**KẾ HOẠCH DỰ ÁN**

Thời gian triển khai ước tính từ 6 đến 12 tuần, tùy thuộc vào phạm vi dự án cuối cùng.

## 4.**CÂN NHẮC BẢO MẬT**

* **Kiểm soát truy cập:** Đã cấu hình quyền truy cập riêng tư S3, cấp quyền đọc độc quyền cho CloudFront.

* **Quản lý thông tin xác thực:** API sử dụng khóa riêng được lưu trữ an toàn trong AWS Secrets Manager.
* **Mã hóa:** Triển khai HTTPS toàn hệ thống.
* **Bảo mật xác thực:** Đăng nhập người dùng được bảo vệ bởi Amazon Cognito.

# **HOẠT ĐỘNG VÀ SẢN PHẨM**

## 1.**HOẠT ĐỘNG VÀ SẢN PHẨM**

| Giai đoạn dự án | Dòng thời gian | Hoạt động | Sản phẩm/Cột mốc | MD |
| :---: | :---: | ----- | ----- | :---: |
| **Đánh giá** | Tuần 1 | \- Phân tích các yêu cầu hệ thống của Trang sức Web. \- Soạn thảo Sơ đồ Kiến trúc. **\-** Thiết kế Sơ đồ Cơ sở dữ liệu (Lightsail MySQL/Postgres). \- Xác định các bí mật cần thiết (Mật khẩu DB, tên bucket). | \- Sơ đồ Kiến trúc \- Sơ đồ DB \- Danh sách Bí mật | **5** |
| **Thiết lập Cơ sở hạ tầng Cơ sở** | Tuần 2 | \- Cấu hình lưu trữ S3 và bản dựng React. \- Cấu hình CloudFront CDN và ACM SSL. \- Ánh xạ miền thông qua Route 53\. \- Cung cấp phiên bản API Lightsail. \- Cung cấp Cơ sở dữ liệu Lightsail. \- Cấu hình Đăng nhập Cognito. \- Tạo các mục nhập Secrets Manager:   + Secret 1: DB\_PASSWORD   + Secret 2: APP\_CONFIG (bucket-name) \- Gán vai trò IAM cho các phiên bản API để có quyền truy xuất bí mật. \- Bật nhật ký CloudWatch. | \- CDN Frontend chức năng \- API Backend vận hành \- Thiết lập kết nối cơ sở dữ liệu \- Đăng nhập Cognito chức năng \- Secrets Manager được điền mật khẩu DB và tên bucket | **7** |

| Thiết lập Thành phần 1 – API Backend | Tuần 3 | \- Triển khai logic API để truy xuất mật khẩu DB từ Secrets Manager. \- Triển khai logic API để truy xuất tên bucket từ Secrets Manager. \- Triển khai tải ảnh lên S3 (thông qua URL được chỉ định trước). \- Phát triển các hoạt động CRUD cho các sản phẩm trang sức. \- Tích hợp xác thực Cognito. \- Triển khai truyền nhật ký đến CloudWatch. | \- Hoạt động API ổn định \- Chức năng tải lên hình ảnh thành công \- Chức năng Đăng nhập thành công \- Cấu hình được mã hóa cứng được thay thế hoàn toàn bằng Secrets Manager | 7 |
| :---: | :---: | :---- | :---- | :---: |
| **Thành phần thiết lập 2 – Frontend React** | Tuần 4 | \- Phát triển Giao diện người dùng Cửa hàng Trang sức. \- Triển khai Giao diện đăng nhập Cognito. \- Triển khai Giao diện tải lên hình ảnh trang sức. \- Lấy dữ liệu từ API. \- Xây dựng và triển khai lên S3 \+ CloudFront. | \- Hoàn thiện Giao diện người dùng (UI) \- Thiết lập Tích hợp API | **7** |
| **Kiểm thử & Go-live** | Tuần 5 | \- Kiểm thử tích hợp (FE ↔ BE ↔ S3 ↔ DB). \- Kiểm thử bảo mật (IAM \+ Secrets Manager). \- Kiểm thử đầu cuối. | \- Báo cáo kiểm thử \- Danh sách kiểm tra bảo mật | **5** |
| **Bàn giao** | Tuần 6 | \- Cung cấp hướng dẫn sử dụng Secrets Manager để cập nhật tên bucket/mật khẩu DB. \- Chuyển quyền sở hữu tài khoản AWS. \- Cung cấp Tài liệu Runbook. | \- Runbook Toàn diện \- Đóng Dự án | **5** |

##

## 2.**NGOÀI PHẠM VI**

* Các tính năng AI/Học máy.
* Các chức năng thương mại điện tử phức tạp.
* Khả năng xử lý hình ảnh tiên tiến.
* Triển khai đa vùng hoặc các site Phục hồi Thảm họa (DR).
* Hệ thống quản trị phức tạp.
* Tích hợp với các hệ thống của bên thứ ba.

## 3.**ĐƯỜNG DẪN ĐẾN SẢN XUẤT**

* Tối ưu hóa Hoạt động Xuất sắc.
* Quản lý Bí mật – Củng cố Sản xuất.
* Xử lý Lỗi Mở rộng.
* Xác minh Triển khai & Sản xuất.
* Kế hoạch Phục hồi Thảm họa.
* Bàn giao Sản xuất.

# **PHÂN TÍCH CHI PHÍ AWS DỰ KIẾN THEO DỊCH VỤ**

| Tên dịch vụ | Chi phí trả trước | Chi phí hàng tháng | Khu vực |
| :---- | :---- | :---- | :---- |
| Amazon S3 | 0.00 USD | 0.26 USD | Châu Á Thái Bình Dương (Singapore) |
| Amazon CloudFront (CDN cho FE) | 0,00 USD | 0,17 USD | Châu Á - Thái Bình Dương (Singapore) |
| AWS ACM | 0,00 USD | 0 USD | Châu Á - Thái Bình Dương (Singapore) |
| Amazon Route 53 | 0,00 USD | 0,50–1,00 USD | Châu Á - Thái Bình Dương (Singapore) |
| AWS Lightsail – Cơ sở dữ liệu | 0,00 USD | 10–15 USD | Châu Á - Thái Bình Dương (Singapore) |
| Amazon Cognito | 0,00 USD | 2,00 USD | Châu Á - Thái Bình Dương (Singapore) |
| AWS Secrets Manager | 0,00 USD | 0,40 USD | Châu Á - Thái Bình Dương (Singapore) |
| Amazon CloudWatch | 0,00 USD | 0,30 USD | Châu Á - Thái Bình Dương (Singapore) |
| AWS Lightsail – Máy chủ API | 0,00 USD | 5 \- 10 USD | Châu Á - Thái Bình Dương (Singapore) |

# **ĐỘI NGŨ**

**Đối tác Tài trợ Điều hành**

| Họ và tên | Chức danh | Mô tả | Email / Thông tin Liên hệ |
| :---- | :---- | :---- | :---- |
| | | |

**Các Bên liên quan của Dự án**

| Họ và tên | Chức danh | Bên liên quan của | Email / Thông tin Liên hệ |
| :---- | :---- | :---- | :---- |
| | | |

**Nhóm Dự án Đối tác**

| Họ và tên | Chức danh | Vai trò | Email / Thông tin Liên hệ |
| :---- | :---- | :---- | :---- |
| Nguyễn Duy Hiếu | Chủ sản phẩm | Quản lý Dự án (BE) | Hieundse185047@fpt.edu.vn |
| Lưu Ngọc Ngân Giang | Lập trình viên Phần mềm | Lập trình viên (BE) | luungocngangiang25@gmail.com |
| Nguyễn Huy Hoàng | Lập trình viên Phần mềm | Lập trình viên (FE) | Hoangnhse185092@fpt.edu.vn |
| Trần Hồ Phương Khánh | Nhà phát triển phần mềm | Nhà phát triển (FE) | khanhthpse185070@fpt.edu.vn |
| Tăng Khánh Nhi | Nhà phát triển phần mềm | Nhà phát triển (FE) | tangkhanhhi111@gmail.com |

**Địa chỉ liên hệ nâng cao dự án**

| Tên | Tiêu đề | Vai trò | Email / Thông tin liên hệ |
| :---- | :---- | :---- | :---- |
| | | | |

# **TÀI NGUYÊN & ƯỚC TÍNH CHI PHÍ**
| Tài nguyên | Trách nhiệm | Giá (USD)/Giờ |
| :---- | :---- | ----- |
| Kiến trúc sư Giải pháp \[số lượng nhân sự được phân công\] | \- Thiết kế và kiến ​​trúc hệ sinh thái cơ sở hạ tầng AWS, tích hợp các dịch vụ như S3, CloudFront, Lightsail, ACM, Route 53 và Secrets Manager \- |12|
| Kiến trúc sư Giải pháp \[số lượng nhân sự được phân công\] |\- Cung cấp tư vấn chiến lược về các giao thức bảo mật và tối ưu hóa chi phí vận hành.|12|
| Kiến trúc sư Giải pháp \[số lượng nhân sự được phân công\] |\- Thực hiện đánh giá toàn diện các quy trình triển khai để đảm bảo tuân thủ các thông lệ tốt nhất của ngành.|12|
| Kỹ sư \[số lượng nhân sự được phân công\] | \- Thực hiện việc cung cấp và triển khai các thành phần cơ sở hạ tầng cốt lõi, bao gồm S3, CloudFront, Route 53, ACM và Lightsail. |6|
| Kỹ sư \[số lượng nhân sự được phân công\] |\- Thiết lập các quy trình Tích hợp Liên tục/Triển khai Liên tục (CI/CD) và tạo điều kiện thuận lợi cho việc triển khai các tài sản giao diện người dùng lên Amazon S3. |6|
| Kỹ sư \[số lượng nhân sự được phân công\] |\- Triển khai và cấu hình môi trường thời gian chạy API Node.js trong cơ sở hạ tầng AWS Lightsail.|6|
| Kỹ sư \[số lượng nhân sự được phân công\] |\- Cấu hình quản lý thông tin xác thực an toàn thông qua AWS Secrets Manager và thiết lập cơ chế ghi nhật ký bằng Amazon CloudWatch. | 6 |
| Khác (Vui lòng ghi rõ) | \- Thực hiện kiểm tra tích hợp và xác minh hệ thống nghiêm ngặt sau khi triển khai.|0|

\* Lưu ý: Tham khảo phần “hoạt động & sản phẩm bàn giao” để biết danh sách các giai đoạn của dự án

| Giai đoạn dự án | Kiến trúc sư giải pháp | Kỹ sư | Khác (Vui lòng nêu rõ) | Tổng Số Giờ |
| :---: | :---: | :---: | :---: | :---: |
| S3 \+ CloudFront | 1 | 2 | | 3 |
| API Lightsail \+ DB | 1 | 4 | | 4 |
| Nhận thức | 1 | 2 | | 5 |
| Ghi nhật ký & Giám sát | 1 | 1 | | 3 |
| Tổng số giờ | 4 | 12 | | 15 |
| Tổng chi phí | | | | |

Phân bổ đóng góp chi phí giữa Đối tác, Khách hàng, AWS:

| Bên | Đóng góp (USD) | % Đóng góp trên Tổng |
| :---- | :---- | :---- |
| Khách hàng | | |
| Đối tác | | |
| AWS | | |

# **CHẤP NHẬN**

Dự án hoàn thành khi: 9/12/2025

Website chạy ổn định trên tên miền thật.

API kết nối hoàn toàn với DB.

Tải lên hình ảnh sản phẩm hoạt động.

Nhật ký CloudWatch & đăng nhập Cognito hoạt động.

Được khách hàng/bên liên quan chấp nhận.

## File TEMPLETE DOCX: [DOWLOAD Proposal (DOCX)](https://drive.google.com/drive/folders/1TLXOU4XDvSqv1hfWYhXhWilc5G53iN2H?usp=sharing)