---
title: "Worklog Tuần 3"
 
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

* Phân biệt các trường hợp sử dụng Site-to-Site VPN và Client-to-Site VPN.
* Mô tả lợi ích, các thành phần (Hosted/Dedicated Connections) và độ trễ của AWS Direct Connect.
* Xác định chức năng của ELB như một dịch vụ được quản lý để phân phối lưu lượng và kiểm tra tình trạng (health checking).
* Nhận biết rằng NLB hỗ trợ IP tĩnh và hiệu năng cao, trong khi ALB hỗ trợ định tuyến dựa trên đường dẫn (path-based routing).
### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Kết nối VPN Site-to-Site <br>&emsp; + Khái niệm: Mô hình lai kết nối Data Center với AWS VPC <br>&emsp; + Thành phần: Virtual Private Gateway (phía AWS) & Customer Gateway (phía Khách hàng) <br>&emsp; + Hiểu về khả năng kết nối giữa các dải IP khác nhau | 22/09/2025 | 22/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2   | - VPN Client-to-Site & Direct Connect <br>&emsp; + Client-to-Site: Các trường hợp sử dụng và giải pháp trên AWS Marketplace (vd: Cisco Meraki) <br>&emsp; + AWS Direct Connect: Lợi ích (Độ trễ thấp 20-30ms) <br>&emsp; + Phân biệt Hosted Connections (qua các đối tác như Viettel, FPT) với Dedicated Connections | 23/09/2025 | 23/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Cơ bản về Elastic Load Balancing (ELB) <br>&emsp; + Khái niệm: Dịch vụ được quản lý để phân phối lưu lượng (EC2/Containers) <br>&emsp; + Tính năng chính: Health checks & Access Logs (tích hợp S3) <br>&emsp; + Sticky Session: Khái niệm và tầm quan trọng trong việc duy trì trạng thái người dùng | 24/09/2025 | 24/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Application Load Balancer (ALB) & Network Load Balancer (NLB) <br>&emsp; + ALB (Layer 7): Giao thức HTTP/HTTPS và định tuyến theo đường dẫn (/mobile vs /desktop) <br>&emsp; + NLB (Layer 4): Giao thức TCP/TLS, hỗ trợ IP tĩnh và xử lý hiệu năng cao | 24/09/2025 | 24/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Các Load Balancer khác & Ôn tập <br>&emsp; + Tổng quan Classic Load Balancer (CLB - Layer 4/7) <br>&emsp; + Gateway Load Balancer (GLB - Layer 3) & giao thức GENEVE <br>&emsp; + Ôn tập: So sánh VPN vs. Direct Connect & ALB vs. NLB | 25/09/2025 | 25/09/2025 | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:

* Làm chủ Kết nối Lai (Hybrid Connectivity Mastery):Hiểu cách kết nối Data Center với AWS sử dụng Site-to-Site VPN (Virtual Private Gateway) hoặc AWS Direct Connect (độ trễ thấp, 20-30ms).
* Chiến lược VPN (VPN Strategy): Phân biệt được các trường hợp sử dụng Site-to-Site (kết nối hạ tầng) và Client-to-Site VPN (người dùng truy cập).
* Thành thạo ELB (ELB Proficiency): Phân biệt giữa Application Load Balancer (Layer 7, định tuyến theo đường dẫn) và Network Load Balancer (Layer 4, hỗ trợ IP tĩnh, hiệu năng cao).
* Quản lý Lưu lượng (Traffic Management): Biết cách sử dụng Sticky Sessions để duy trì trạng thái người dùng và Health Checks để đảm bảo độ tin cậy của hệ thống.


