---
title: "Blog 1"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Amazon S3 – Tối ưu chi phí, bảo mật và tự động hóa quản lý dữ liệu trên AWS

| Thông tin | Nội dung |
|---|---|
| Tác giả | Nguyễn Hữu Huy Hoàng |
| Ngày gửi bài | 27/11/2026 |
| Ngày đăng | 27/11/2026 |
| Trạng thái | Đã được duyệt và đăng |
| Nền tảng | [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) |
| Đường dẫn bài đăng | [Xem bài viết gốc](https://www.facebook.com/groups/awsstudygroupfcj/posts/2350947399003552/) |
| Hình thức | Nghiên cứu và chia sẻ kiến thức kỹ thuật |

## 1. Giới thiệu

Trong quá trình tìm hiểu về Cloud Computing và các dịch vụ của AWS, Amazon Simple Storage Service (Amazon S3) là một trong những dịch vụ nền tảng có tính ứng dụng cao trong việc xây dựng và vận hành hệ thống trên môi trường đám mây.

Amazon S3 không chỉ cung cấp khả năng lưu trữ dữ liệu mà còn hỗ trợ nhiều tính năng giúp doanh nghiệp quản lý vòng đời dữ liệu, kiểm soát quyền truy cập, phục hồi dữ liệu và tự động hóa các quy trình xử lý.

Trong bài viết này, mình muốn chia sẻ cách khai thác những tính năng của Amazon S3 nhằm giải quyết ba vấn đề quan trọng trong quá trình xây dựng hệ thống Cloud: tối ưu chi phí, bảo mật dữ liệu và nâng cao hiệu quả vận hành.

## 2. Tổng quan về Amazon S3

Amazon S3 là dịch vụ lưu trữ đối tượng (Object Storage) có khả năng mở rộng, cho phép người dùng lưu trữ và truy xuất dữ liệu thông qua Internet hoặc các kết nối mạng riêng phù hợp.

Dữ liệu trong Amazon S3 được tổ chức thông qua hai thành phần chính:

- Bucket: Không gian chứa và quản lý các đối tượng.
- Object: Đối tượng dữ liệu bao gồm nội dung, metadata và khóa định danh (Object Key).

Amazon S3 được sử dụng trong nhiều trường hợp thực tế, chẳng hạn như:

- Lưu trữ hình ảnh, video và tài liệu.
- Sao lưu và phục hồi dữ liệu.
- Xây dựng Data Lake phục vụ phân tích dữ liệu.
- Lưu trữ dữ liệu cho các ứng dụng AI và Machine Learning.
- Quản lý nhật ký hoạt động của hệ thống.
- Lưu trữ tài nguyên cho các ứng dụng web.

S3 Standard được thiết kế với độ bền dữ liệu 99.999999999% và tính sẵn sàng 99.99% trong một năm.

Độ bền dữ liệu thể hiện khả năng bảo toàn dữ liệu theo thời gian, trong khi tính sẵn sàng thể hiện khả năng truy cập và sử dụng dịch vụ khi cần thiết. Đây là hai khái niệm khác nhau cần được phân biệt khi thiết kế hệ thống lưu trữ.

## 3. Bài toán quản lý dữ liệu trong thực tế

Giả sử một doanh nghiệp đang phát triển hệ thống thương mại điện tử trên AWS. Hệ thống phát sinh nhiều loại dữ liệu khác nhau mỗi ngày, bao gồm:

- Hình ảnh sản phẩm.
- Hóa đơn của khách hàng.
- Báo cáo doanh thu.
- Nhật ký hoạt động của ứng dụng.
- Dữ liệu sao lưu định kỳ.

Nếu toàn bộ dữ liệu được lưu trữ trong cùng một lớp lưu trữ, không có chính sách quản lý vòng đời và không được phân quyền phù hợp, doanh nghiệp có thể gặp phải những vấn đề như:

- Chi phí lưu trữ tăng theo thời gian.
- Dữ liệu quan trọng bị xóa hoặc ghi đè ngoài ý muốn.
- Nguy cơ truy cập trái phép vào thông tin khách hàng.
- Phát sinh nhiều công việc quản lý dữ liệu thủ công.
- Khó khăn trong việc mở rộng và vận hành hệ thống.

Để giải quyết những vấn đề này, doanh nghiệp có thể kết hợp các tính năng sẵn có của Amazon S3 thay vì xây dựng toàn bộ cơ chế quản lý dữ liệu từ đầu.

## 4. Tối ưu chi phí với S3 Storage Classes

Một trong những yếu tố quan trọng khi sử dụng Amazon S3 là lựa chọn lớp lưu trữ phù hợp với đặc điểm của dữ liệu.

Amazon S3 cung cấp nhiều Storage Class phục vụ các nhu cầu khác nhau:

**S3 Standard**

Phù hợp với dữ liệu được truy cập thường xuyên, chẳng hạn như hình ảnh sản phẩm và tài nguyên của ứng dụng.

**S3 Intelligent-Tiering**

Tự động điều chỉnh tầng truy cập của dữ liệu dựa trên tần suất sử dụng. Phù hợp khi khó dự đoán nhu cầu truy cập trong tương lai.

**S3 Standard-IA (Infrequent Access)**

Phù hợp với dữ liệu ít được truy cập nhưng vẫn cần khả năng truy xuất nhanh khi cần thiết.

**S3 One Zone-IA**

Lưu trữ dữ liệu trong một Availability Zone, phù hợp với những dữ liệu ít được truy cập và có thể tái tạo nếu xảy ra sự cố.

**S3 Glacier Instant Retrieval**

Phù hợp với dữ liệu lưu trữ dài hạn, ít được truy cập nhưng yêu cầu truy xuất với độ trễ thấp.

**S3 Glacier Flexible Retrieval**

Phù hợp với dữ liệu lưu trữ dài hạn không yêu cầu truy xuất ngay lập tức.

**S3 Glacier Deep Archive**

Phù hợp với dữ liệu lưu trữ lâu dài, có tần suất truy cập rất thấp và chấp nhận thời gian khôi phục dài hơn.

**S3 Express One Zone**

Được thiết kế cho các ứng dụng yêu cầu hiệu năng cao và độ trễ truy cập thấp trong một Availability Zone.

Việc lựa chọn Storage Class không nên chỉ dựa trên giá lưu trữ. Người thiết kế hệ thống cũng cần xem xét chi phí truy xuất, số lượng request, thời gian lưu trữ tối thiểu, kích thước đối tượng và yêu cầu về tính sẵn sàng.

![Hình 1. Minh họa phân loại dữ liệu theo tần suất truy cập và lớp lưu trữ S3.](/images/blogs/blog1/s3-storage-classes.jpg)

*Hình 1. Minh họa phân loại dữ liệu theo tần suất truy cập và lớp lưu trữ S3.*

## 5. Tự động hóa quản lý dữ liệu với S3 Lifecycle

Trong hệ thống thương mại điện tử được đề cập ở trên, nhật ký hoạt động của ứng dụng thường được truy cập nhiều trong những ngày đầu để theo dõi hiệu năng và phân tích sự cố.

Sau một khoảng thời gian, nhu cầu truy cập những dữ liệu này giảm dần. Việc tiếp tục lưu trữ toàn bộ nhật ký trong S3 Standard có thể không còn phù hợp về mặt chi phí.

S3 Lifecycle cho phép thiết lập các quy tắc để tự động chuyển đổi lớp lưu trữ hoặc hết hạn đối tượng dựa trên vòng đời của dữ liệu.

Ví dụ, doanh nghiệp có thể xây dựng chính sách dành cho nhật ký ứng dụng như sau:

- Từ ngày 0 đến ngày 29: Lưu trữ trong S3 Standard.
- Sau 30 ngày: Chuyển sang S3 Standard-IA.
- Sau 90 ngày: Chuyển sang S3 Glacier Flexible Retrieval.
- Sau 365 ngày: Áp dụng chính sách hết hạn nếu dữ liệu không còn cần thiết và đáp ứng các yêu cầu lưu giữ.

Quá trình này được Amazon S3 quản lý tự động dựa trên Lifecycle Rule đã cấu hình.

Tuy nhiên, khi triển khai cần lưu ý:

- S3 Standard-IA có thời gian lưu trữ tối thiểu tính phí là 30 ngày.
- S3 Glacier Flexible Retrieval có thời gian lưu trữ tối thiểu tính phí là 90 ngày.
- S3 Glacier Deep Archive có thời gian lưu trữ tối thiểu tính phí là 180 ngày.
- Theo cấu hình mặc định hiện hành, các đối tượng nhỏ hơn 128 KB không được tự động chuyển lớp lưu trữ thông qua Lifecycle Rule, trừ khi có cấu hình phù hợp để thay đổi hành vi này.
- Việc chuyển đổi lớp lưu trữ và truy xuất dữ liệu có thể phát sinh chi phí.

Ngoài ra, Lifecycle Rule có thể áp dụng cho cả những đối tượng hiện có đáp ứng điều kiện của quy tắc. Vì vậy, cần kiểm tra kỹ phạm vi áp dụng trước khi cấu hình hành động hết hạn dữ liệu.

S3 Lifecycle giúp giảm công việc quản lý thủ công và hỗ trợ tối ưu chi phí dài hạn khi chính sách được xây dựng dựa trên đặc điểm sử dụng dữ liệu thực tế.

![Hình 2. Minh họa các hướng chuyển đổi lớp lưu trữ S3 qua Lifecycle.](/images/blogs/blog1/s3-lifecycle-transitions.jpg)

*Hình 2. Minh họa các hướng chuyển đổi lớp lưu trữ S3 qua Lifecycle.*

## 6. Bảo vệ dữ liệu với S3 Versioning

Trong quá trình vận hành hệ thống, việc vô tình ghi đè hoặc xóa nhầm dữ liệu có thể gây ảnh hưởng đến hoạt động của doanh nghiệp.

S3 Versioning cho phép lưu giữ nhiều phiên bản của cùng một đối tượng trong Bucket.

Ví dụ, một tập tin báo cáo doanh thu có tên `monthly-report.pdf` được cập nhật nhiều lần. Khi Versioning được kích hoạt, Amazon S3 có thể giữ lại các phiên bản trước đó thay vì chỉ lưu trữ nội dung mới nhất.

Nếu người dùng vô tình tải lên một báo cáo không chính xác, các phiên bản trước vẫn có thể được truy xuất và khôi phục.

Khi thực hiện thao tác xóa thông thường trên Bucket đã bật Versioning mà không chỉ định Version ID, Amazon S3 tạo một Delete Marker thay vì xóa vĩnh viễn các phiên bản dữ liệu trước đó.

Tuy nhiên, Versioning không bảo vệ dữ liệu tuyệt đối. Người dùng có quyền phù hợp vẫn có thể xóa vĩnh viễn một phiên bản cụ thể.

Ngoài ra, mỗi phiên bản được lưu giữ đều phát sinh chi phí lưu trữ. Doanh nghiệp nên kết hợp S3 Versioning với S3 Lifecycle để quản lý các phiên bản không còn hiện hành (Noncurrent Versions).

Đối với dữ liệu có yêu cầu bảo vệ nghiêm ngặt, có thể cân nhắc bổ sung S3 Object Lock, AWS Backup hoặc các cơ chế sao lưu phù hợp với yêu cầu của hệ thống.

![Hình 3. Thao tác PUT tạo phiên bản mới khi bật S3 Versioning.](/images/blogs/blog1/s3-versioning-put.jpg)

*Hình 3. Thao tác PUT tạo phiên bản mới khi bật S3 Versioning.*



![Hình 4. Thao tác DELETE tạo Delete Marker khi bật S3 Versioning.](/images/blogs/blog1/s3-versioning-delete.jpg)

*Hình 4. Thao tác DELETE tạo Delete Marker khi bật S3 Versioning.*

## 7. Chia sẻ dữ liệu an toàn với S3 Presigned URL

Trong hệ thống thương mại điện tử, khách hàng có thể cần tải xuống hóa đơn hoặc tài liệu liên quan đến đơn hàng của mình.

Việc công khai Bucket để đáp ứng nhu cầu này sẽ tạo ra rủi ro bảo mật không cần thiết.

S3 Presigned URL cung cấp một giải pháp cho phép ứng dụng cấp quyền truy cập có thời hạn đối với một đối tượng cụ thể mà không cần công khai Bucket.

Ví dụ, khi khách hàng yêu cầu tải xuống hóa đơn:

1. Ứng dụng xác thực danh tính khách hàng.
2. Backend kiểm tra quyền truy cập đối với hóa đơn được yêu cầu.
3. Backend tạo Presigned URL có thời hạn 15 phút.
4. Khách hàng sử dụng đường dẫn để tải xuống hóa đơn từ Amazon S3.

Ứng dụng cũng có thể sử dụng Presigned URL để hỗ trợ người dùng tải tập tin trực tiếp lên Amazon S3 mà không cần truyền toàn bộ nội dung qua máy chủ backend.

Cách tiếp cận này giúp giảm tải cho backend và giới hạn quyền truy cập theo từng đối tượng.

Tuy nhiên, Presigned URL cần được bảo vệ như một thông tin cấp quyền truy cập. Bất kỳ ai sở hữu đường dẫn hợp lệ đều có thể sử dụng nó trong phạm vi được cấp.

Một số nguyên tắc nên áp dụng:

- Chỉ cấp quyền cho những thao tác thực sự cần thiết.
- Thiết lập thời hạn hiệu lực ngắn và phù hợp.
- Xác thực người dùng trước khi tạo đường dẫn.
- Không công khai Presigned URL chứa quyền truy cập dữ liệu nhạy cảm.
- Sử dụng HTTPS để bảo vệ dữ liệu trong quá trình truyền tải.

Thời hạn thực tế của Presigned URL cũng phụ thuộc vào thời hạn và trạng thái của thông tin xác thực được sử dụng để tạo đường dẫn.

## 8. Áp dụng Security Best Practices cho Amazon S3

Bảo mật dữ liệu là một phần quan trọng trong quá trình thiết kế kiến trúc trên AWS.

Đối với Amazon S3, có thể áp dụng các cơ chế sau:

**S3 Block Public Access**

Giúp ngăn chặn việc cấp quyền truy cập công khai ngoài ý muốn. Đối với dữ liệu nội bộ và dữ liệu khách hàng, nên duy trì các thiết lập chặn truy cập công khai.

**AWS Identity and Access Management (IAM)**

Sử dụng IAM Policy và S3 Bucket Policy để kiểm soát quyền truy cập theo nguyên tắc Least Privilege, đảm bảo mỗi người dùng hoặc ứng dụng chỉ được cấp những quyền cần thiết.

**Server-Side Encryption**

Amazon S3 cung cấp mã hóa mặc định cho các đối tượng mới bằng khóa do Amazon S3 quản lý (SSE-S3).

Đối với hệ thống có yêu cầu quản lý và kiểm soát khóa mã hóa chặt chẽ hơn, có thể sử dụng SSE-KMS kết hợp với AWS Key Management Service (AWS KMS).

**AWS CloudTrail**

Hỗ trợ ghi nhận các hoạt động API liên quan đến Amazon S3. Đối với những hoạt động truy cập đối tượng như GetObject, PutObject và DeleteObject, cần cấu hình CloudTrail Data Events phù hợp.

**S3 Object Lock**

Hỗ trợ bảo vệ các phiên bản đối tượng khỏi việc bị xóa hoặc thay đổi trong phạm vi chính sách lưu giữ, phù hợp với các hệ thống có yêu cầu tuân thủ và bảo toàn dữ liệu.

Việc kết hợp những cơ chế trên giúp giảm thiểu rủi ro bảo mật và tăng khả năng giám sát trong quá trình vận hành.

## 9. Tự động hóa xử lý dữ liệu với Amazon S3 và AWS Lambda

Amazon S3 có thể tích hợp với các dịch vụ AWS khác thông qua cơ chế S3 Event Notifications.

Giả sử doanh nghiệp cần tự động tạo ảnh thumbnail mỗi khi người quản trị tải lên một hình ảnh sản phẩm mới.

Quy trình có thể được thiết kế như sau:

1. Người quản trị tải hình ảnh lên Input S3 Bucket.
2. Amazon S3 phát sinh sự kiện ObjectCreated.
3. Sự kiện kích hoạt AWS Lambda.
4. Lambda xử lý hình ảnh và tạo thumbnail.
5. Hình ảnh đã xử lý được lưu vào Output S3 Bucket.
6. Ứng dụng sử dụng hình ảnh đã được tối ưu.

Đây là một ví dụ về kiến trúc Event-Driven kết hợp với Serverless Computing.

Doanh nghiệp không cần duy trì máy chủ xử lý hình ảnh hoạt động liên tục mà có thể thực thi chức năng khi sự kiện phát sinh.

Khi thiết kế kiến trúc này, cần lưu ý sử dụng Bucket đầu ra riêng biệt hoặc cấu hình bộ lọc sự kiện phù hợp để tránh việc Lambda liên tục kích hoạt lại chính nó.

Ngoài ra, S3 Event Notifications có thể gửi sự kiện trùng lặp hoặc không theo thứ tự mong muốn. Vì vậy, Lambda nên được thiết kế để xử lý an toàn các sự kiện lặp lại.

Cách tiếp cận tương tự có thể được áp dụng cho:

- Tự động xử lý tập tin CSV.
- Xây dựng quy trình xử lý dữ liệu.
- Chuyển đổi định dạng tập tin.
- Phân tích nhật ký hệ thống.
- Tiền xử lý dữ liệu phục vụ Machine Learning.

![Hình 5. Kiến trúc tạo ảnh thumbnail bằng Amazon S3 và AWS Lambda.](/images/blogs/blog1/s3-lambda-thumbnail.jpg)

*Hình 5. Kiến trúc tạo ảnh thumbnail bằng Amazon S3 và AWS Lambda.*

## 10. Hướng dẫn thực hành cấu hình S3 Lifecycle

Để hiểu rõ hơn về khả năng quản lý vòng đời dữ liệu của Amazon S3, có thể thực hiện một bài thực hành đơn giản trên AWS Management Console.

**Bước 1: Tạo S3 Bucket**

Truy cập Amazon S3 và tạo một General Purpose Bucket với tên duy nhất.

Giữ nguyên thiết lập Block Public Access và kiểm tra cấu hình mã hóa mặc định.

**Bước 2: Tải dữ liệu mẫu**

Tải lên một số tập tin thử nghiệm có kích thước lớn hơn 128 KB với Object Key bắt đầu bằng `logs/`.

Không sử dụng dữ liệu cá nhân hoặc dữ liệu quan trọng trong bài thực hành.

**Bước 3: Tạo Lifecycle Rule**

Truy cập Bucket vừa tạo, chọn tab Management và chọn Create lifecycle rule.

Đặt tên cho quy tắc là `application-logs-lifecycle`.

**Bước 4: Xác định phạm vi áp dụng**

Thiết lập Prefix là `logs/` để quy tắc chỉ áp dụng cho nhóm đối tượng nhật ký thử nghiệm.

**Bước 5: Cấu hình Lifecycle Actions**

Thiết lập các hành động:

- Chuyển sang S3 Standard-IA sau 30 ngày kể từ ngày tạo đối tượng.
- Chuyển sang S3 Glacier Flexible Retrieval sau 90 ngày.
- Áp dụng Expiration sau 365 ngày nếu phù hợp với chính sách lưu giữ dữ liệu.

**Bước 6: Kiểm tra cấu hình**

Xem lại tên quy tắc, phạm vi áp dụng, lớp lưu trữ và các mốc thời gian trước khi lưu.

Nếu Bucket đã bật Versioning, cần cấu hình thêm chính sách quản lý Noncurrent Versions nếu muốn tự động xóa vĩnh viễn các phiên bản cũ.

**Bước 7: Theo dõi kết quả**

Kiểm tra Lifecycle Rule trong tab Management.

Khi đối tượng đủ điều kiện, Amazon S3 sẽ lên lịch thực hiện hành động tương ứng. Quá trình chuyển đổi hoặc hết hạn có thể không diễn ra ngay lập tức.

**Bước 8: Dọn dẹp tài nguyên**

Sau khi kết thúc bài thực hành, vô hiệu hóa hoặc xóa Lifecycle Rule nếu không còn sử dụng.

Xóa dữ liệu thử nghiệm, bao gồm các phiên bản và Delete Marker nếu có, sau đó xóa Bucket để tránh phát sinh chi phí không cần thiết.

## 11. Đánh giá lợi ích và hạn chế

Việc kết hợp các tính năng của Amazon S3 mang lại nhiều lợi ích:

- Tối ưu chi phí dựa trên đặc điểm truy cập dữ liệu.
- Giảm khối lượng công việc quản lý thủ công.
- Hỗ trợ phục hồi dữ liệu khi xảy ra sai sót.
- Kiểm soát quyền truy cập theo nhu cầu của ứng dụng.
- Tích hợp linh hoạt với các dịch vụ AWS.
- Hỗ trợ xây dựng kiến trúc có khả năng mở rộng.

Tuy nhiên, hiệu quả của giải pháp phụ thuộc vào cách thiết kế và cấu hình.

Lifecycle Rule không phù hợp có thể gây mất dữ liệu hoặc phát sinh chi phí ngoài dự kiến. Versioning có thể làm tăng dung lượng lưu trữ. Presigned URL có thể tạo ra rủi ro bảo mật nếu không được kiểm soát đúng cách.

Do đó, cần đánh giá yêu cầu nghiệp vụ, đặc điểm dữ liệu, chi phí vận hành và các tiêu chuẩn bảo mật trước khi áp dụng vào môi trường Production.

## 12. Tổng kết

Qua việc tìm hiểu Amazon S3, mình nhận thấy rằng giá trị của một dịch vụ Cloud không chỉ nằm ở chức năng chính mà còn ở khả năng kết hợp các tính năng để giải quyết những vấn đề thực tế.

Amazon S3 cung cấp nhiều công cụ hỗ trợ xây dựng hệ thống lưu trữ an toàn, linh hoạt và hiệu quả. Việc kết hợp S3 Storage Classes, S3 Lifecycle, S3 Versioning, Presigned URL và các cơ chế bảo mật giúp cải thiện chất lượng kiến trúc và giảm sự phức tạp trong quá trình vận hành.

Những phương pháp này cũng phù hợp với định hướng của AWS Well-Architected Framework, đặc biệt là các trụ cột Security, Reliability, Operational Excellence và Cost Optimization.

Đối với những bạn đang bắt đầu tìm hiểu AWS, Amazon S3 là một dịch vụ đáng để nghiên cứu chuyên sâu, bởi các kiến thức về lưu trữ, phân quyền, quản lý chi phí và tự động hóa có thể được áp dụng trong nhiều mô hình kiến trúc Cloud khác nhau.

Hy vọng bài viết mang lại những thông tin hữu ích cho cộng đồng. Mình rất mong nhận được những góp ý và chia sẻ kinh nghiệm từ mọi người để có thể tiếp tục hoàn thiện kiến thức về AWS.

## Kiến thức và bài học rút ra

Thông qua quá trình nghiên cứu và xây dựng bài viết về Amazon S3, em có cơ hội củng cố và mở rộng kiến thức về các dịch vụ lưu trữ dữ liệu trên nền tảng AWS.

Em hiểu rõ hơn về cách lựa chọn lớp lưu trữ phù hợp, quản lý vòng đời dữ liệu, bảo vệ dữ liệu thông qua S3 Versioning và kiểm soát quyền truy cập theo các nguyên tắc bảo mật.

Bên cạnh đó, việc tìm hiểu khả năng tích hợp giữa Amazon S3 và AWS Lambda giúp em mở rộng kiến thức về kiến trúc Event-Driven và Serverless Computing, cũng như nhận thức được tầm quan trọng của việc tự động hóa trong quá trình vận hành hệ thống.

Quá trình thực hiện bài viết cũng giúp em rèn luyện khả năng tự học, nghiên cứu tài liệu kỹ thuật, tổng hợp kiến thức và trình bày nội dung một cách có hệ thống.

Đây là những kiến thức nền tảng có giá trị, giúp em tiếp tục nghiên cứu và phát triển các dự án liên quan đến AWS và Cloud Computing trong tương lai.

## Hình ảnh minh chứng gửi bài


![Hình 6. Bài viết Amazon S3 – Tối ưu chi phí, bảo mật và tự động hóa quản lý dữ liệu trên AWS được gửi ngày 27/11/2026.](/images/blogs/blog1/blog1-submission-proof.png)

*Hình 6. Bài viết Amazon S3 – Tối ưu chi phí, bảo mật và tự động hóa quản lý dữ liệu trên AWS được gửi ngày 27/11/2026.*


Ảnh chụp minh chứng cho thời điểm gửi bài ban đầu. Người viết xác nhận bài đã được duyệt ngày 27/11/2026. [Xem bài viết đã đăng trên AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/posts/2350947399003552/).

## Tài liệu tham khảo

1. Amazon S3 Overview
https://aws.amazon.com/s3/

2. Amazon S3 Storage Classes
https://aws.amazon.com/s3/storage-classes/

3. Transitioning objects using Amazon S3 Lifecycle
https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html

4. Setting an S3 Lifecycle configuration
https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-set-lifecycle-configuration-intro.html

5. S3 Versioning
https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html

6. Download and upload objects with presigned URLs
https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html

7. Security best practices for Amazon S3
https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html

8. Amazon S3 Event Notifications
https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html

9. AWS Well-Architected Framework
https://aws.amazon.com/architecture/well-architected/

10. Amazon S3 Pricing
https://aws.amazon.com/s3/pricing/

#AWS #AmazonS3 #CloudComputing #CloudSecurity #CostOptimization #AWSLambda #Serverless #AWSStudyGroup #FirstCloudAIJourney
