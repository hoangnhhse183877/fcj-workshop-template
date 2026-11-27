---
title: "Blog 3"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Amazon S3 Vectors Hybrid Search – Tối ưu hệ thống tìm kiếm và RAG trên AWS

| Thông tin | Nội dung |
|---|---|
| Tác giả | Đan Huy (Trưởng nhóm) |
| Ngày đăng | 27/11/2026 |
| Trạng thái | Đã được duyệt và đăng |
| Nền tảng | [AWS Study Group VN](https://www.facebook.com/groups/awsstudygroupfcj) |
| Đường dẫn bài đăng | [Xem bài viết gốc](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347637312667894) |

Bài viết do Đan Huy, trưởng nhóm của em, thực hiện và đăng tải. Bài viết được đưa vào báo cáo như một đóng góp của nhóm; nội dung bên dưới giữ nguyên góc nhìn của tác giả.

## 1. Giới thiệu

Bạn đang làm RAG mà vẫn phải dựng riêng một Vector Database? AWS vừa có một cập nhật khá thú vị có thể khiến chúng ta phải suy nghĩ lại.

Trước giờ khi làm các dự án AI có sử dụng RAG, mình thường thấy mọi người dùng Pinecone, Qdrant, pgvector hoặc OpenSearch để lưu embeddings và thực hiện semantic search.

## 2. Tổng quan Amazon S3 Vectors

Nhưng có một dịch vụ của AWS mà mình thấy khá đáng chú ý gần đây, đó là Amazon S3 Vectors.

Trước đây mình biết Amazon S3 chủ yếu dùng để lưu ảnh, video, tài liệu hoặc dữ liệu dạng JSON. Nhưng từ khi AWS ra mắt S3 Vectors, chúng ta có thể lưu trữ và truy vấn vector embeddings mà không nhất thiết phải tự triển khai một vector database riêng.

Đặc biệt, ngày 16/11/2026, AWS vừa bổ sung tính năng Hybrid Search cho S3 Vectors.

## 3. Hybrid Search, BM25 và RRF

Điều này giải quyết một vấn đề khá thú vị trong các hệ thống tìm kiếm sử dụng AI.

Giả sử người dùng tìm kiếm "iPhone 16 Pro bị hao pin sau khi cập nhật".

Nếu chỉ tìm kiếm theo từ khóa, hệ thống có thể bỏ sót những bình luận như "Máy tụt pin nhanh bất thường sau bản update".

Ngược lại, nếu chỉ dùng Vector Search, hệ thống có thể tìm được những bình luận cùng ngữ nghĩa nhưng đôi khi không ưu tiên chính xác tên sản phẩm hoặc mã lỗi mà người dùng đang tìm.

Hybrid Search kết hợp cả hai phương pháp:

- Vector Search giúp tìm kiếm theo ngữ nghĩa, hiểu được những cách diễn đạt khác nhau nhưng cùng một vấn đề.
- Full-text Search (BM25) giúp ưu tiên những từ khóa cụ thể như tên sản phẩm, mã lỗi hoặc thuật ngữ kỹ thuật.

Sau đó, AWS sử dụng Reciprocal Rank Fusion (RRF) để kết hợp kết quả và trả về danh sách phù hợp nhất.

Điểm hay là cả hai cách tìm kiếm có thể thực hiện trên cùng một S3 Vectors index thông qua API QueryVectors.

## 4. Ứng dụng trong dự án BrandTracker

Mình đang tìm hiểu hướng ứng dụng này vào BrandTracker, một hệ thống tổng hợp và phân tích phản hồi người dùng từ mạng xã hội.

Ví dụ, dữ liệu bài viết và bình luận được thu thập từ Reddit, YouTube rồi đưa qua Amazon SQS để xử lý bất đồng bộ. AWS Lambda sẽ làm sạch dữ liệu, chia nội dung và sử dụng model embeddings trên Amazon Bedrock.

Sau đó Lambda lưu vector cùng metadata vào Amazon S3 Vectors.

Khi người dùng muốn tìm những vấn đề khách hàng đang phàn nàn về một sản phẩm, backend có thể kết hợp semantic search và keyword search để tìm các phản hồi liên quan, kèm liên kết đến bài viết gốc.

Như vậy, thay vì chỉ tìm xem khách hàng có nhắc đến từ khóa nào, hệ thống có thể tìm cả những phản hồi có cùng ý nghĩa dù cách diễn đạt hoàn toàn khác nhau.

## 5. Kiến trúc hệ thống

Mình có vẽ một architecture đơn giản minh họa luồng xử lý bằng SQS, Lambda, Bedrock và S3 Vectors để mọi người dễ hình dung hơn.


![Hình 1. Kiến trúc đề xuất của BrandTracker: nạp dữ liệu qua Amazon SQS và AWS Lambda, tạo embeddings bằng Amazon Bedrock và truy vấn Hybrid Search trên Amazon S3 Vectors.](/images/blogs/blog3/brandtracker-s3-vectors-architecture.png)

*Hình 1. Kiến trúc đề xuất của BrandTracker: nạp dữ liệu qua Amazon SQS và AWS Lambda, tạo embeddings bằng Amazon Bedrock và truy vấn Hybrid Search trên Amazon S3 Vectors.*


Sơ đồ mô tả hướng ứng dụng đang được nghiên cứu, không phải minh chứng cho một hệ thống đã triển khai thành công.

## 6. Tối ưu chi phí

Một điểm nữa khá đáng chú ý là AWS công bố S3 Vectors có thể giảm chi phí lưu trữ và truy vấn vector tới 90% so với một số giải pháp vector chuyên dụng. Tất nhiên đây là mức tối đa theo AWS, còn thực tế phụ thuộc vào lượng dữ liệu, tần suất truy vấn và yêu cầu latency.

## 7. Hạn chế và lưu ý kỹ thuật

S3 Vectors cũng chưa phải giải pháp thay thế hoàn toàn pgvector, Qdrant hay OpenSearch. Với những hệ thống cần truy vấn cực nhanh, tần suất cao hoặc khả năng tìm kiếm phức tạp, các giải pháp chuyên dụng vẫn rất đáng cân nhắc.

Một lưu ý kỹ thuật là Hybrid Search yêu cầu vector index ở chế độ ENHANCED. Nếu dùng S3 Vectors thông qua Bedrock Knowledge Bases, hiện tại tích hợp đó vẫn có giới hạn semantic-only, nên muốn khai thác Hybrid Search mới có thể gọi trực tiếp API QueryVectors.

## 8. Kết luận

Mình thấy đây là một hướng khá đáng thử với những dự án RAG, AI Agent, hệ thống phân tích dữ liệu hoặc semantic search có lượng dữ liệu lớn nhưng không nhất thiết cần duy trì một vector database riêng.

Nếu đang xây dựng một hệ thống AI thực tế, mọi người sẽ chọn S3 Vectors, pgvector hay OpenSearch? Và trong trường hợp nào thì các bạn nghĩ nên dùng từng giải pháp?

## 9. Tài liệu tham khảo

1. [AWS – Amazon S3 Vectors now supports hybrid search (16/11/2026)](https://aws.amazon.com/about-aws/whats-new/2026/11/s3-vectors-hybrid-search/)
2. [InfoWorld – AWS looks to cut storage costs for LLM embeddings with Amazon S3 Vectors](https://www.infoworld.com/article/4023401/aws-looks-to-cut-storage-costs-for-llm-embeddings-with-amazon-s3-vectors.html)
3. [DevelopersIO – I Tried Hybrid Search (Vector + Full-Text) with Amazon S3 Vectors (17/11/2026)](https://dev.classmethod.jp/en/articles/s3-vectors-hybrid-search-try/)

### Tài liệu kỹ thuật bổ sung để đối chiếu

- [Hybrid search with Amazon S3 Vectors](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-hybrid-search.html)
- [Configuring full-text search](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-configuring-full-text-search.html)
- [Using S3 Vectors with Amazon Bedrock Knowledge Bases](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-bedrock-kb.html)

#AWS #AmazonS3 #RAG #VectorSearch #GenerativeAI #CloudComputing #SoftwareEngineering #FCAJ

## Hình ảnh minh chứng bài đăng


![Hình 2. Bài viết của Đan Huy trên AWS Study Group VN, ngày 27/11/2026.](/images/blogs/blog3/blog3-publication-proof.png)

*Hình 2. Bài viết của Đan Huy trên AWS Study Group VN, ngày 27/11/2026.*


Bài viết được xác nhận đã được duyệt và đăng ngày 27/11/2026. [Xem bài đăng gốc](https://www.facebook.com/groups/awsstudygroupfcj/posts/2347637312667894).
