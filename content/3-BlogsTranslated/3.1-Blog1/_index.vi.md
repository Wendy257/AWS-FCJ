---
title: "Blog 1"
 
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Zupee triển khai Amazon Neptune để phát hiện các bất thường trong giao dịch Ví (Wallet) theo thời gian thực

*Tác giả: Aman Kumar Bahl, Leader of Data Engineering, và Apoorv Mathur, Lead Data Engineer tại Zupee, phối hợp thực hiện cùng AWS*

Zupee là một nền tảng trò chơi kỹ năng hàng đầu, cung cấp các trò chơi giải trí và board game, đồng thời là một trong những nền tảng chơi game kiếm tiền thực tăng trưởng nhanh nhất tại Ấn Độ. Người dùng có thể chơi nhiều trò chơi trực tuyến dựa trên kỹ năng và giành giải thưởng.
Zupee cung cấp cả hai định dạng trò chơi: miễn phí và trả phí. Đối với các game trả phí, người dùng phải nạp tiền vào ví trên ứng dụng của họ. Zupee vận hành các chương trình khuyến khích người dùng nhằm giúp họ nhận thưởng, mời bạn bè hoặc tăng mức độ tương tác. Để duy trì tính toàn vẹn của các chương trình khuyến khích, họ muốn hiểu hành vi người dùng trên nền tảng và ngăn chặn những người dùng có biểu hiện giao dịch đáng ngờ.
Trong bài viết này, chúng tôi sẽ trình bày cách Zupee tích hợp  Amazon Neptune Database để phát hiện bất thường theo thời gian thực đối với các giao dịch ví, bằng cách xây dựng một hệ thống truy vết các mối quan hệ phức tạp giữa người dùng, thiết bị và siêu dữ liệu giao dịch ví.
Amazon Neptune là một dịch vụ cơ sở dữ liệu đồ thị được quản lý hoàn toàn trên nền tảng đám mây, giúp đơn giản hóa quá trình phát triển và vận hành các ứng dụng đồ thị để xử lý những tập dữ liệu có mức độ kết nối rất cao. Neptune được tối ưu hóa để lưu trữ hàng tỷ mối quan hệ và truy vấn đồ thị với độ trễ chỉ tính bằng mili giây, đồng thời hỗ trợ các ngôn ngữ truy vấn đồ thị phổ biến như Apache TinkerPop Gremlin, the W3C’s SPARQL và openCypher. Neptune cung cấp sẵn các tính năng bảo mật, sao lưu liên tục, điện toán không máy chủ (serverless) và khả năng tích hợp với các dịch vụ AWS khác. Neptune hỗ trợ nâng cấp tại chỗ các cụm (cluster) và phiên bản cơ sở dữ liệu. Amazon Neptune Analytics là một công cụ phân tích hiệu năng cao, được thiết kế để trích xuất thông tin chi tiết và phát hiện các mẫu hình trong khối lượng lớn dữ liệu đồ thị được lưu trữ trong Amazon Simple Storage Service (Amazon S3) hoặc  Amazon Neptune Database.


---

## Tổng quan về Giải pháp

Ban đầu, Zupee phát triển giải pháp của mình với một thiết kế cơ bản, sử dụng cơ sở dữ liệu quan hệ để lưu trữ dữ liệu. Cách tiếp cận này giúp xác định các người dùng có kết nối dựa trên các thuộc tính khác nhau, từ đó cho phép xác định giao dịch của người dùng là hợp pháp hay không. Các hạn chế của hệ thống bắt đầu lộ rõ khi xử lý hàng triệu giao dịch trên một cơ sở người dùng ngày càng mở rộng với nhiều thuộc tính tương quan, đòi hỏi một giải pháp mạnh mẽ hơn. Bản chất vốn có của dữ liệu là các kết nối liên quan - nơi người dùng có thể được liên kết thông qua nhiều thuộc tính và mối quan hệ phức tạp - khiến cho việc truy vấn và phân tích các kết nối này ngày càng khó khăn khi sử dụng các cấu trúc cơ sở dữ liệu quan hệ truyền thống. Nhu cầu duyệt và khám phá các mối quan hệ phức tạp này trong thời gian thực đã dẫn Zupee đến việc xem xét một phương pháp tiếp cận dựa trên đồ thị, vốn được thiết kế để xử lý dữ liệu có kết nối cao và các truy vấn quan hệ phức tạp một cách hiệu quả hơn.

