# AWS Well-Architected Framework

## Bối cảnh & Khái niệm

- AWS bắt đầu dịch vụ Cloud vào năm **2006** với 2 dịch vụ đầu tiên:
  - **EC2** (Amazon Elastic Compute Cloud) — máy chủ ảo trên cloud
  - **S3** (Simple Storage Service) — lưu trữ đối tượng
- Hiện nay có hơn **200 dịch vụ** và vẫn tiếp tục tăng theo nhu cầu người dùng

**AWS Well-Architected Framework** là tập hợp best practice được AWS đúc kết qua nhiều năm vận hành cloud, giúp kiến trúc sư và kỹ sư xây dựng hệ thống đúng chuẩn ngay từ đầu.

> Framework này hoạt động như một **checklist kiến trúc** — giúp xác nhận xem hệ thống có tuân theo nguyên tắc thiết kế và best practice hay không, dựa trên 6 trụ cột chính.

---

## 6 Trụ Cột Chính

### 1. Tính ưu tú vận hành (Operational Excellence)

**Mục tiêu:** Đảm bảo hệ thống liên tục cung cấp giá trị cho business, đồng thời không ngừng cải thiện quy trình vận hành.

Các chủ đề chính:
- **Tự động hóa thay đổi**: dùng IaC (CloudFormation, CDK) để giảm lỗi thủ công
- **Giám sát chủ động**: CloudWatch, X-Ray để phát hiện sự cố trước khi user báo
- **Runbook / Playbook**: chuẩn bị sẵn kịch bản xử lý sự cố
- **Cải tiến liên tục**: review định kỳ, không dừng lại sau khi deploy

**Từ khóa đề thi:** `quy trình vận hành`, `CI/CD`, `giám sát liên tục`, `cải tiến quy trình`

---

### 2. Bảo mật (Security)

**Mục tiêu:** Bảo vệ dữ liệu, hệ thống và tài sản thông qua quản lý quyền truy cập chặt chẽ, mã hóa và phát hiện sự cố bảo mật kịp thời.

Các chủ đề chính:
- **Least privilege**: chỉ cấp đúng quyền cần thiết (IAM Roles, Policies)
- **Mã hóa dữ liệu**: at-rest (S3, RDS) và in-transit (TLS/HTTPS) — luôn luôn
- **Phát hiện & phản ứng**: GuardDuty, Security Hub, CloudTrail để audit
- **Quản lý credentials**: không nhúng vào code — dùng Secrets Manager hoặc IAM Role
- **Đảm bảo toàn vẹn dữ liệu**: kiểm soát ai được đọc/ghi/xóa

**Từ khóa đề thi:** `nhất quán/toàn vẹn dữ liệu`, `phân quyền`, `audit log`, `mã hóa`

---

### 3. Độ tin cậy (Reliability)

**Mục tiêu:** Khả năng hệ thống thực hiện đúng chức năng, chịu được lỗi, và **tự phục hồi** khi có sự cố.

> ⚠️ Hay bị nhầm với Operational Excellence. Điểm phân biệt: Reliability tập trung vào **phục hồi sau sự cố**, còn Operational Excellence tập trung vào **cải thiện quy trình vận hành**.

Các chủ đề chính:
- **Multi-AZ deployment**: triển khai ở nhiều Availability Zone để tránh single point of failure
- **Auto Scaling**: tự động thêm/bớt tài nguyên theo tải, ngăn hết capacity
- **Backup & Disaster Recovery**: định nghĩa rõ RTO (Recovery Time Objective) và RPO (Recovery Point Objective)
- **Circuit Breaker pattern**: cô lập thành phần lỗi, không để lây sang toàn hệ thống

**Từ khóa đề thi:** `phục hồi nhanh`, `khả dụng cao`, `chướng ngại vật`, `fault tolerance`, `high availability`

---

### 4. Hiệu suất (Performance Efficiency)

**Mục tiêu:** Sử dụng tài nguyên tính toán một cách hiệu quả để đáp ứng yêu cầu hệ thống, và duy trì hiệu quả đó khi yêu cầu thay đổi theo thời gian.

Các chủ đề chính:
- **Chọn đúng loại instance**: compute-optimized (C-series) vs memory-optimized (R-series) vs GPU
- **Serverless khi phù hợp**: Lambda, Fargate giúp tránh over-provisioning
- **Caching chiến lược**: CloudFront (CDN), ElastiCache (Redis/Memcached) để giảm latency
- **Benchmark định kỳ**: đo lường performance thực tế, không giả định

