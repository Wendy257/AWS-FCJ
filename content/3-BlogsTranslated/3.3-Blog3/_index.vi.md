---
title: "Blog 3"
 
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Chuyển đổi từ SQL sang NoSQL: Lập chiến lược di chuyển ứng dụng sang Amazon DynamoDB

Các tổ chức chuyển dịch từ cơ sở dữ liệu quan hệ sang Amazon DynamoDB thường gặp thách thức trong việc thiết kế lại mô hình dữ liệu và phương thức truy cập. Sự thay đổi này đòi hỏi phải hiểu rõ những khác biệt cốt lõi về khả năng truy vấn, phương pháp mô hình hóa dữ liệu và kiến trúc ứng dụng - những yếu tố then chốt để đạt được hiệu năng tối ưu mà vẫn duy trì đầy đủ chức năng của ứng dụng.

Một ví dụ thực tế minh họa rõ những thách thức này: Một nền tảng mạng xã hội đang tăng trưởng nhanh chóng gặp phải tình trạng cơ sở dữ liệu quan hệ gặp khó khăn khi xử lý các truy vấn phức tạp liên quan đến kết nối giữa nhiều bảng, đặc biệt cho các tính năng như bài đăng thịnh hành và tương tác nội dung của người nổi tiếng. Với dự báo số lượng người dùng sẽ tăng gấp nhiều lần trong thời gian tới, kiến trúc hiện tại đang đặt ra những lo ngại về khả năng mở rộng. Trong các sự kiện lan truyền với hàng ngàn người dùng đồng thời, các truy vấn kết nối phức tạp cho nội dung thịnh hành bắt đầu xuất hiện dấu hiệu quá tải, cho thấy kiến trúc hiện tại có thể không đáp ứng được nhu cầu mở rộng ngày càng tăng của nền tảng.

Đây là phần đầu tiên trong loạt bài khám phá cách di chuyển hiệu quả từ SQL sang DynamoDB. Chúng tôi sẽ phân tích cách thức đánh giá cấu trúc cơ sở dữ liệu hiện có và các mẫu truy cập để chuẩn bị cho quá trình di chuyển, tập trung vào phân tích lược đồ, mẫu truy vấn và các chỉ số sử dụng giúp định hướng thiết kế mô hình dữ liệu DynamoDB. Các bài viết tiếp theo sẽ đề cập đến chiến lược mô hình hóa dữ liệu và hiện đại hóa tầng truy cập dữ liệu.

---

## Cơ sở dữ liệu Quan hệ so với NoSQL

When transitioning your application from a traditional relational database to DynamoDB, there are several key differences compared to migrating between relational databases. Understanding these distinctions (detailed in the following table) is critical for a successful modernization.

| Khía cạnh | Cơ sở dữ liệu quan hệ | Amazon DynamoDB |
| :--- | :--- | :--- |
| **Ngôn ngữ Truy vấn** | Sử dụng SQL với các mẫu truy vấn có cấu trúc | Cung cấp các thao tác dựa trên API với độ trễ ổn định ở mức mili giây nhờ truy cập tối ưu qua Partition Key và Sort Key |
| **Mô hình Dữ liệu** | Sử dụng cấu trúc dữ liệu chuẩn hóa đã được kiểm chứng | Đề xuất mô hình dữ liệu phi chuẩn hóa linh hoạt, tối ưu cho hiệu năng và khả năng mở rộng ở mọi quy mô |
| **Framework Truy cập Dữ liệu** | Sử dụng các ORM framework trưởng thành như Entity Framework và Hibernate | Cung cấp bộ SDK gốc toàn diện cho đa ngôn ngữ lập trình, cùng các thư viện chuyên biệt cho phép đạt hiệu suất cao trong khi vẫn duy trì các mẫu hướng đối tượng quen thuộc |

