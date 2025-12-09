---
title: "Đề xuất Dự án"
date: 2025-11-10
weight: 1
chapter: false
pre: " <b> 2. </b> "
---
# **Nền tảng Thương mại Điện tử Trang sức**
## **Hệ thống Bán hàng Trực tuyến Dựa trên Đám mây Sử dụng React, .NET và MySQL trên AWS Lightsail**

---

## **1. Tổng quan Dự án**

Nền tảng Thương mại Điện tử Trang sức là một hệ thống bán lẻ trực tuyến hiện đại được xây dựng trên hạ tầng đám mây, được thiết kế cho các doanh nghiệp trang sức vừa và nhỏ. Dự án nhằm mục đích giúp các doanh nghiệp này chuyển đổi từ mô hình bán hàng truyền thống sang một môi trường kỹ thuật số an toàn, linh hoạt và tự động.

Nền tảng tích hợp giao diện người dùng **ReactJS**, backend **.NET Core** được host trên Amazon Lightsail, và cơ sở dữ liệu **MySQL** để quản lý sản phẩm, người dùng và đơn hàng một cách hiệu quả.

Các tài nguyên tĩnh như hình ảnh sản phẩm và nội dung web được lưu trữ trên **Amazon S3** và phân phối toàn cầu qua **Amazon CloudFront**, đảm bảo tốc độ và bảo mật tối ưu. **Amazon Cognito** xử lý xác thực người dùng, trong khi **Amazon CloudWatch**, **AWS CloudTrail**, và **Lightsail Snapshots** cung cấp khả năng giám sát, kiểm tra (auditing) và phục hồi sau thảm họa.

Dự án này mang lại một giải pháp thương mại điện tử **hiệu quả về chi phí, dễ vận hành và có khả năng mở rộng** được thiết kế riêng cho các doanh nghiệp trang sức vừa và nhỏ.

### **Mục tiêu Dự án**
- Phát triển một trang web thương mại điện tử trang sức đáp ứng, thân thiện với người dùng và hoạt động liền mạch trên mọi thiết bị.
- Tập trung quản lý sản phẩm, tồn kho và đơn hàng.
- Đảm bảo thời gian hoạt động **≥99.9%** thông qua sao lưu và phục hồi tự động.
- Duy trì chi phí hạ tầng **dưới 65 USD/tháng** bằng cách sử dụng Lightsail và các tài nguyên AWS Free Tier.

### **Giá trị Kinh doanh**
Các cửa hàng trang sức nhỏ thường đối mặt với thách thức liên quan đến chi phí hạ tầng cao và chuyên môn kỹ thuật hạn chế. Giải pháp này giúp:
- Giảm chi phí vận hành với chi phí Lightsail có thể dự đoán được.
- Tự động hóa các tác vụ lặp đi lặp lại, cải thiện hiệu quả.
- Tăng cường niềm tin thương hiệu thông qua sự ổn định của hệ thống và bảo vệ dữ liệu mạnh mẽ.

---

## **2. Phân tích Vấn đề**

### **2.1 Tình hình Hiện tại**

Thị trường bán lẻ trang sức đang nhanh chóng chuyển sang các kênh trực tuyến, được thúc đẩy bởi nhu cầu của người tiêu dùng về cá nhân hóa, minh bạch và trải nghiệm kỹ thuật số chất lượng cao — đặc biệt là ở thế hệ trẻ. Tuy nhiên, hầu hết các hệ thống thương mại điện tử hiện tại phải đối mặt với các hạn chế quan trọng:

- **Trải nghiệm người dùng kém:** Tải trang chậm, thiết kế lỗi thời và thiếu các tính năng tương tác như thử đồ AR (Augmented Reality).
- **Thiếu niềm tin:** Khách hàng ngần ngại mua các mặt hàng có giá trị cao (vàng, kim cương) do lo ngại về bảo mật dữ liệu và tính xác thực.
- **Hạ tầng không an toàn, lỗi thời:** Nhiều hệ thống kế thừa lưu trữ mật khẩu hoặc dữ liệu khách hàng dưới dạng văn bản thuần túy, khiến chúng dễ bị tấn công và giới hạn khả năng mở rộng.

---

### **2.2 Thách thức Chủ yếu**