**Từ khóa đề thi:** `loại tài nguyên phù hợp`, `thay đổi yêu cầu`, `latency thấp`, `scaling`

---

### 5. Tối ưu chi phí (Cost Optimization)

**Mục tiêu:** Tránh chi phí không cần thiết bằng cách hiểu rõ luồng tiền, chọn đúng pricing model và co dãn tài nguyên theo nhu cầu thực tế.

Các chủ đề chính:
- **Right-sizing**: đừng dùng instance lớn hơn cần — dùng Cost Explorer để phân tích
- **Reserved Instances / Savings Plans**: cam kết 1–3 năm để tiết kiệm 30–70% so với On-Demand
- **Spot Instances**: tiết kiệm tới 90% cho workload không cần chạy liên tục (batch, CI)
- **Xóa tài nguyên không dùng**: snapshot cũ, EIP chưa gắn, idle EC2 — dùng Cost Anomaly Detection

**Từ khóa đề thi:** `tiết kiệm chi phí`, `loại bỏ chi phí không cần thiết`, `định giá phù hợp`, `right-sizing`

---

### 6. Tính bền vững (Sustainability)

**Mục tiêu:** Giảm tác động môi trường của workload cloud, chủ yếu thông qua tối ưu hóa năng lượng tiêu thụ và giảm lượng khí carbon thải ra.

> 📌 Trụ cột mới nhất, được AWS bổ sung vào năm **2021**.

Các chủ đề chính:
- **Tối ưu utilization**: tránh idle resources — tài nguyên nhàn rỗi vẫn tiêu điện
- **Dùng managed services**: AWS tối ưu hóa infrastructure tốt hơn tự quản lý
- **Chọn region có năng lượng tái tạo**: dùng AWS Carbon Footprint Tool để đo lường
- **Giảm data transfer không cần thiết**: ít network traffic = ít năng lượng tiêu thụ

**Từ khóa đề thi:** `năng lượng`, `môi trường`, `carbon footprint`, `tác động địa cầu`

---

## Cách phân biệt các trụ cột dễ nhầm

| Từ khóa trong câu hỏi | Trụ cột |
|---|---|
| Phục hồi sau sự cố, fault tolerance, high availability | **Reliability** |
| Cải tiến quy trình vận hành, giám sát, CI/CD | **Operational Excellence** |
| Phân quyền, mã hóa, toàn vẹn dữ liệu | **Security** |
| Chọn loại instance, latency, scaling | **Performance Efficiency** |
| Tiết kiệm tiền, loại bỏ chi phí lãng phí | **Cost Optimization** |
| Năng lượng, carbon, môi trường | **Sustainability** |

---

## Câu hỏi luyện tập

**Câu 1:** Ở một công ty nọ muốn thiết kế kiến trúc cloud khởi tạo chính xác, liên tục ứng dụng sau release và thực hiện việc vận hành có khả năng phục hồi nhanh chóng khi có chướng ngại xảy ra. Việc này ám chỉ gì trong AWS Well-Architected Framework?

- A. Performance Efficiency
- B. Cost Optimization
- C. Operational Excellence
- D. Security

**Giải thích:** Câu hỏi đề cập đến 3 yếu tố: (1) thiết kế chính xác ban đầu, (2) vận hành liên tục sau release, (3) phục hồi nhanh khi có chướng ngại. Tất cả đều thuộc phạm vi **Operational Excellence** — trụ cột tập trung vào việc xây dựng quy trình vận hành tốt và khả năng phản ứng nhanh với sự cố trong quá trình vận hành.

> Lưu ý: Nếu câu hỏi chỉ nhắc đến "phục hồi sau thảm họa" hoặc "high availability" thì đáp án sẽ là **Reliability**. Sự khác biệt nằm ở ngữ cảnh toàn bộ vòng đời vận hành.

---

## Tài liệu tham khảo

1. [AWS Well-Architected Framework (Official)](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
2. [AWS Well-Architected Tool](https://aws.amazon.com/well-architected-tool/)
3. AWS Whitepapers — Well-Architected Framework
4. ![[AWS Well-Architected Framework.png]]