Việc di chuyển từ một cơ sở dữ liệu quan hệ sang một cơ sở dữ liệu quan hệ khác thường có tác động tương đối thấp đến tầng truy cập dữ liệu, do cấu trúc nền tảng và các mẫu truy cập vẫn giữ nguyên. Trong khi đó, việc chuyển sang DynamoDB đòi hỏi một cách tiếp cận khác biệt nhằm tận dụng các khả năng NoSQL mạnh mẽ của nó. Sự thay đổi trong mô hình dữ liệu, ngôn ngữ truy vấn và mẫu truy cập cho phép ứng dụng khai thác hiệu năng cao và khả năng mở rộng của DynamoDB. Các nhóm phát triển có thể điều chỉnh tầng truy cập dữ liệu để phù hợp với các nguyên tắc thiết kế của DynamoDB, tối ưu hóa cho hiệu suất ổn định và hiệu quả vận hành.

Bên cạnh đó, trong quá trình di chuyển cơ sở dữ liệu quan hệ truyền thống, nhóm cơ sở dữ liệu thường đảm nhận chính việc thiết lập, trong khi sự tham gia của nhóm phát triển ứng dụng thường bị hạn chế. Tuy nhiên, khi chuyển đổi sang DynamoDB, sự hợp tác chặt chẽ giữa nhóm ứng dụng và nhóm cơ sở dữ liệu trở nên vô cùng quan trọng. Nhóm phát triển ứng dụng cần làm việc song hành cùng các chuyên gia cơ sở dữ liệu để thiết kế một mô hình DynamoDB tối ưu, phù hợp với yêu cầu truy cập dữ liệu của ứng dụng. Nỗ lực hợp tác này chính là chìa khóa cho một quá trình di chuyển thành công sang cơ sở dữ liệu NoSQL DynamoDB.

---

## Phân tích việc mô hình hóa dữ liệu

Việc di chuyển từ cơ sở dữ liệu quan hệ sang DynamoDB sẽ đạt được hiệu quả tối ưu về hiệu suất và chi phí nếu được lập kế hoạch cẩn thận. Trước tiên, chúng ta cần đánh giá mức độ phức tạp của cơ sở dữ liệu quan hệ hiện có để xác định các yêu cầu cụ thể. Việc đánh giá này sẽ định hướng cho thiết kế mô hình dữ liệu DynamoDB phù hợp, có khả năng mở rộng hiệu quả cùng với khối lượng công việc của bạn. Một đánh giá ban đầu toàn diện sẽ mang lại quá trình chuyển đổi sang DynamoDB suôn sẻ hơn. Quy trình di chuyển bao gồm phân tích cấu trúc dữ liệu hiện tại, xác định các mẫu truy cập và tối ưu hóa cho mô hình cặp khóa-giá trị của DynamoDB.

### Use Case: Nền Tảng Mạng Xã Hội

Để minh họa cho các khái niệm được trình bày xuyên suốt bài viết, chúng tôi sử dụng một nền tảng mạng xã hội cho phép người dùng tạo và tương tác với các bài đăng. Như minh họa trong sơ đồ dưới đây, các thực thể cốt lõi bao gồm: người dùng - chủ thể tạo bài đăng, cùng với các tương tác bình luận và lượt thích đi kèm. Hệ thống theo dõi các chỉ số quan trọng thông qua các thực thể bộ đếm người dùng và bộ đếm bài đăng, được thiết kế để duy trì các thống kê như tổng số bài đăng theo người dùng và chỉ số tương tác theo bài đăng. Sơ đồ thể hiện rõ các mối quan hệ trọng yếu trong mô hình cơ sở dữ liệu quan hệ hiện tại:

- Quan hệ Người dùng - Bài đăng (một-nhiều)
- Quan hệ Bài đăng - Bình luận (một-nhiều) 
- Quan hệ Người dùng/Bài đăng với các bộ đếm tương ứng (một-một)
- Quan hệ Người dùng - Bài đăng thông qua lượt thích (nhiều-nhiều), thể hiện việc một người dùng có thể thích nhiều bài đăng và một bài đăng có thể nhận được lượt thích từ nhiều người dùng

---

### Phân Tích Lược Đồ Cơ Sở Dữ Liệu

Bắt đầu bằng việc kiểm tra kỹ lưỡng cấu trúc cơ sở dữ liệu hiện có. Phân tích này giúp xác định các bảng cần di chuyển và đánh giá mức độ phức tạp của việc mô hình hóa dữ liệu trong DynamoDB.