- **Tốc độ và độ tin cậy:**
  Trang sức phụ thuộc rất nhiều vào hình ảnh độ phân giải cao và nội dung tương tác (ví dụ: góc nhìn 360°, thử đồ AR). Nếu không có CDN toàn cầu, việc tải nội dung như vậy sẽ trở nên chậm và không đáng tin cậy, dẫn đến tỷ lệ bỏ giỏ hàng cao.

- **Bảo mật và rủi ro gian lận:**
  Các nền tảng thương mại điện tử là mục tiêu hàng đầu của các cuộc tấn công mạng. Nếu không có Web Application Firewall (WAF), hệ thống sẽ bị phơi bày trước các cuộc tấn công chèn SQL (SQL injection), kịch bản chéo trang (cross-site scripting) và vi phạm dữ liệu. Lưu trữ thông tin xác thực trực tiếp trong mã nguồn gây ra rủi ro nghiêm trọng về truy cập trái phép.

- **Mất dữ liệu và các vấn đề phục hồi:**
  Các giao dịch và hồ sơ tồn kho phải chính xác 100%. Lưu trữ cục bộ có nguy cơ mất dữ liệu vĩnh viễn nếu phần cứng bị hỏng. Nếu không có bộ lưu trữ bền vững như Amazon S3, dữ liệu thiết yếu như hóa đơn và hình ảnh sản phẩm có thể không thể phục hồi được.

- **Giám sát và kiểm soát hạn chế:**
  Nếu không có các công cụ giám sát tập trung như CloudWatch, người vận hành chỉ có thể phát hiện các vấn đề sau khi khách hàng báo cáo, làm tăng MTTR (Thời gian Trung bình để Phục hồi) và làm tổn hại niềm tin người dùng.

---

### **2.3 Tác động đến các Bên liên quan**

| **Bên liên quan** | **Tác động Chủ yếu** |
|------------------|----------------|
| Khách hàng | Tăng sự tin tưởng thông qua trải nghiệm mua sắm trực tuyến nhanh, minh bạch và an toàn. |
| Đội ngũ Vận hành | Giám sát hệ thống dễ dàng hơn, với các quy trình sao lưu và phục hồi tự động. |
| Nhà phát triển | Phát triển nhanh hơn thông qua kiến trúc module với API Gateway và Lightsail. |
| Chủ doanh nghiệp | Hoạt động kinh doanh liên tục, giảm rủi ro mất dữ liệu và lợi thế cạnh tranh mạnh mẽ hơn. |

---

### **2.4 Hậu quả Kinh doanh**

- **Thất thoát doanh thu:** Hiệu suất chậm và UX kém làm giảm tỷ lệ chuyển đổi và tăng tỷ lệ bỏ giỏ hàng.
- **Rủi ro danh tiếng và tuân thủ:** Vi phạm dữ liệu (do thiếu WAF hoặc Secrets Manager) có thể dẫn đến các hình phạt nặng và thiệt hại thương hiệu lâu dài — đặc biệt trong ngành hàng xa xỉ.
- **Chi phí vận hành cao hơn:** Thiếu giám sát và sao lưu tự động làm tăng nhân lực và thời gian phục hồi.
- **Khả năng mở rộng hạn chế:** Các hệ thống kế thừa không thể thích ứng với tốc độ tăng trưởng thị trường nhanh, gây ra những cơ hội kinh doanh bị bỏ lỡ.

---

## **3. Kiến trúc Giải pháp**

![Platform Architecture Diagram](/images/2-Proposal/AWS_Architecture.jpg)

### **3.1 Tổng quan Kiến trúc**
Kiến trúc được đề xuất là một nền tảng thương mại điện tử hiệu suất cao được triển khai trên AWS Cloud (Khu vực Singapore) sử dụng thiết kế SPA/microservices tách biệt. Điều này cho phép xử lý động và khả năng mở rộng cao:

- **Phân phối Frontend Tĩnh:** Các tệp React frontend (HTML, CSS, JS) được lưu trữ trên S3 và phục vụ qua CloudFront CDN để phân phối độ trễ thấp trên toàn cầu.
- **Xử lý Backend Động:** Logic nghiệp vụ và dữ liệu (danh mục, giỏ hàng, đơn hàng) được xử lý thông qua API .NET được host trên Lightsail, được công khai an toàn qua API Gateway.

---

### **3.2 Các Dịch vụ AWS được Sử dụng**