Cơ sở dữ liệu đồ thị vượt trội trong việc quản lý các thực thể đa dạng với các mối quan hệ phức tạp và đan xen. Không giống như các cơ sở dữ liệu quan hệ truyền thống đòi hỏi phải thực hiện các phép kết hợp (join) bảng ở nhiều cấp độ phức tạp để truy vấn dữ liệu xuyên qua nhiều kết nối, cơ sở dữ liệu đồ thị cho phép duyệt qua các mối quan hệ này một cách hiệu quả mà không cần các thao tác kết hợp được định nghĩa trước. Khả năng này cho phép khám phá dữ liệu thông qua nhiều bước nhảy và biến đổi với rất ít hạn chế. Kết quả là, cơ sở dữ liệu đồ thị có thể thực thi các truy vấn phức tạp xuyên qua vô số điểm dữ liệu được kết nối và trả về kết quả chỉ trong vài mili giây. Lợi thế về hiệu suất này đặc biệt giá trị khi xử lý các tập dữ liệu có kết nối cao, nơi các mối quan hệ giữa các thực thể rất nhiều và đường đi truy vấn tối ưu có thể không được biết trước. Bằng cách biểu diễn dữ liệu như một mạng lưới các nút (node) và cạnh(edges), cơ sở dữ liệu đồ thị mang lại một phương pháp trực quan và linh hoạt hơn để lưu trữ và truy vấn thông tin được kết nối, khiến chúng trở nên lý tưởng cho các tình huống mà phân tích mối quan hệ là yếu tố then chốt.
Cơ sở dữ liệu đồ thị vượt trội trong việc quản lý và truy cập dữ liệu có tính kết nối thông qua thiết kế chuyên biệt của chúng. Chúng lưu trữ thông tin dưới dạng các nodes (đại diện cho các thực thể) và các edges (đại diện cho các mối quan hệ), cho phép khám phá hiệu quả mạng lưới kết nối phức tạp.


---

### Xây dựng trên Neptune

Zupee lưu trữ các đặc trưng mô hình cần thiết trong Neptune và sử dụng những đặc trưng này để tạo ra các cụm người dùng cũng như giám sát các giao dịch ví. Zupee bắt đầu bằng việc xử lý hơn 1 triệu giao dịch ví mỗi ngày trong thời gian thực. Vì lý do bảo mật, chúng tôi chỉ sử dụng các tên giữ chỗ (placeholder) cho các thuộc tính, nhưng bạn có thể hình dung rằng có một số thuộc tính thuộc về người dùng thực hiện giao dịch trên nền tảng Zupee. Các giao dịch có chung thuộc tính và mẫu hành vi tương tự với các giao dịch đáng ngờ đã biết sẽ được đánh dấu là bất thường. Điều này tạo thành nền tảng của hệ thống toàn vẹn của Zupee, được thiết kế để bảo vệ toàn bộ người dùng và các chương trình khuyến mãi.

Hình ảnh dưới đây mô tả mô hình dữ liệu đồ thị mà Zupee sử dụng trong Cơ sở dữ liệu Đồ thị Neptune. Nó mô hình hóa các mối quan hệ giữa người dùng, thiết bị, giao dịch và công cụ thanh toán.


![pic1](/images/zupee1.png)

Node User là trung tâm:

Kết nối với **Device_Token** để thể hiện việc đăng nhập từ thiết bị.

Kết nối với **Transaction** để thể hiện việc tạo và chuyển tiền giao dịch.

Kết nối với **Payment_Instrument** để thể hiện các phương thức thanh toán.

Kết nối với **User_Profile** để thể hiện hồ sơ người dùng.

Cấu trúc đồ thị linh hoạt này cho phép quản lý và truy vấn dữ liệu hiệu quả xuyên suốt các thực thể khác nhau và các kết nối giữa chúng trong nền tảng của Zupee.

Zupee đã tìm kiếm một cơ sở dữ liệu dựa trên đồ thị và xây dựng một nguyên mẫu thử nghiệm nhỏ (proof of concept). Kết quả thu được rất khả quan về mặt thông tin mà Zupee có thể rút ra từ mô hình đồ thị. Bằng cách sử dụng mô hình này, họ ngay lập tức phát hiện ra các mối liên kết giữa người dùng mà trước đây họ không hề hay biết.

Sơ đồ sau đây minh họa cấu trúc của một cơ sở dữ liệu đồ thị, trong đó các nodes(hình tròn) tương ứng với các thực thể dữ liệu (vertices), và các edges (đường thẳng) mô tả các mối quan hệ hoặc kết nối giữa các thực thể đó.

![pic2](/images/zupee2.png)

Các nodes màu đỏ tượng trưng cho các bản ghi hoặc tài liệu dữ liệu riêng lẻ, trong khi các nodes màu xanh lam đại diện cho các chế độ xem hoặc chỉ mục đã được hợp nhất, có chức năng liên kết các dữ liệu có liên quan. Các cụm sắp xếp theo dạng tỏa tròn cho thấy sự phân bố dữ liệu được tổ chức xung quanh các loại quan hệ hoặc lĩnh vực cụ thể. Cách biểu diễn trực quan này giúp chúng ta dễ dàng nắm bắt được tính kết nối vốn có và các mối liên hệ phức tạp trong một cơ sở dữ liệu đồ thị - vốn là loại hình nổi trội trong việc lưu trữ và truy vấn hiệu quả các cấu trúc dữ liệu có độ liên kết cao so với các cơ sở dữ liệu dạng bảng truyền thống.