#### Phân Tích Cấu Trúc Cơ Sở Dữ Liệu

Trước tiên, xem xét kiến trúc cơ sở dữ liệu hiện tại:

- Định danh các bảng (giao dịch, phân tích, nhật ký kiểm toán, v.v.) và các mối quan hệ giữa chúng
- Phân tích kiểu dữ liệu (văn bản, số, datetime, GUID, không gian, v.v.) và xác định data types tương đương trong DynamoDB
- Ghi nhận kích thước bảng và xu hướng tăng trưởng, lưu ý giới hạn kích thước 400 KB cho mỗi item trong DynamoDB

Việc hiểu rõ cách ứng dụng sử dụng các kiểu dữ liệu khác nhau sẽ định hướng cho việc thể hiện chúng trong DynamoDB. Các trường datetime cần được ánh xạ thành chuỗi ISO hoặc số epoch tùy theo yêu cầu truy vấn và sắp xếp. Các trường TEXT cần được đánh giá kích thước khi thực hiện denormalization để kết hợp nhiều trường vào một item duy nhất, chẳng hạn như nội dung bài đăng kèm theo các bình luận được nhúng. Phân tích này hỗ trợ thiết kế một mô hình dữ liệu vừa đảm bảo hiệu suất truy vấn tối ưu vừa duy trì tính toàn vẹn của dữ liệu.

#### Tối ưu hóa Kiến trúc Microservices

Trong quá trình tái cấu trúc hệ thống theo mô hình microservices, việc phân tích sự phụ thuộc dữ liệu giữa các service đóng vai trò then chốt:

- Xác định tập hợp bảng dữ liệu cần thiết cho từng service
- Lập bảng đồ phụ thuộc dữ liệu đa tầng giữa các service
- Xây dựng chiến lược phi chuẩn hóa dữ liệu hợp lý
- Đánh giá các yêu cầu về tính nhất quán dữ liệu_

Cần phân tích kỹ lưỡng nhu cầu chia sẻ và truy xuất dữ liệu xuyên biên giới service. Ví dụ điển hình: service quản lý bài đăng cần hiển thị thông tin tên và ảnh đại diện người dùng, trong khi service thông báo lại yêu cầu thiết lập cá nhân hóa để gửi cảnh báo. Những mối quan hệ đan xen này sẽ trực tiếp ảnh hưởng đến thiết kế mô hình DynamoDB. Cần cân nhắc kỹ giữa hai phương án: duy trì tính độc lập tuyệt đối giữa các service và triển khai phi chuẩn hóa có chọn lọc dựa trên tần suất truy cập và cập nhật. Đồng thời, cần dự phòng các kịch bản điều chỉnh biên giới service khi mô hình ứng dụng phát triển.

#### Chiến lược Di chuyển Từng phần

Khi áp dụng phương pháp di chuyển cục bộ, việc nắm bắt mối quan hệ giữa các bảng được chuyển đổi và bảng giữ nguyên là vô cùng quan trọng:

- Định nghĩa rõ ràng phạm vi di chuyển và các ràng buộc phụ thuộc
- Phân tích toàn diện tác động hiệu năng hệ thống
- Lập kế hoạch tinh chỉnh ứng dụng chi tiết
- Thiết kế cơ chế đảm bảo tính nhất quán dữ liệu
- Xác định lộ trình di chuyển các phase kế tiếp

Xét kịch bản thực tế: các bảng có tần suất truy cập cao (bài đăng, bình luận) được chuyển sang DynamoDB, trong khi dữ liệu phân tích lịch sử vẫn duy trì trên hệ quản trị quan hệ. Cần thiết kế mẫu kiến trúc mới để xử lý các nghiệp vụ phụ thuộc tính toàn vẹn giao dịch liên bảng. Lập kế hoạch xử lý độ trễ tiềm ẩn từ các truy vấn đa nguồn dữ liệu và triển khai cơ chế đồng bộ hóa phù hợp. Hiểu rõ những thách thức này giúp xây dựng chiến lược duy trì tính nhất quán và hiệu năng ứng dụng xuyên suốt quá trình chuyển đổi.

