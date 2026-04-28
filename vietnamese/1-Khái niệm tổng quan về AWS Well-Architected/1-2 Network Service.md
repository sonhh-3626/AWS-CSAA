# Khái niệm về AWS Infrastructure

## Region và Availability Zone

AWS sở hữu hạ tầng toàn cầu bao gồm Region và Availability Zone (AZ) phân bổ trên khắp thế giới.

### Region

Region là một lãnh thổ địa lý độc lập, hiện tại (2024) AWS có khoảng **34 Region** trên toàn cầu. Mỗi Region được định danh bằng cả tên địa danh lẫn ký hiệu kỹ thuật, ví dụ:

- Tokyo → `ap-northeast-1`
- Singapore → `ap-southeast-1`
- US East (N. Virginia) → `us-east-1`

> **Lưu ý quan trọng:** Không phải tất cả dịch vụ AWS đều khả dụng ở mọi Region. Đây là yếu tố cần kiểm tra và cân nhắc kỹ khi thiết kế kiến trúc hệ thống.

---

### Availability Zone (AZ)

AZ là tập hợp nhiều data center trong cùng một khu vực địa lý. Hiện AWS có **hơn 100 AZ** trên toàn cầu.

Đặc điểm của AZ:

- **Kết nối tốc độ cao** giữa các AZ trong cùng một Region (thường dưới 1–2ms latency).
- **Độc lập hoàn toàn** về mạng, nguồn điện và vị trí địa lý — khoảng cách giữa các AZ thường là vài chục km, đủ xa để cách ly thảm họa vật lý nhưng đủ gần để duy trì hiệu năng cao.
- Nếu một AZ gặp sự cố (động đất, hỏa hoạn...), hệ thống vẫn có thể chuyển kết nối sang AZ khác.

**Cách đặt tên AZ:** AZ được ký hiệu bằng cách thêm hậu tố chữ cái vào tên Region, ví dụ:

```
ap-northeast-1a
ap-northeast-1b
ap-northeast-1c
```

---

### Thiết kế hệ thống với Multi-AZ và Multi-Region

| Mục tiêu               | Giải pháp        | Mô tả                                                    |
| ---------------------- | ---------------- | -------------------------------------------------------- |
| High Availability (HA) | **Multi-AZ**     | Triển khai hệ thống trên nhiều AZ trong cùng Region      |
| Disaster Recovery (DR) | **Multi-Region** | Khôi phục hệ thống ở Region khác khi xảy ra thảm họa lớn |

- **Multi-AZ** được khuyến nghị cho hầu hết hệ thống production để đảm bảo tính khả dụng cao.
- **Multi-Region** được dùng khi cần xây dựng DR site — ví dụ: hệ thống vẫn hoạt động được dù cả một vùng địa lý gặp thảm họa diện rộng.

Khi sử dụng **AWS Managed Services** (các dịch vụ mà AWS quản lý hạ tầng) kết hợp với Multi-AZ, hệ thống có thể tận dụng tối đa lợi thế của Public Cloud.
![[Region và Availability Zone.png]]
---

## Edge Location

Edge Location là cơ sở hạ tầng toàn cầu được AWS triển khai để phục vụ các dịch vụ như CDN (Content Delivery Network). Hiện tại có **hơn 600 Edge Location** trên toàn cầu — nhiều hơn đáng kể so với số lượng Region.

Người dùng sẽ nhận nội dung từ Edge Location gần nhất về mặt vật lý, từ đó:

- Giảm khoảng cách vật lý của network.
- Truyền thông với **độ trễ thấp**.
- Tạo ra hệ thống phân phát nội dung hiệu quả.

Các dịch vụ sử dụng Edge Location bao gồm: **Amazon CloudFront**, **Amazon Route 53**, **AWS Shield**, **Lambda@Edge**.

---

## Phân loại dịch vụ AWS theo phạm vi

AWS phân loại dịch vụ theo phạm vi hoạt động như sau:

### 1. Global Service

Dịch vụ chung, không gắn với Region cụ thể nào. Ví dụ: **AWS IAM** (Identity and Access Management), **Amazon CloudFront**, **Amazon Route 53**

> Do hoạt động ở mức global, các dịch vụ này không cần thiết kế DR riêng biệt khi xây dựng hệ thống khôi phục sau thảm họa.

### 2. Region Service

Dịch vụ chỉ hoạt động trong một Region nhất định. Ví dụ: **Amazon VPC** (Virtual Private Cloud), **Amazon DynamoDB**, **AWS Lambda**
### 3. Availability Zone Service

Dịch vụ chỉ hoạt động trong một AZ cụ thể. Ví dụ: **Subnet của VPC**, **Amazon EC2** (Elastic Compute Cloud), **Amazon RDS** (Relational Database Service)

> Các dịch vụ AZ như EC2 hay RDS chỉ vận hành trong một AZ xác định. Vì vậy khi thiết kế DR site, cần cân nhắc kiến trúc **Multi-Region** hoặc **Multi-AZ** để đảm bảo tính liên tục của hệ thống.
![[AWS infrastructure.png]]
---

## Tóm tắt

```
Global Services      →  IAM, CloudFront, Route 53       (không cần DR riêng)
Region Services      →  VPC, DynamoDB, Lambda            (hoạt động trong 1 Region)
AZ Services          →  Subnet, EC2, RDS                 (hoạt động trong 1 AZ)
Edge Location        →  CloudFront CDN, Route 53 DNS     (hơn 600 điểm toàn cầu)
```

> **Lưu ý khi thi:** Các khái niệm về location là nền tảng để hiểu nhiều nội dung quan trọng, đặc biệt là:
> - **Chương 3** — Thiết kế hệ thống có tính đàn hồi (Resilient Architecture)
> - **Chương 4** — Thiết kế hệ thống có hiệu năng cao (High-Performing Architecture)
>
> Khi gặp câu hỏi về **High Availability** → nghĩ đến **Multi-AZ**.  
> Khi gặp câu hỏi về **Disaster Recovery** → nghĩ đến **Multi-Region**.

![[AWS Infrastructture 2.png]]
## Câu hỏi ôn tập
![[Quiz Region and AZ.png]]