| **Danh mục** | **Dịch vụ AWS** | **Chức năng Chính** |
|---------------|----------------|----------------------|
| Mạng & Biên | Route 53, CloudFront, WAF, ACM | Định tuyến DNS, phân phối CDN, bảo vệ web, quản lý SSL |
| Tính toán & API | API Gateway, Lightsail | Quản lý endpoint API và hosting ứng dụng backend |
| Danh tính & Truy cập | Cognito, Secrets Manager | Xác thực/ủy quyền và quản lý thông tin xác thực an toàn |
| Lưu trữ & Cơ sở dữ liệu | S3, Lightsail MySQL | Lưu trữ tài sản tĩnh và cơ sở dữ liệu quan hệ cho dữ liệu giao dịch |
| Khả năng phục hồi & Sao lưu | AWS Backup, S3 Versioning, Glacier, Lightsail Snapshots | Sao lưu tự động, lưu trữ, và kiểm soát phiên bản dữ liệu |
| Giám sát & Kiểm tra | CloudWatch, CloudTrail | Theo dõi hiệu suất thời gian thực và kiểm tra hoạt động API |

---

### **3.3 Thiết kế Thành phần**

- **Lớp Trình bày (Frontend):** Trang React tĩnh được host trên S3, phân phối qua CloudFront. AWS WAF cung cấp bảo vệ cấp biên khỏi các lỗ hổng phổ biến.
- **Lớp Ứng dụng (Backend):** API Gateway xác thực token Cognito, quản lý các yêu cầu API và áp dụng giới hạn tỷ lệ (throttling). Lightsail Instance (Ubuntu) chạy API .NET Core để xử lý logic nghiệp vụ (đơn hàng, sản phẩm, thanh toán).
- **Lớp Dữ liệu:** Cơ sở dữ liệu MySQL trên Lightsail quản lý tất cả dữ liệu quan hệ; thông tin xác thực được lưu trữ an toàn trong Secrets Manager. Amazon S3 lưu trữ các tệp tải lên của người dùng, hình ảnh sản phẩm và các tài sản tĩnh khác.

---

### **3.4 Kiến trúc Bảo mật**

- **Bảo vệ Cấp biên:** WAF lọc các yêu cầu độc hại; ACM thực thi mã hóa HTTPS.
- **Xác thực Người dùng:** Tất cả đăng nhập và đăng ký được xử lý bởi Cognito với xác thực token an toàn.
- **Bảo mật Hạ tầng:** Secrets Manager đảm bảo thông tin xác thực nhạy cảm không bao giờ được lưu trữ dưới dạng văn bản thuần túy.
- **Kiểm tra (Auditing):** CloudTrail ghi lại mọi lệnh gọi API để tuân thủ và truy xuất nguồn gốc.

---

### **3.5 Khả năng Mở rộng và Khả năng Quan sát**

- Khả năng mở rộng toàn cầu qua bộ nhớ đệm và phân phối của CloudFront.
- Tăng trưởng lưu trữ không giới hạn trên S3 cho các tài sản media.
- Giám sát tài nguyên thông qua các chỉ số CloudWatch, cho phép các quyết định mở rộng quy mô chủ động.

---

## **4. Kế hoạch Triển khai Kỹ thuật**

| **Giai đoạn** | **Thời gian** | **Mục tiêu** | **Kết quả Bàn giao** | **Tiêu chí Thành công** |
|------------|---------------|-----------|------------------|----------------------|
| 1. Thiết lập Hạ tầng | Tuần 1–2 | Cấu hình môi trường AWS | S3, CloudFront, Cognito, Route 53, SSL | Môi trường ổn định |
| 2. Phát triển Backend | Tuần 3–5 | Xây dựng API .NET & lược đồ MySQL | Chức năng CRUD, cơ sở dữ liệu | Backend hoạt động |
| 3. Tích hợp Frontend | Tuần 6–7 | Kết nối React SPA với API | Đăng nhập, các trang UI | UI hoạt động |
| 4. Module Tải lên Hình ảnh | Tuần 8–9 | Kích hoạt tải lên S3 | Kiểm tra tải lên thành công | Đạt |
| 5. Giám sát & Sao lưu | Tuần 10–11 | Cấu hình CloudWatch & Snapshots | Cảnh báo và sao lưu tự động | Hệ thống ổn định |
| 6. Kiểm thử & Triển khai | Tuần 12–14 | QA và phát hành cuối cùng | Demo và tài liệu | Toàn bộ hệ thống ổn định |

---

## **5. Lộ trình & Các Mốc quan trọng**