#### Đánh giá Đối tượng Lược đồ

Tái thiết kế các chức năng đối tượng lược đồ SQL cho DynamoDB:

- Phân loại và đánh giá toàn bộ SQL View đang được sử dụng
- Đo lường mức độ phức tạp của Stored Procedure (bao gồm CTE)
- Lập lộ trình chuyển đổi nghiệp vụ xử lý bằng Trigger
- Nhận diện các tham chiếu đến bảng không thuộc phạm vi di chuyển
- Đề xuất giải pháp thay thế cho truy vấn phân tích phức tạp
- Thiết kế cơ chế xử lý dữ liệu quy mô lớn


Các đối tượng đặc thù như View tổng hợp chỉ số tương tác đa bảng, Stored Procedure tính toán xu hướng nội dung theo khung thời gian, hay Trigger cập nhật dữ liệu dẫn xuất cần được tái cấu trúc phù hợp với DynamoDB. Thiết kế mô hình dữ liệu mới cần đảm bảo hỗ trợ truy vấn hiệu quả mà không phụ thuộc vào tính năng SQL. Ưu tiên phương pháp xử lý batch cho các thao tác dữ liệu quy mô lớn và thiết kế cơ chế xử lý nghiệp vụ thay thế hoàn toàn Procedure và Trigger cũ. Phân tích lược đồ toàn diện giúp dự báo yêu cầu triển khai, tối ưu hóa mẫu truy vấn và xây dựng chiến lược đồng bộ dữ liệu hiệu quả.

---

### Phân tích Mẫu Truy vấn

Phân tích chuyên sâu toàn bộ truy vấn SQL trong hệ thống (bao gồm SQL thuần, truy vấn ORM và truy vấn động) để xác định:

- Khóa phân vùng (Partition Key) tối ưu
- Khóa sắp xếp (Sort Key) phù hợp
- Quan hệ giữa các thực thể dữ liệu

#### Phân tích Mệnh đề SELECT

Việc phân tích mệnh đề SELECT giúp hiểu rõ cách ứng dụng tiêu thụ dữ liệu và mối quan hệ giữa các thực thể:

- Truy vấn con: Nhận diện các truy vấn con trong nghiệp vụ hiện tại - thường phục vụ mục đích tổng hợp, lọc điều kiện phức tạp, lấy thông tin thực thể liên quan hoặc truy xuất bản ghi mới nhất. Ví dụ: truy vấn hồ sơ người dùng có thể sử dụng truy vấn con để tính tổng bài đăng và số lượng follower, trong khi truy vấn bài đăng cần lấy danh sách bình luận mới nhất. Hiểu rõ các mẫu này giúp xác định dữ liệu cần phi chuẩn hóa và thuộc tính cần tính toán trước trong DynamoDB.

```sql
-- User profile with total posts count
SELECT u.name, u.profile_pic,
(SELECT COUNT(*) FROM posts WHERE user_id = u.user_id) as post_count
FROM users u
WHERE u.user_id = '123'
```

- Lựa chọn cột đa bảng – Phân tích cách các truy vấn kết hợp dữ liệu từ nhiều bảng, qua đó xác định các nhóm thông tin thường được truy xuất đồng thời. Ví dụ điển hình là truy vấn hiển thị bài đăng kết hợp nội dung với thông tin tác giả và số liệu tương tác trong một lần đọc duy nhất. Phân tích này giúp xác định哪些 thuộc tính từ các thực thể khác nhau nên được kết hợp vào cùng một item để tối ưu hóa thao tác đọc trong DynamoDB.
  
```sql
-- Post with author details
SELECT p.content, p.created_at, u.name, u.profile_pic
FROM posts p
JOIN users u ON p.user_id = u.user_id
WHERE p.post_id = '456'
```

