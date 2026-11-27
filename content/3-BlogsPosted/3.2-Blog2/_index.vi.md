---
title: "Blog 2"
date: 2026-11-27
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS EC2-Other: Tìm hiểu các chi phí tiềm ẩn và cách tối ưu chi phí trên AWS

| Thông tin | Nội dung |
|---|---|
| Tác giả | Khánh Vương (thành viên trong nhóm) |
| Ngày gửi bài | 27/11/2026 |
| Ngày đăng | 27/11/2026 |
| Trạng thái | Đã được duyệt và đăng (theo thông tin cung cấp) |
| Nền tảng | [AWS Study Group VN](https://www.facebook.com/groups/awsstudygroupfcj) |
| Đường dẫn bài đăng | [Xem bài viết gốc](https://www.facebook.com/groups/awsstudygroupfcj/posts/2351052395659719/) |

Bài viết do Khánh Vương, thành viên trong nhóm của em, thực hiện và đăng tải. Bài viết được đưa vào báo cáo như một đóng góp của nhóm. Nội dung bên dưới giữ nguyên góc nhìn của tác giả bài viết.

## 1. Giới thiệu

Khi sử dụng Amazon EC2, nhiều người thường chỉ quan tâm đến chi phí vận hành EC2 Instance. Tuy nhiên, ngay cả khi đã dừng máy chủ, các tài nguyên và dịch vụ liên quan vẫn có thể tiếp tục phát sinh chi phí.

Trong quá trình kiểm tra AWS Billing and Cost Management, mình nhận thấy chi phí tháng 11 đang ở mức $4.13 và dự kiến đạt $4.77 vào cuối tháng (Figure 1).

Khi phân tích chi phí theo từng dịch vụ, toàn bộ chi phí $4.80 trong tháng 10 đến từ EC2-Other (Figure 2). Đây là một nhóm chi phí dễ bị bỏ qua, đặc biệt đối với những người mới tiếp cận AWS hoặc thường xuyên thực hành các bài lab trên môi trường Cloud.

![Hình 1. Tổng quan AWS Billing and Cost Management, bao gồm Cost Summary và Cost Monitor.](/images/blogs/blog2/aws-billing-cost-summary.png)

*Hình 1. Tổng quan AWS Billing and Cost Management, bao gồm Cost Summary và Cost Monitor.*



![Hình 2. Phân tích chi phí theo dịch vụ: EC2-Other chiếm toàn bộ khoản chi phí 4.80 USD trong tháng 10/2026.](/images/blogs/blog2/ec2-other-cost-breakdown.png)

*Hình 2. Phân tích chi phí theo dịch vụ: EC2-Other chiếm toàn bộ khoản chi phí 4.80 USD trong tháng 10/2026.*

## 2. EC2-Other là gì?

EC2-Other là nhóm chi phí liên quan đến Amazon EC2 nhưng không bao gồm chi phí chạy EC2 Instance trực tiếp.

Một số nguyên nhân phổ biến gây phát sinh chi phí EC2-Other:

- EBS Volume: Chi phí lưu trữ ổ đĩa, kể cả khi EC2 Instance đã dừng hoạt động.
- EBS Snapshot: Chi phí lưu trữ các bản sao lưu ổ đĩa.
- NAT Gateway: Chi phí duy trì dịch vụ và xử lý dữ liệu mạng.
- Data Transfer: Chi phí truyền dữ liệu trong một số trường hợp.
- EBS IOPS và Throughput: Chi phí hiệu năng lưu trữ được cấp phát bổ sung.

Ngoài ra, Public IPv4 và Elastic IP cũng là những tài nguyên cần được kiểm tra khi tối ưu chi phí AWS, mặc dù chúng có thể được phân loại riêng trong Amazon VPC.

## 3. Cách kiểm tra chi phí EC2-Other

AWS Cost Explorer cho phép người dùng phân tích chi tiết chi phí thông qua các bước sau:

- Truy cập AWS Billing and Cost Management → Cost Explorer.
- Chọn khoảng thời gian cần phân tích.
- Trong Filters → Service, chọn EC2-Other.
- Trong Group by → Dimension, chọn Usage type.
- Kiểm tra từng loại chi phí để xác định nguồn phát sinh.

Trong trường hợp của mình, mặc dù đã xác định được EC2-Other là nhóm chi phí chính trong tháng 10, nguyên nhân cụ thể của khoản $4.80 vẫn chưa được xác minh.

## 4. AWS Free Tier và AWS Credits

Một điểm quan trọng cần lưu ý là AWS Free Tier không đồng nghĩa với việc mọi tài nguyên đều không phát sinh chi phí.

Các tài nguyên vẫn có thể tạo ra chi phí sử dụng và tiêu hao AWS Credits.

Trong trường hợp của mình (Figure 3):

- AWS Credits ban đầu: $100.00.
- AWS Credits còn lại theo ước tính: $89.48.
- Tổng Credits đã và dự kiến sử dụng: $10.52.
- Tỷ lệ Credits đã và dự kiến sử dụng: 10.52%.
- Amazon EC2 chiếm phần lớn lượng Credits đã sử dụng.

Điều này cho thấy việc theo dõi và quản lý chi phí là cần thiết ngay cả khi chưa phải thanh toán bằng tiền thật.

![Hình 3. Thông tin AWS Free Tier Credit và lịch sử sử dụng credit.](/images/blogs/blog2/aws-free-tier-credits.png)

*Hình 3. Thông tin AWS Free Tier Credit và lịch sử sử dụng credit.*

## 5. Các phương pháp tối ưu chi phí AWS

Để hạn chế những khoản chi phí không mong muốn, chúng ta có thể áp dụng một số phương pháp sau:

- Kiểm tra và xóa EBS Volume không còn sử dụng sau khi xác nhận dữ liệu không còn cần thiết.
- Xóa EBS Snapshot không còn nhu cầu sử dụng.
- Giải phóng Elastic IP không sử dụng.
- Kiểm tra và xóa NAT Gateway không còn phục vụ workload.
- Rà soát tài nguyên trên tất cả AWS Regions.
- Thiết lập AWS Budgets để nhận cảnh báo khi chi phí vượt ngưỡng mong muốn.
- Thường xuyên theo dõi AWS Cost Explorer và số dư AWS Credits.

Đặc biệt, đối với những người đang học AWS hoặc thực hiện các dự án cá nhân, việc kiểm tra và dọn dẹp tài nguyên sau mỗi bài lab nên trở thành một thói quen.

## 6. Kết luận

Việc dừng EC2 Instance không đồng nghĩa với việc tất cả chi phí AWS sẽ ngừng phát sinh.

Việc hiểu rõ EC2-Other giúp người dùng chủ động kiểm soát chi phí, hạn chế lãng phí tài nguyên và sử dụng AWS Credits hiệu quả hơn.

Đây cũng là một cách áp dụng thực tế Cost Optimization – một trong sáu trụ cột của AWS Well-Architected Framework.

Quản lý hạ tầng Cloud hiệu quả không chỉ là triển khai và vận hành hệ thống thành công mà còn là khả năng kiểm soát và tối ưu chi phí trong suốt vòng đời của tài nguyên.

## 7. Tài liệu tham khảo

1. [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
2. [AWS Cost Explorer – Tips and Tricks](https://aws.amazon.com/blogs/aws-cloud-financial-management/tips-and-tricks-for-exploring-your-data-in-aws-cost-explorer-part-2/)
3. [Amazon EBS Pricing](https://aws.amazon.com/ebs/pricing/)
4. [Amazon VPC Pricing](https://aws.amazon.com/vpc/pricing/)
5. [AWS Free Tier](https://aws.amazon.com/free/)
6. [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/)
7. [AWS Well-Architected Framework – Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html)

#AWS #AmazonEC2 #EC2Other #AWSFreeTier #CostOptimization #FinOps #CloudComputing #DevOps #AWSCommunity #AWSStudyGroup #FirstCloudAIJourney

## Hình ảnh minh chứng bài đăng

![Hình 4. Bài viết AWS EC2-Other trên cộng đồng AWS Study Group VN.](/images/blogs/blog2/blog2-publication-proof.png)

*Hình 4. Bài viết AWS EC2-Other trên cộng đồng AWS Study Group VN.*

Bài viết đã được duyệt theo thông tin cung cấp. [Xem bài đăng gốc](https://www.facebook.com/groups/awsstudygroupfcj/posts/2351052395659719/).