Dự án sẽ kéo dài 14 tuần (Tháng 9–Tháng 12 năm 2025), chia thành sáu sprint theo phương pháp Agile–Scrum.

| **Sprint** | **Kết quả Bàn giao** | **Tiêu chí Thành công** |
|-------------|-----------------|----------------------|
| Sprint 1 – Nền tảng | Thiết lập AWS (Lightsail, S3, CloudFront, Cognito, Route53) | Trang web React được phục vụ qua HTTPS; kiểm tra đăng nhập Cognito thành công |
| Sprint 2 – Backend & DB | API .NET và lược đồ MySQL | Các thao tác CRUD hoạt động chính xác |
| Sprint 3 – Frontend | Các trang React được tích hợp với API | Hiển thị sản phẩm và giỏ hàng hoạt động |
| Sprint 4 – Media | Tích hợp tải lên S3 | Phân phối hình ảnh qua CDN |
| Sprint 5 – Thanh toán & Đơn hàng | Hoàn thành quy trình đặt hàng | Thanh toán và xác nhận đơn hàng hoạt động |
| Sprint 6 – Kiểm thử & Giám sát | Hệ thống hoạt động đầy đủ | Log CloudWatch và sao lưu đã được xác minh |

---

## **6. Ngân sách Ước tính**

| **Dịch vụ** | **Mô tả** | **Chi phí Ước tính Hàng tháng (USD)** | **Ghi chú** |
|--------------|----------------|----------------------------------|------------|
| Lightsail Instance (API .NET) | 2–4 vCPU, 4–8 GB RAM | $10–$40 | Gói ≥$20 được khuyến nghị |
| Lightsail MySQL Database | DB được quản lý 20–50 GB | $15–$50 | Tốt hơn nên tách biệt khỏi instance |
| Amazon S3 | Lưu trữ tệp tĩnh và hình ảnh | $1–$5 | Bao gồm phí yêu cầu |
| Amazon CloudFront | Phân phối CDN | $1–$30 | Miễn phí 1TB đầu tiên/tháng |
| Route 53 + ACM | Tên miền và SSL | $1–$4 | ACM miễn phí; tên miền ~$12/năm |
| Amazon Cognito | Quản lý người dùng | $0–$10 | Miễn phí 10k người dùng đầu tiên |
| CloudWatch + CloudTrail | Giám sát và ghi log | $1–$10 | Phụ thuộc vào dung lượng log |
| Sao lưu | Snapshots và phiên bản | $1–$10 | Khuyến nghị sao lưu tự động hàng tuần |

**Tổng ước tính:** $30–$160/tháng (≈ $90–$480 cho 3 tháng)

---

### **Mẹo Tối ưu hóa Chi phí**

1. Sử dụng AWS Free Tier cho Lightsail, S3, CloudFront và Cognito.
2. Triển khai tại khu vực **ap-southeast-1 (Singapore)** để có độ trễ tối thiểu.
3. Áp dụng Chính sách Vòng đời S3 (S3 Lifecycle Policies) để chuyển các tệp cũ sang Glacier Deep Archive.
4. Bật cảnh báo thanh toán với AWS Cost Explorer và Budgets.
5. Lên lịch chụp nhanh (snapshot) hàng tuần và bật MFA Delete trên S3.

---

## **7. Đánh giá Rủi ro**

| **ID Rủi ro** | **Mô tả** | **Mức độ Nghiêm trọng** | **Tác động** |
|--------------|----------------|--------------|-------------|
| R1 | Lỗi instance Lightsail | Trung bình | Downtime tạm thời |
| R2 | Hỏng dữ liệu cơ sở dữ liệu | Cao | Mất dữ liệu giao dịch |
| R3 | Rò rỉ thông tin xác thực | Trung bình | Rủi ro truy cập trái phép |
| R4 | Quá tải do lưu lượng truy cập tăng đột biến | Trung bình | Trang web chậm hoặc không phản hồi |

---

### **7.1 Chiến lược Giảm thiểu**

- R1: Chụp nhanh Lightsail hàng ngày và quy trình phục hồi chi tiết.
- R2: Sao lưu DB tự động được xuất sang S3 để dự phòng.
- R3: Bắt buộc sử dụng Secrets Manager với xoay vòng khóa.
- R4: Tối ưu hóa bộ nhớ đệm CloudFront và mở rộng Lightsail khi lưu lượng truy cập tăng đột biến.