- Hàm do người dùng định nghĩa – Đánh giá việc sử dụng các hàm tùy chỉnh trong truy vấn có chức năng biến đổi hoặc tính toán dữ liệu khi truy xuất. Các hàm phức tạp như tính điểm xu hướng dựa trên nhiều yếu tố tương tác kết hợp trọng số thời gian cần được phân tích kỹ. Hiểu rõ các phép tính này giúp xác định thuộc tính nào nên được tính toán trước và lưu trữ, đồng thời thiết kế mô hình dữ liệu phù hợp để hỗ trợ cập nhật hiệu quả các giá trị đã tính toán trong DynamoDB.
- Cột dẫn xuất – Phân tích các cột được tính toán hoặc xử lý theo điều kiện trong truy vấn, kết hợp nhiều giá trị trường hoặc áp dụng logic nghiệp vụ. Các truy vấn thường dẫn xuất số liệu tương tác tổng hợp và xác định trạng thái nội dung dựa trên ngưỡng tương tác. Phân tích này định hướng cho việc quyết định duy trì các thuộc tính tính toán sẵn trong item và thiết kế mẫu cập nhật hiệu quả trong DynamoDB để đảm bảo các giá trị dẫn xuất luôn được đồng bộ.

```sql
-- Derived engagement score and post status
SELECT post_id, content,
(like_count + comment_count) as total_engagement,
CASE WHEN like_count > 1000 THEN 'TRENDING'
WHEN like_count > 100 THEN 'POPULAR'
ELSE 'NORMAL'
END as post_status
FROM posts
WHERE created_at > DATEADD(year, -1, GETUTCDATE())
```

#### Phân tích Mệnh đề JOIN

Việc phân tích các mệnh đề JOIN trong truy vấn SQL giúp làm rõ mối quan hệ giữa các thực thể và cách ứng dụng kết hợp dữ liệu từ nhiều bảng. Phân tích này đóng vai trò then chốt trong việc xác định chiến lược mô hình dữ liệu tối ưu cho DynamoDB:

- Quan hệ thực thể – Xác định các bảng thường xuyên được kết nối với nhau. Một truy vấn hiển thị bài đăng có thể kết hợp dữ liệu từ bảng bài đăng, bình luận, lượt thích và người dùng, cho thấy các thực thể này thường được truy xuất cùng lúc. Hiểu rõ các kết hợp này giúp xác định dữ liệu cần được phi chuẩn hóa hoặc mô hình hóa chung trong DynamoDB để tối ưu truy cập.
- Điều kiện kết nối – Đánh giá điều kiện và bản số quan hệ giữa các bảng. Ví dụ: một bài đăng có thể kết nối với nhiều bình luận (quan hệ một-nhiều) nhưng chỉ kết nối với một hồ sơ người dùng (quan hệ một-một). Các điều kiện kết nối có thể bao gồm phạm vi ngày tháng hoặc cờ trạng thái bên cạnh việc khớp khóa đơn thuần. Hiểu rõ các bản số này giúp xác định chiến lược mô hình hóa phù hợp—liệu có nên nhúng dữ liệu trong cùng item hay duy trì các item riêng biệt—và hỗ trợ ước lượng mẫu đọc/ghi để đánh giá hiệu năng và chi phí của các phương pháp tiếp cận khác nhau.
  
```sql
-- Post with its comments
SELECT p.*, u.*, c.comment_text, c.created_at
FROM posts p
JOIN comments c ON p.post_id = c.post_id
AND c.created_at > DATEADD(hour, -24, GETUTCDATE())
JOIN users u ON p.user_id = u.user_id
WHERE p.post_id = '456'
```

- Kết nối đa dịch vụ và kiến trúc hybrid – Phân tích các kết nối liên quan đến bảng thuộc các dịch vụ khác nhau hoặc các bảng không nằm trong kế hoạch di chuyển. Ví dụ: nếu dữ liệu bài đăng kết nối với dữ liệu hồ sơ người dùng được quản lý bởi một dịch vụ riêng biệt, hoặc với dữ liệu phân tích còn lưu trong cơ sở dữ liệu quan hệ, các mẫu này sẽ ảnh hưởng đến quyết định mô hình hóa dữ liệu. Phân tích này định hướng chiến lược truy cập dữ liệu xuyên biên giới dịch vụ và giữa các hệ thống cơ sở dữ liệu khác nhau.

#### Phân tích Mệnh đề WHERE