Zupee đã sử dụng Union Find algorithm để tạo ra một cách hiệu quả các đồ thị con (groups) riêng biệt gồm các thực thể trên nền tảng của họ, dựa trên các mối quan hệ hoặc liên kết giữa chúng. Cách tiếp cận này giúp họ xử lý các liên kết giả (pseudo associations) và sử dụng kết hợp nhiều mô hình dữ liệu để nhận dạng chúng mà không cần phải mô hình hóa một cách tường minh từng quan hệ đơn lẻ trong một cơ sở dữ liệu đồ thị như Neptune.

Bằng cách sử dụng phương pháp này, Zupee đã có thể phát triển một dịch vụ có khả năng nhóm gộp các mối liên kết lại với nhau và tìm kiếm các nhóm con thực thể rời rạc, cho phép họ phát hiện ra những mối liên hệ phức tạp và chưa từng được biết đến trước đây.

Kiến trúc giải pháp - được minh họa trong hình sau - triển khai việc phát hiện giao dịch gian lận trong thời gian thực.


![pic3](/images/zupee3.png)

Các giao dịch của người dùng được xử lý thông qua Amazon API Gateway, sau đó chuyển tiếp các giao dịch này đến AWS Lambda để xử lý. Dữ liệu giao dịch, bao gồm các thuộc tính như Device_Token) và Payment_Instrument, được lưu trữ trong Neptune. AWS Global Accelerator được sử dụng để tối ưu hóa hiệu suất mạng, và Network Load Balancer phân phối khối lượng công việc trên nhóm các máy chủ game, nơi hiển thị trò chơi.

Cấu trúc đồ thị của Neptune cho phép Zupee:

Xác định các mẫu đáng ngờ bằng cách phân tích mối quan hệ giữa hồ sơ người dùng, công cụ thanh toán và mã thiết bị.
Phát hiện tài khoản trùng lặp bằng cách tìm các thành phần được kết nối trong đồ thị giao dịch người dùng.
Khám phá các công cụ thanh toán được chia sẻ  (như UPI/VPA IDs) trên nhiều tài khoản.
Tính toán giá trị khuyến mãi phù hợp dựa trên mức độ xác thực của người dùng.

Đối với các hình thức chơi game trả phí, khi người dùng nạp tiền vào ví, hệ thống sẽ truy vấn Neptune để lấy về đồ thị con liên quan cho thấy các mối liên kết của người dùng. Điều này giúp xác định xem người dùng đó có hợp lệ hay có khả năng đang sử dụng nhiều tài khoản. Dựa trên phân tích này:

   * Người dùng hợp lệ không có kết nối đáng ngờ sẽ nhận đầy đủ lợi ích khuyến mãi.
   
   * Các tài khoản được phát hiện có bất thường sẽ nhận được khuyến mãi bị điều chỉnh hoặc không được nhận.
   
   * Các công cụ thanh toán được chia sẻ giữa các tài khoản sẽ kích hoạt việc giảm số tiền khuyến mãi.

Cách tiếp cận có hệ thống này giúp Zupee tối ưu hóa chi phí bằng cách đảm bảo rằng các ưu đãi chủ yếu được trao cho người dùng chính chủ, đồng thời bảo vệ khỏi các hành vi lạm dụng tiềm ẩn thông qua tài khoản trùng lặp hoặc phương thức thanh toán dùng chung.


---

## Tóm tắt

Bằng cách sử dụng Amazon Neptune, Zupee đã đạt được thời gian phản hồi ấn tượng dưới 50 mili giây trong điều kiện tối ưu, hỗ trợ phát hiện bất thường trong thời gian thực và cho phép nhóm nhanh chóng triển khai các biện pháp điều chỉnh việc phân phối ưu đãi cho người dùng hợp pháp. Hiện tại, Zupee quản lý một mạng lưới dữ liệu được kết nối rộng lớn bao gồm hơn 5 triệu nodes và edges. Công ty đã phát triển khả năng tìm kiếm động các thực thể và xác định các nodes được kết nối trong thời gian thực.


---

### Tác giả

**Aman Kumar Bahl** là Leader of Data Engineering tại Zupee. Aman được giao nhiệm vụ dẫn dắt nền tảng công nghệ làm động lực cho hệ sinh thái dữ liệu của tổ chức. Anh thể hiện một cách tiếp cận toàn diện trong việc xây dựng các ứng dụng chuyên sâu về dữ liệu nhằm thúc đẩy khả năng mở rộng kinh doanh.

**Apoorv Mathur** là Lead of Data Engineering tại Zupee, chịu trách nhiệm thiết kế và kiến trúc các hệ thống dữ liệu của tổ chức. Anh thúc đẩy việc áp dụng các công nghệ mới, cho phép phân tích thời gian thực và nâng cấp cơ sở hạ tầng dữ liệu.

**Ajeet Dubey** là một Solution Architect kỳ cựu tại AWS, với hơn 13 năm kinh nghiệm chuyên môn trong việc thiết kế và triển khai các giải pháp cloud có tính mở rộng và khả năng chịu lỗi cao. Chuyên môn của ông tập trung vào việc phát triển các giải pháp machine learning và generative AI, được tối ưu hóa cho môi trường cloud.