---

### **7.2 Kế hoạch Dự phòng**

- Phục hồi Hệ thống: Khôi phục instance từ snapshot mới nhất trong vòng 4 giờ.
- Khôi phục Dữ liệu: Khôi phục các bản sao lưu MySQL được lưu trữ trong S3.
- Tính liên tục: Phục vụ trang bảo trì qua S3 + CloudFront trong thời gian downtime.
- Sự cố Bảo mật: Xoay vòng khóa và điều tra bằng log CloudTrail.

---

### **7.3 Kế hoạch Giám sát Rủi ro**

- Giám sát Vận hành: Cảnh báo CloudWatch cho các ngưỡng CPU/mạng.
- Đánh giá Bảo mật: Kiểm tra CloudTrail hàng tuần cho các hoạt động API bất thường.
- Đánh giá Rủi ro Hàng quý: Đánh giá lại ma trận rủi ro sau các bản cập nhật lớn.

---

## **8. Kết quả Mong đợi**

### **8.1 Các Chỉ số Thành công (KPIs)**

**KPI Kỹ thuật**
- Độ trễ Frontend < 200ms (qua CloudFront)
- Phản hồi API < 350ms (qua API Gateway + Lightsail)
- Thời gian hoạt động 99.9% với phục hồi tự động
- 70%+ yêu cầu được phục vụ từ bộ nhớ đệm CDN
- Không có sự cố bảo mật lớn nào (Cognito + WAF)
- RTO < 30 phút, RPO = 0 cho bảo vệ dữ liệu

**KPI Kinh doanh**
- Tăng 20–30% tỷ lệ chuyển đổi
- Cải thiện 15–25% khả năng giữ chân khách hàng
- Giảm 25–40% chi phí hạ tầng
- Chi phí giao dịch thấp hơn phù hợp với hiệu quả giá trị AWS

---

### **8.2 Lợi ích Ngắn hạn (0–6 Tháng)**

- Tải trang nhanh hơn 40–70%, tỷ lệ thoát trang thấp hơn
- Giảm tải backend với bộ nhớ đệm CDN
- Xác thực mạnh mẽ hơn qua Cognito
- Cảnh báo và sao lưu tự động với CloudWatch
- Triển khai tính năng nhanh hơn nhờ kiến trúc tách rời

---

### **8.3 Lợi ích Trung hạn (6–18 Tháng)**

- Chi phí lưu trữ thấp hơn qua chu kỳ sống S3 → Glacier
- API ổn định dưới lưu lượng truy cập cao
- Dự báo chi phí thông qua AWS Cost Explorer
- Điều chỉnh hiệu suất liên tục với dashboard CloudWatch
- Giảm khối lượng công việc bảo trì cho nhà phát triển

---

### **8.4 Giá trị Dài hạn (18+ Tháng)**

- Nền tảng cloud-native có khả năng mở rộng hoàn toàn, sẵn sàng cho việc mở rộng sang di động hoặc thị trường (marketplace)
- Sẵn sàng cho AI/ML để đề xuất trang sức cá nhân hóa
- Giảm 80–90% chi phí lưu trữ qua lưu trữ Glacier
- Bảo mật và tuân thủ cấp doanh nghiệp (IAM, WAF, CloudTrail)
- Tiếp cận toàn cầu với Mạng lưới Biên CloudFront
- Hoạt động bền vững, linh hoạt cho tăng trưởng dài hạn

---

### **8.5 Cải tiến Trải nghiệm Người dùng**

- Tải hình ảnh và duyệt sản phẩm nhanh hơn
- Đăng nhập và theo dõi đơn hàng mượt mà qua Cognito
- Giảm độ trễ và downtime trong giờ cao điểm
- Tăng niềm tin khách hàng thông qua độ tin cậy cấp AWS

---

### **8.6 Khả năng Chiến lược Đạt được**

- Kiến trúc cloud-native, sẵn sàng cho sự phát triển microservices
- Độ trưởng thành FinOps với theo dõi chi phí chi tiết
- Xuất sắc trong vận hành thông qua giám sát và kiểm tra tập trung
- Hạ tầng sẵn sàng cho tương lai có thể mở rộng sang ECS, EKS, hoặc RDS
- Tuân thủ bảo mật cấp cao bằng IAM, Cognito, WAF, Secrets Manager
- Nền tảng vững chắc cho phân tích dữ liệu và tích hợp AI