Việc phân tích mệnh đề WHERE trong truy vấn SQL cung cấp thông tin quý giá về cách ứng dụng lọc và truy xuất dữ liệu, từ đó thiết kế các mẫu truy cập hiệu quả trong DynamoDB:

- Bộ lọc đơn giá trị – Nhận diện các bộ lọc đơn giá trị được sử dụng thường xuyên, chẳng hạn như lọc bài đăng theo ID người dùng cụ thể hoặc truy xuất bài đăng theo trạng thái. Các mẫu này giúp xác định partition key tiềm năng cho bảng chính hoặc global secondary index (GSI) trong DynamoDB, cho phép truy xuất dữ liệu hiệu quả theo các mẫu truy cập đa dạng.

```sql
-- Single value filter
SELECT * FROM posts WHERE user_id = '123'
AND status = 'ACTIVE'
```

- Bộ lọc theo khoảng – Phân tích các truy vấn kết hợp định danh thực thể với điều kiện phạm vi. Ví dụ, truy vấn lọc bài đăng theo ID người dùng kèm khoảng thời gian cho thấy cách thức xây dựng khóa tổng hợp trong DynamoDB. Hiểu rõ các mẫu này giúp xác định sort key phù hợp để hỗ trợ truy vấn phạm vi hiệu quả trong cùng partition.

```sql
-- Getting user's recent posts
SELECT * FROM posts
WHERE user_id = '123'
AND created_at > DATEADD(year, -1, GETUTCDATE())
ORDER BY created_at DESC
```

- Bộ lọc đa giá trị – Xem xét các điều kiện liên quan đến nhiều giá trị khả dụng cho một thuộc tính, như tìm bài đăng theo nhóm ID người dùng hoặc bài đăng chứa hashtag cụ thể. Các mẫu này ảnh hưởng trực tiếp đến quyết định thiết kế mô hình dữ liệu DynamoDB. Chẳng hạn, việc truy xuất bài đăng từ nhiều người dùng có thể yêu cầu truy vấn riêng biệt sử dụng GSI với ID người dùng làm partition key, trong khi tìm kiếm bài đăng theo hashtag có thể tận dụng cấu trúc chỉ mục đảo với hashtag đóng vai trò partition key kèm theo danh sách ID bài đăng liên quan.

```sql
-- Finding posts with specific hashtags
SELECT DISTINCT p.*
FROM posts p
JOIN post_hashtags ph ON p.post_id = ph.post_id
WHERE ph.hashtag IN ('#POPULAR', '#TRENDING')
```

- Bộ lọc xuyên thực thể – Nhận diện các bộ lọc trải rộng trên nhiều thực thể trong truy vấn SQL. Ví dụ điển hình là truy vấn tìm bài đăng dựa trên cả thuộc tính bài đăng và thuộc tính người dùng (như vị trí hoặc loại người dùng), phản ánh mối quan hệ ảnh hưởng đến quyết định phi chuẩn hóa trong mô hình DynamoDB. Khi những bộ lọc này liên quan đến thực thể được quản lý bởi các microservice khác nhau hoặc các thực thể không được di chuyển sang DynamoDB, cần đánh giá kỹ tác động hiệu năng của việc lọc phía máy khách và truy xuất dữ liệu xuyên biên giới dịch vụ.

```sql
-- Finding active user posts with high engagement
SELECT p.post_id, p.content,
FROM posts p
JOIN users u ON p.user_id = u.user_id
WHERE u.status = 'ACTIVE'
AND p.like_count > 100
ORDER BY p.created_at DESC
```

#### Phân tích Mệnh đề ORDER BY và TOP/LIMIT

Việc phân tích các mệnh đề ORDER BY và TOP/LIMIT giúp nhận diện các yêu cầu sắp xếp và phân trang trong ứng dụng, từ đó thiết kế sort key hiệu quả trong DynamoDB:

- Yêu cầu sắp xếp – Xác định các thuộc tính và tổ hợp thuộc tính được sử dụng để sắp xếp dữ liệu. Ví dụ: sắp xếp bài đăng theo ngày tạo để hiển thị nội dung mới nhất, hoặc theo chỉ số tương tác để xác định xu hướng. Một số truy vấn có thể yêu cầu sắp xếp kép, như ưu tiên theo ngày trước rồi mới đến lượt thích trong cùng ngày. Những mẫu này giúp xác định cấu trúc sort key phù hợp trong DynamoDB để đáp ứng nhu cầu sắp xếp.
- Yêu cầu phân trang – Phân tích các truy vấn sử dụng kết hợp TOP/LIMIT với sắp xếp. Ví dụ: truy vấn lấy bài đăng gần nhất với giới hạn số lượng, hoặc bài đăng thịnh hành dựa trên tương tác cho thấy nhu cầu phân trang. Hiểu rõ các mẫu này giúp thiết kế chiến lược phân trang hiệu quả thông qua tính năng Limit và LastEvaluatedKey của DynamoDB.

#### Phân tích Mệnh đề GROUP BY

Việc phân tích các thao tác GROUP BY tiết lộ yêu cầu tổng hợp dữ liệu trong ứng dụng, giúp xác định các chỉ số và bộ đếm cần duy trì trong mô hình DynamoDB:

- Mẫu tổng hợp – Nhận diện các thao tác nhóm và tổng hợp trong truy vấn. Điển hình như đếm số bài đăng theo người dùng, tính tổng lượt thích theo bài đăng, hoặc thống kê chỉ số tương tác theo loại nội dung. Hiểu rõ các mẫu này giúp xác định thuộc tính nào cần được tính toán sẵn và lưu trữ trực tiếp trong items.

```sql
-- Posts count by user
SELECT user_id, COUNT(*) as total_posts
FROM posts
GROUP BY user_id
```

- Tần suất cập nhật – Đánh giá mức độ thay đổi của các giá trị tổng hợp. Chẳng hạn, số lượt thích bài đăng được cập nhật liên tục khi người dùng tương tác, trong khi tổng số bài đăng của người dùng chỉ thay đổi khi họ tạo hoặc xóa nội dung. Những mẫu này giúp đánh giá tần suất ghi dữ liệu và tác động đến việc duy trì các giá trị tính toán trước.
  
#### Phân Tích Lưu lượng Ứng dụng

Thu thập số liệu sử dụng ứng dụng, bao gồm tần suất yêu cầu HTTP mỗi giờ. Kết hợp phân tích mức độ quan trọng nghiệp vụ và chỉ số sử dụng với các mục tiêu sau:

- Ưu tiên xử lý các tình huống đặc biệt, ví dụ:

    + Khi người nổi tiếng đăng bài, hệ thống cần phân phối ngay lập tức đến hàng triệu người theo dõi
    + Các bài đăng lan truyền có thể nhận hàng ngàn tương tác chỉ trong vài phút
  
- Xác định các module trọng yếu cần ưu tiên di chuyển

Cách tiếp cận dựa trên dữ liệu này đảm bảo các phần quan trọng và được sử dụng thường xuyên nhất trong ứng dụng được ưu tiên trong quá trình di chuyển, từ đó tối ưu hóa phân bổ tài nguyên và giảm thiểu gián đoạn đến hoạt động kinh doanh cốt lõi. Đồng thời phân tích kích thước bản ghi trung bình, tỷ lệ đọc/ghi và tốc độ tăng trưởng dự kiến của các bảng nhằm:

- Xác định phương pháp tối ưu để xây dựng quan hệ thực thể, ví dụ lựa chọn giữa chiến lược single-item hoặc phân vùng dọc
- Ước lượng và phân bổ chính xác năng lực đọc/ghi

## Kết luận

Thông qua việc phân tích toàn diện cấu trúc cơ sở dữ liệu hiện tại, các mẫu truy cập và chỉ số sử dụng, bạn có thể xây dựng nền tảng vững chắc cho hành trình di chuyển sang DynamoDB. Hiểu biết này tạo tiền đề cho giai đoạn tiếp theo—thiết kế mô hình dữ liệu DynamoDB sẽ được khám phá trong Phần 2 của loạt bài. Việc tuân theo quy trình có cấu trúc này đảm bảo chiến lược di chuyển của bạn không chỉ đáp ứng nhu cầu hiện tại mà còn sẵn sàng cho các yêu cầu mở rộng trong tương lai.