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

![Region và Availability Zone](images/Region%20và%20Availability%20Zone.png)
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
![AWS infrastructure](images/AWS%20infrastructure.png)
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

<img src="images/AWS Infrastructture 2.png" alt="AWS Infrastructture 2" width="450">

## Câu hỏi ôn tập
<img src="images/Quiz Region and AZ.png" alt="Quiz Region and AZ" width="450">

# Amazon Virtual Private Cloud (VPC)

Trong môi trường AWS mà hàng triệu người dùng trên toàn thế giới cùng sử dụng, **Amazon Virtual Private Cloud (VPC)** là dịch vụ cho phép tạo ra một môi trường mạng riêng biệt, chỉ những người dùng được chỉ định mới có thể truy cập.

Về bản chất, hạ tầng và phần cứng trên AWS được chia sẻ chung giữa tất cả người dùng. Tuy nhiên, bằng cách tạo ra một mạng ảo gọi là VPC, người dùng có thể xây dựng một **private network tách biệt về mặt logic**, đảm bảo tính bảo mật của hệ thống mạng.
Amazon VPC là một **Region Service** — tức là nó được tạo và vận hành trong phạm vi một Region cụ thể trên AWS.

> **Lưu ý khi tạo VPC**: Người dùng có thể tạo VPC tự do, tuy nhiên khi tạo thì có một vài điểm chú ý
> 1. Với VPC của IPv4 cần phải chỉ định CIDR trong phạm vi của địa chỉ IP private là từ 16bit đến 28bit
> 2. VPC của IPv6 lđược cố định CIDR với 56 bit 
> 3. Khi VPC cần kết nối với hạ tầng on-premises (ví dụ qua VPN hoặc AWS Direct Connect), **phải đảm bảo không có sự trùng lặp (overlap) về dải địa chỉ IP** giữa VPC và mạng on-premises. Nếu hai dải địa chỉ bị chồng lấp, việc định tuyến sẽ không hoạt động đúng và gây ra xung đột kết nối.

| Loại     | Quy tắc CIDR                                                                 |
| -------- | ---------------------------------------------------------------------------- |
| VPC IPv4 | Phải nằm trong phạm vi **địa chỉ IP private**, với prefix từ `/16` đến `/28` |
| VPC IPv6 | CIDR được cố định ở `/56` bit                                                |

> **Ví dụ địa chỉ IP private hợp lệ cho VPC:**
> 
> - `10.0.0.0/16`
> - `172.16.0.0/20`
> - `192.168.0.0/24`

>**CIDR (Classless Inter-Domain Routing)** là cơ chế phân chia địa chỉ IP mà không phụ thuộc vào phân loại lớp (class) của địa chỉ IP truyền thống, cho phép sử dụng không gian địa chỉ IP một cách hiệu quả hơn.

Dưới đây là giải thích cề kỹ năng và dich vụ liên quan đến VPC

## Subnet
Subnet là segment của mạng tạo trong VPC. Người dùng khi à tạo subnet thì sẽ chỉ định xem là tạo ở AZ nào. Một VPC có thể chứa nhiều subnet, tuy nhiên cần phải thiết kế CIDR trong phạm vi của địa chỉ IP của VPC 
Khi mà tạo subnet thì cũng cần phải cân nhắc đến tính khả dụng và bảo mật. 
- Trước tiên về tính bảo mật đi, cần phải xem sét xem là được thiết kế như là một subnet của mạng nào là public hay là private network. 
	- - **Public subnet**: Là subnet có thể giao tiếp với Internet. Đặt ở đây các server cần truy cập từ bên ngoài (ví dụ: web server, load balancer).
	- **Private subnet**: Là subnet không có kết nối trực tiếp ra Internet. Đặt ở đây các server không cần exposure ra ngoài (ví dụ: database server, application server nội bộ).
	- Bằng cách phân chia rõ ràng public/private subnet, hệ thống có thể kiểm soát quyền kết nối Internet ở cấp độ mạng, từ đó nâng cao tính bảo mật tổng thể.
	 > Định nghĩa chính xác của *public subnet* và *private subnet* được xác định dựa trên cấu hình **Route Table** — sẽ được giải thích ở phần sau. 
- Tiếp đến là quan điểm về tính khả dụng, các subnet có cùng mục đích sử dụng nên được tạo ở **nhiều AZ khác nhau**. Ví dụ: tạo một public subnet ở `ap-northeast-1a` và một public subnet nữa ở `ap-northeast-1c`. Bằng cách này, nếu một AZ gặp sự cố, hệ thống vẫn tiếp tục hoạt động thông qua subnet ở AZ còn lại — đây là nền tảng của kiến trúc **High Availability**.

> Để sử dụng tính năng Multi-AZ của các dịch vụ như **Amazon RDS**, điều kiện tiên quyết là phải có các subnet được tạo ở ít nhất hai AZ khác nhau.

<img src="images/VPC và subnet.png" alt="VPC và subnet" width="450">

| Tiêu chí            | Giải pháp                         |
| ------------------- | --------------------------------- |
| Bảo mật             | Phân chia Public / Private subnet |
| Tính khả dụng       | Tạo subnet ở nhiều AZ (Multi-AZ)  |
| Kết nối on-premises | Tránh trùng lặp dải CIDR          |

> VPC trong không gian mạng private có thể tự do quyết định địa chỉ IP. Subnet là segment network tạo bên trong VPC cần phải tạo ở từng AZ hoặc mục đích của subnet.

> **Lưu ý khi thi:** Định nghĩa của Public và Private subnet **không phải** là thuộc tính đặt khi tạo subnet, mà được xác định bởi **Route Table** liên kết với subnet đó. Một subnet được gọi là "public" khi Route Table của nó có route tới **Internet Gateway (IGW)**.

<img src="images/VPC subnet sumary.png" alt="VPC subnet summary" width="450">
<img src="images/VPC subnet quiz.png" alt="VPC subnet quiz" width="450">


## Internet Gateway IGW
Là gateway cho phép các tài nguyên bên trong VPC giao tiếp với Internet. IGW hỗ trợ cả traffic **IPv4** và **IPv6**. Vai trò của IGW gồm hai chiều:
- **Outbound**: Tài nguyên trong VPC (ví dụ EC2) có thể gửi traffic ra Internet.
- **Inbound**: Cho phép truy cập từ Internet vào tài nguyên trong VPC (ví dụ web server).

> IGW là một **Global Service** — không gắn với AZ cụ thể, tự động dư thừa và có tính khả dụng cao. Mỗi VPC chỉ có thể đính kèm **một IGW** tại một thời điểm.

**Route Table** định nghĩa các quy tắc định tuyến tĩnh (static routing) cho các tài nguyên AWS trong subnet. Route Table được thiết kế và áp dụng ở **cấp độ subnet** — mỗi subnet phải được liên kết với đúng một Route Table.

|Destination|Target|Ý nghĩa|
|---|---|---|
|`10.0.0.0/16`|`local`|Traffic nội bộ trong VPC, tự động tồn tại|
|`0.0.0.0/0`|`igw-xxxxxx`|Toàn bộ traffic còn lại → ra Internet qua IGW|

> Route `local` luôn có sẵn trong mọi Route Table và không thể xóa. Nó đảm bảo các tài nguyên trong cùng VPC có thể giao tiếp trực tiếp với nhau.

<img src="images/Route Table.png" alt="Route Table" width="450">

## NAT gateway
Nat Gateway (Network Address Translation Gateway) là một dịch vụ chuyển đổi địa chỉ network để có thể kết nối internet từ bên trong private subnet. NAT Gateway là một **AWS Managed Service** — AWS tự quản lý hạ tầng, tự động dư thừa trong phạm vi một AZ, giúp nâng cao tính khả dụng so với việc tự vận hành NAT Instance.

Hình dưới là sơ đồ luồng truyền tin khi mà EC2 Instance được lắp đặt trong public subnet và private subnet vào internet.

<img src="images/Public subnet and Private subnet.png" alt="Public subnet and Private subnet" width="450">

- **EC2 trong public subnet** A: Có thể kết nối trực tiếp ra Internet thông qua **Internet Gateway (IGW)**.
- **EC2 trong private subnet  A và B** : Vì không thể trực tiếp kết nối với internet, nên phải đi qua **NAT Gateway** được đặt tại public subnet B, sau đó NAT Gateway sẽ forward traffic ra Internet qua IGW.
> **Lưu ý thiết kế:** NAT Gateway phải được đặt ở **public subnet**, không phải private subnet.

Các trường hợp sử dụng phổ biến
Dù EC2 nằm ở private subnet không nhận kết nối từ Internet, nhưng đôi khi vẫn cần gửi traffic ra ngoài, ví dụ:
- Cập nhật, vá lỗi hệ điều hành (OS patching)
- Xác thực software license
- Tải về dependency khi deploy ứng dụng

#### NAT Gateway vs NAT Instance

Trước khi có NAT Gateway, người dùng có thể tự xây dựng chức năng NAT bằng cách sử dụng một EC2 Instance thông thường, gọi là **NAT Instance**. Việc này cần thiết kế vô hiệu quá Source/Destination Check và loại bỏ traffic của bản thân.
NAT Intance này do sử dụng EC2 intance như là một NAT server nên càn thiết xem sét cơ chế dư thừa để tránh SPOF (Single Point of Failure).

| Tiêu chí                       | NAT Gateway              | NAT Instance                       |
| ------------------------------ | ------------------------ | ---------------------------------- |
| Loại                           | AWS Managed Service      | EC2 Instance tự quản lý            |
| Tính khả dụng                  | Tự động dư thừa trong AZ | Phải tự thiết kế cơ chế redundancy |
| Source/Destination Check       | Tự động tắt              | **Phải tắt thủ công**              |
| Khả năng mở rộng               | Tự động scale            | Giới hạn bởi instance type         |
| SPOF (Single Point of Failure) | Không                    | Có — cần thiết kế thêm để tránh    |
| Khuyến nghị                    | ✅ Ưu tiên sử dụng        | Chỉ dùng khi có yêu cầu đặc biệt   |
**Lưu ý quan trọng khi dùng NAT Instance**
EC2 mặc định có tính năng **Source/Destination Check** — tức là nó sẽ kiểm tra và loại bỏ các packet mà địa chỉ nguồn/đích không phải chính nó. Vì NAT Instance cần forward traffic của các instance khác, tính năng này **phải được tắt đi** thủ công mới hoạt động được.

Ngoài ra, vì NAT Instance là một EC2 bình thường, nếu instance này gặp sự cố thì toàn bộ private subnet sẽ mất kết nối Internet — đây là điểm **Single Point of Failure (SPOF)**. Cần thiết kế cơ chế dự phòng (ví dụ: Auto Scaling, failover) nếu muốn đảm bảo tính khả dụng cao.

> Để mà kết nối vào internet từ EC2 trong private subnet thì cần có NAT Intance hoặc là NAT gateway

> **Lưu ý khi thi:** Câu hỏi kinh điển trong SAA-C03: _"EC2 ở public subnet nhưng không kết nối được Internet — nguyên nhân là gì?"_ — hãy kiểm tra theo thứ tự: 
> - (1) Route Table có route tới IGW không? 
> - (2) EC2 có public/Elastic IP không? 
> - (3) Security Group có mở port cần thiết không?

<img src="images/NAT Gateway quiz.png" alt="NAT Gateway quiz" width="450">

<img src="images/AWS VPC.png" alt="AWS VPC" width="450">

## Giới hạn Network Access
### Security Group
Security Group là tính năng **firewall hoạt động ở cấp độ instance (EC2)**. Mỗi Security Group định nghĩa hai chiều truyền tin:
- **Inbound**: Giới hạn traffic đi vào EC2
- **Outbound**: Giới hạn traffic đi ra từ EC2

Đặc trưng của securtiy group được mô tả dưới đây. (Chi tiết hơn thì tham khảo [[2-3 Network Security]])
- Giá trị mặc định: Outboard thì cho phép tất cả, còn InBoard thì từ chối tất cả
- Chỉ định nghĩa những quy tắc cho phép (Allow-only), Security Group không có quy tắc *deny* tường minh — tất cả traffic không khớp với bất kỳ Allow rule nào đều bị từ chối ngầm định. Mỗi rule chỉ định:
	- Loại traffic: Inbound / Outbound
	- Giao thức: TCP / UDP / ICMP...
	- Phạm vi port: ví dụ `80`, `443`, `0-65535`
	- Nguồn / đích: địa chỉ IP (CIDR), hoặc Security Group khác
- Một EC2 instance có thể gán **nhiều Security Group** cùng lúc — các rule từ tất cả các group được hợp nhất.
- Thay đổi rule có hiệu lực **ngay lập tức**, không cần restart instance.
- Security Group là **Stateful** — nếu một request inbound được cho phép, traffic phản hồi (return traffic) sẽ tự động được cho phép outbound mà không cần tạo rule outbound riêng, và ngược lại.

### Network Access Control List (ACL)
Network ACL là tính năng **firewall hoạt động ở cấp độ subnet**. Khác với Security Group, ACL kiểm soát toàn bộ traffic vào/ra của một subnet, không phải từng instance. **Ví dụ sử dụng thực tế:** Từ chối traffic từ public subnet đến subnet chứa database (để bảo vệ DB khỏi bị truy cập trực tiếp từ tầng web). Từ chối traffic SMTP (port 25) từ subnet ra ngoài (để ngăn spam email từ server bị xâm nhập). Cũng giống như là security Group thì thằng này cũng có 2 loại truyền tin là OutBoard và InBoard.

Đặc trưng chính của ACL như dưới đây. (Chi tiết hơn thì tham khảo [[2-3 Network Security]])
- **Default ACL** (tạo tự động khi tạo VPC):
	- Inbound → Cho phép tất cả
	- Outbound → Cho phép tất cả
- **Custom ACL** (tạo mới):
	- Inbound → Từ chối tất cả (mặc định ban đầu)
	- Outbound → Từ chối tất cả (mặc định ban đầu)
- Đối với từng loại Inboard và Outboard **hỗ trợ cả Allow và Deny tường minh** — đây là điểm khác biệt lớn so với Security Group.
- **Stateless** — không nhớ trạng thái kết nối. Phải định nghĩa rule cho cả hai chiều Inbound và Outbound một cách độc lập. Ví dụ: cho phép HTTP inbound không tự động cho phép phản hồi HTTP outbound.
- Sau khi mà đánh số cho từng luật, thì có thể sử dụng luật đồng ý và từ chối cho từng số hiệu.
- **Quy tắc đánh số (numbered rules):** Mỗi rule được gán một số thứ tự. ACL đánh giá các rule theo thứ tự tăng dần — rule đầu tiên khớp sẽ được áp dụng, các rule sau không được kiểm tra.
	```
	Rule 100: ALLOW  TCP  0.0.0.0/0  port 443
	Rule 200: ALLOW  TCP  0.0.0.0/0  port 80
	Rule 300: DENY   TCP  10.0.2.0/24  ALL     ← từ chối DB subnet
	Rule *  : DENY   ALL  0.0.0.0/0            ← implicit deny (cuối cùng)
	```

Bảng dưới đây là so sánh Security Group và Network ACL 

| Tiêu chí             | Security Group                                       | Network ACL (Default)                                |
| -------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| Phạm vi áp dụng      | Instance (EC2)                                       | Subnet                                               |
| Default              | Inboard: Từ chối tất cả<br>Outboard: Cho phép tất cả | Inboard: Cho phép tất cả<br>Outboard: Từ chối tất cả |
| Loại rule            | Allow only                                           | Allow và Deny                                        |
| Thứ tự đánh giá      | Tất cả rule được đánh giá                            | Theo số thứ tự — rule đầu khớp thắng                 |
| Stateful / Stateless | Statefull                                            | Stateless                                            |
Trong thực tế, Security Group và Network ACL được dùng **kết hợp** với nhau theo nguyên tắc **defense in depth** (bảo vệ nhiều lớp):
```
Internet
  ↓
Internet Gateway
  ↓
Network ACL (subnet level) ← lớp 1: lọc theo IP/port toàn subnet
  ↓
Security Group (instance level) ← lớp 2: lọc chi tiết cho từng EC2
  ↓
EC2 Instance
```
- **Network ACL** là phòng thủ ngoài cùng — dùng để block toàn bộ dải IP độc hại, hoặc deny một loại traffic cho toàn subnet.
- **Security Group** là phòng thủ sát instance — kiểm soát chính xác port và nguồn truy cập cho từng server.
<img src="images/Security Group and Network ACL.png" alt="Security Group and Network ACL" width="450">

> **Lưu ý khi thi:**
> - **Security Group** là **Stateful** → chỉ cần tạo rule một chiều, return traffic được tự động cho phép.
> - **Network ACL** là **Stateless** → bắt buộc phải tạo rule cho **cả Inbound lẫn Outbound**. Đây là lỗi phổ biến nhất khi cấu hình ACL — tạo rule inbound cho phép nhưng quên tạo rule outbound tương ứng, khiến phản hồi bị chặn.
> - Khi cần **block một địa chỉ IP cụ thể**, dùng **Network ACL** (vì Security Group không có rule Deny tường minh).

<img src="images/Security Group and Network ACL Quiz.png" alt="Security Group and Network ACL Quiz" width="450">

<img src="images/Security Group and Network ACL Summary.png" alt="Security Group and Network ACL Summary" width="450">

# Kết nối VPC với bên ngoài
### AWS Direct Connect
**AWS Direct Connect (DX)** là dịch vụ kết nối giữa AWS và môi trường on-premises thông qua **đường truyền vật lý chuyên dụng** (dedicated line), không đi qua Internet công cộng.
Nhờ đường truyền riêng, Direct Connect cung cấp:
- **Băng thông cao** và ổn định
- **Độ trễ thấp** và nhất quán
- **Bảo mật cao** hơn so với VPN qua Internet

##### Direct Connect Gateway
Thêm vào đó, Direct Connect Gateway cũng là một tính năng bổ xung của Direct conect. Thông thường Direct Connect chỉ cho phép kết nối 1vs1 giữa on-premises và VPC, tuy nhiên Direct Connect Gateway có thể cho phép kết nối 1 nhiều tức là cho phép kết nối 1 on-premises và nhiều VPC. 

> **Lưu ý thuật ngữ:** Direct Connect thường được viết tắt là **DX**. Cần phân biệt với "DX" trong ngành Digital Transformation — hai khái niệm hoàn toàn khác nhau.

### AWS Virtual Private Network (VPN)

AWS VPN là dịch vụ tạo kết nối mạng riêng ảo (VPN) giữa AWS và on-premises hoặc thiết bị client. Bao gồm hai dịch vụ chính:  AWS site-to-site VPN và AWS Client VPN.
#### AWS Site-to-Site VPN
Ngoại trừ Direct Connect ra thì cách kết nối giữa AWS và môi trường on-premises còn có cách AWS Site-to-Site VPN có thể truyền tin một cách bảo mật sử dụng giao thức IPSec. Do sử dụng đường truyền internet, nên là nếu mà so sánh với Direct Connect thì thông lượng và chất lượng sẽ bị giảm, tuy nhiên ưu điểm là chi phí và thời gian ngắn. 

>  IPSec (Security Architecture for INternet Protocal) là kỹ thuật phòng chống nghe trộm hoặc giả mạo bằng cách sẽ mã hoáIP package, được sử dụng rất nhiều giữa on primice và VPN

| Tiêu chí             | Site-to-Site VPN                 | Direct Connect                   |
| -------------------- | -------------------------------- | -------------------------------- |
| Đường truyền         | Internet (có mã hóa IPSec)       | Đường chuyên dụng vật lý         |
| Băng thông / ổn định | Thấp hơn, phụ thuộc Internet     | Cao, ổn định                     |
| Chi phí              | Thấp                             | Cao                              |
| Thời gian triển khai | Nhanh (vài giờ)                  | Chậm (vài tuần đến vài tháng)    |
| Phù hợp              | Kết nối nhanh, ngân sách hạn chế | Workload quan trọng, cần ổn định |

#### AWS Client VPN
AWS Client VPN là dịch vụ cung cấp kết nối VPN từ Client Device (laptop, điện thoại) đến tài nguyên trong VPC như là EC2. Do là dịch vụ Full Managed nên là không cần xây dựng VPN server mà chỉ cần cáu tạo môi trường VPN và cấp quyền cho người dùng. 

#### Virtual Private Gateway (VPG)
là gateway sử dụng khi mà kết nối AWS với môi trường on-premises được đính vào VPC. Cần tạo và attach VGW vào VPC trước khi thiết lập kết nối hybrid (Direct Connect hay là Site-to-Site VPN)
```
On-premises                      AWS
    │                             │
  Router ──── DX / VPN ──── Virtual Private Gateway ──── VPC
```

> Lưu ý khi thi: Để mà kết nối private giữa on-premises và AWS thì cần sử dụng Direct Connect hoặc là Site-to-Site VPN. Direct Connect thì hơn Site-to-Site VPN về mặt thông lượng cao và độ ổn định, tría lại thì lại tốn thời gian và chi phí. 

### VPC peer link
Là dịch vụ kết nối private trực tiếp giữa hai VPC khác nhau. Do là VPC sẽ tạo không gian private trên AWS, nên là không thể tiến hành truyền thông trực tiếp giữa các VPC. Tuy nhiên, Băng việc kết nối VPC peer link với các VPC khác, thì có thể kết nối trực tiếp trung cùng mạng private mà không cần thông qua internet.

Đặc điểm:
- Có thể kết nối VPC giữa **các tài khoản AWS khác nhau**.
- Có thể kết nối VPC giữa **các Region khác nhau** (inter-region peering).
- **Không hỗ trợ transitive routing** — nếu VPC-A peering với VPC-B và VPC-B peering với VPC-C, VPC-A vẫn không thể nói chuyện với VPC-C trừ khi tạo peering trực tiếp.
- **Không thể kết nối nếu CIDR bị trùng lặp** giữa hai VPC.

Hình dưới đây mô tả ví dụ về kết nối VPC peer link với Direct Connect Gateway.

<img src="images/Ví dụ về kết nối VPC peer link và Direct Gateway.png" alt="Ví dụ về kết nối VPC peer link và Direct Gateway" width="450">

### AWS Transit Gateway
là dịch vụ lắp đặt Gateway hoạt động như một **hub trung tâm** trong VPC. Trong trường hợp mà VPC cụ thể cần kết nối ra bên ngoài network, thì cần phải tiến hành kết nối, thiết kế, vận hành với từng dịch vụ đã nói trước đó (Direct Connect, VPN, VPC peer link). Đương nhiên, khi số lượng VPC và kết nối tăng lên, việc quản lý từng peering link riêng lẻ trở nên phức tạp, bằng việc sử dụng Transit Gateway thì có thể đơn giản hoá và chỉ cần quản lý 1 nguồn

**Trước Transit Gateway** — mô hình mesh (phức tạp):

```
VPC-A ──── VPC-B
  │    ╲  ╱   │
  │     ╳     │
  │    ╱  ╲   │
VPC-C ──── VPC-D
```

**Sau Transit Gateway** — mô hình hub-and-spoke (đơn giản):

```
VPC-A ──┐
VPC-B ──┤
VPC-C ──┼── Transit Gateway ── Direct Connect Gateway ── On-premises (DX / VPN)
VPC-D ──┤
VPC-E ──┘
```

Lợi ích: quản lý tập trung, giảm số lượng kết nối cần duy trì, dễ mở rộng.

Hình dưới là mô tả về ví dụ kết nối của Transit Gateway

<img src="images/Ví dụ về kết nối Transit Gateway.png" alt="Ví dụ về kết nối Transit Gateway" width="450">

#### VPC End Point
Thông thường, traffic từ VPC đến các dịch vụ AWS (như S3, DynamoDB) sẽ đi ra Internet. **VPC Endpoint** cho phép kết nối đến dịch vụ AWS **hoàn toàn qua mạng private của AWS**, không cần qua Internet, không cần NAT Gateway hay IGW.

Có hai loại VPC Endpoint:
**Gateway Endpoint**
	- Hoạt động ở **Network layer** — thêm target vào Route Table của subnet.
	- Hỗ trợ: **Amazon S3** và **Amazon DynamoDB**.
	- **Miễn phí**.
```
EC2 (private subnet)
  → Route Table: S3 → vpce-xxxxxx (Gateway Endpoint)
  → Amazon S3  [không qua Internet]
```
**Interface Endpoint (AWS PrivateLink)**
- Hoạt động ở **Application layer** — tạo một Elastic Network Interface (ENI) trong subnet với private IP.
- Hỗ trợ hầu hết các dịch vụ AWS: **CloudWatch**, **SQS**, **SNS**, **API Gateway**, v.v.
- Còn gọi là **AWS PrivateLink**.
- **Có tính phí** theo giờ và theo lượng data.

> Lưu ý khi thi: VPC endpoint có 2 hình thái gateway và interface, và có thể truy cập dịch vụ AWS mà không cần qua internet.

Tổng quan so sánh các phương thức kết nối

|Kết nối|Dùng để nối|Đặc điểm|
|---|---|---|
|**Direct Connect**|On-premises ↔ AWS|Đường chuyên dụng, băng thông cao, ổn định, tốn kém|
|**Site-to-Site VPN**|On-premises ↔ AWS|Qua Internet (IPSec), nhanh triển khai, chi phí thấp|
|**Client VPN**|Thiết bị cá nhân ↔ AWS|Fully Managed, cho remote access|
|**VPC Peering**|VPC ↔ VPC|Private, không transitive, không trùng CIDR|
|**Transit Gateway**|Nhiều VPC + On-premises|Hub trung tâm, đơn giản hóa kết nối phức tạp|
|**VPC Endpoint**|EC2 trong VPC ↔ Dịch vụ AWS|Không qua Internet, private access đến AWS services|


<img src="images/VPC kết nối ra bên ngoài Chart.png" alt="VPC kết nối ra bên ngoài Chart" width="450">

<img src="images/VPC kết nối ra bên ngoài Quiz.png" alt="VPC kết nối ra bên ngoài Quiz" width="450">

<img src="images/Kết nối VPC ra bên ngoài Summary.png" alt="Kết nối VPC ra bên ngoài Summary" width="450">


## Elastic Load Balancing (ELB)
**Elastic Load Balancing (ELB)** là dịch vụ cân bằng tải của AWS, có nhiệm vụ phân phối traffic đến nhiều **Backend Instance** (các EC2 Instance đã được đăng ký vào ELB). ELB hoạt động như một điểm tiếp nhận duy nhất cho client, ẩn đi sự phức tạp của hạ tầng phía sau.
Có 4 loại ELB và đặc điểm từng loại khác nhau.

1. Classic Load Balancer (CLB): Cung cấp cân bằng tải theo tiêu chuẩn. Những TCP request tới CLB sẽ được gán cho Backend Instance và điều hướng traffic đến port cụ thể.
2. Application Load Balancer (ALB): hoạt động ở Layer 7 (tầng ứng dụng), dựa vào nội dung của request để điều hướng đến mục tiêu đã xác định và tiến hành xử lý. Do đó mà nó có thể giao nhiệm vụ traffic HTTP/HTTPS sử dụng cho server cụ thể. 
	- Hỗ trợ **content-based routing**: điều hướng dựa trên URL path, hostname, HTTP header, query string.
    - Ví dụ: `/api/*` → nhóm EC2 dành cho API, `/static/*` → S3 hoặc EC2 dành cho static files.
	- Phù hợp với kiến trúc microservices và container (ECS/EKS).
	- Hỗ trợ WebSocket và HTTP/2.
3. Network Load Balancer (NLB): Hoạt động ở Layer 4 (TCP/UDP) — không phân tích nội dung request, chỉ dựa trên IP và port, sử dụng trong các trường hợp workload có thông lượng cao và độ trễ cực thấp. Bảo toàn địa chỉ IP nguồn (source IP preservation) — backend instance nhận được IP thực của client.
4. Gateway Load Balancer (GLB): Hoạt đông ở Layer 3 (IP packet) — dùng để điều hướng traffic qua các **virtual network appliance**, nhằm nâng cao tính khả dụng của các thiết bị ảo như là fireware hay là phát hiện xâm phạm.

Tóm tắt lựa chọn ELB theo use case

```
Cần routing theo URL path / header          → ALB (Layer 7)
Cần throughput cao / độ trễ thấp / source IP → NLB (Layer 4)
Cần kiểm soát traffic qua firewall ảo       → GWLB (Layer 3)
Hệ thống legacy                             → CLB (không khuyến khích mới)
```


Dưới đây là giải thích về các đặc trưng quan trọng của ELB
1. Tính khả dụng cao (High Availability)
	ELB có thể được triển khai trên **nhiều AZ** cùng lúc. Traffic chỉ được chuyển đến các Backend Instance đang hoạt động bình thường — nếu một Instance hoặc một AZ gặp sự cố, ELB tự động loại Instance đó ra khỏi pool và tiếp tục phân phối đến các Instance còn lại.
2. Tự động Scale (Auto Scaling)
	ELB tự động scale theo lưu lượng traffic — AWS quản lý hạ tầng bên trong ELB. Vì IP của ELB thay đổi khi scale, **không nên kết nối đến ELB bằng địa chỉ IP**, mà phải dùng **tên DNS (Endpoint)** được AWS cấp.
3. Tính bảo mật
	ELB cũng có tính năng giải mã SSL, bằng việc này thì có thể giúp quản lý certificate tập trung tại một nơi và giảm tải xử lý mã hóa cho Backend. Bản thân ELB có thể gán **Security Group** riêng, giới hạn chỉ ELB mới nhận traffic từ Internet — Backend Instance chỉ nhận traffic từ ELB.
4. Health Check và Monitoring
	ELB liên tục thực hiện **health check** đối với Backend Instance. Nếu Instance bất thường → tự động bị loại khỏi pool phân phối và sẽ tiến hành truyền tin với chỉ với EC2 instance bình thường. Nếu Instance hồi phục → tự động được thêm lại vào pool. Khi đó bằng việc sử dụng tính năng Sticky Session (Session Affinity) sẽ cho phép gán request của cùng một người dùng đến cùng một Backend Instance (instance đã tạo session lần đầu). Hữu ích cho ứng dụng lưu trạng thái session trên server.
	Thêm vào đó thì khi xoá đăng ký Backend Instance ra khỏi ELB thì cần phải chú ý gián đoạn dịch vụ đột ngột nếu mà server đang tiến hành xử lý. Trường hợp này nếu dùng tính năng Connection Draining thì có thể trì hoãn việc xoá cho đến khi request đang xử lý hoàn thành. 
	ELB còn có thể ghi log toàn bộ request (số lượng request, thời gian phản hồi, lỗi...) và lưu vào **Amazon S3** để phân tích sau.
5. Cross-Zone Load Balancing
	Nếu sử dụng Cross-Zone Load Balancing thì traffic sẽ được phân phối đều đến tất cả Instance trên **tất cả AZ**. Trái lại, trường hợp vô hiệu thì traffic sẽ được phân phối đều trong từng AZ — AZ có ít Instance hơn sẽ bị quá tải.
	Ví dụ minh họa: AZ-a có 2 instance, AZ-c có 8 instance.
	- **Cross-zone bật**: mỗi instance nhận 10% traffic.
	- **Cross-zone tắt**: 2 instance ở AZ-a mỗi cái nhận 25%, 8 instance ở AZ-c mỗi cái nhận 6.25%.
> Mặc định: ALB bật cross-zone, NLB và GWLB tắt cross-zone (có thể bật thủ công).

6. Internet-facing vs Internal ELB
	ELB sẽ hoạt động ở bất ký Internal nào như là ELB nội bộ và Internet-facing đối với ELB ngoại (đối với internet công khai). Trường hợp Internet-facing thì do là bản thân ELB đang được đặt trong public subnet là không cần đặt Backend instance vào trong public subnet, thay vào đó bằng việc đặt Backend Instance vào trong subnet private thì có thể năng cao tính bảo mật. Internal thì chỉ có thể được sử dụng trong môi trường VPC hoặc là môi trường On-premises.

| Loại                | Vị trí đặt     | Truy cập từ                 |
| ------------------- | -------------- | --------------------------- |
| **Internet-facing** | Public subnet  | Internet công cộng          |
| **Internal**        | Private subnet | Nội bộ VPC hoặc on-premises |

**Kiến trúc bảo mật phổ biến:**

```
Internet
  ↓
Internet-facing ELB (public subnet)   ← nhận traffic từ Internet
  ↓
Backend EC2 (private subnet)          ← không tiếp xúc trực tiếp với Internet
```

ELB đặt ở public subnet làm điểm tiếp nhận duy nhất; Backend Instance được giấu trong private subnet, nâng cao bảo mật mà không ảnh hưởng đến khả năng phục vụ.

7. AWS Certificate Manager (ACM)
	**ACM** quản lý tập trung toàn bộ certificate SSL/TLS được dùng trên các dịch vụ AWS. Thêm vào đó thì còn có thể 
		- Phát hành mới certificate miễn phí cho domain của bạn
		- **Import** certificate SSL đã tạo từ bên ngoài.
		- **Tự động gia hạn** certificate do ACM phát hành.
	Certificate từ ACM được dùng chủ yếu trong:
	- **ELB** (ALB/NLB) — HTTPS termination.
	- **Amazon CloudFront** — HTTPS cho CDN.
	- **API Gateway** — HTTPS cho API endpoint.

![[ELB Flow.png]]
> **Lưu ý khi thi:**
> - ELB phải được triển khai trên **nhiều AZ** để đảm bảo High Availability.
> - **Kết nối bằng DNS name**, không phải IP address — IP của ELB thay đổi khi scale.
> - Kiến trúc chuẩn: **ELB ở public subnet, Backend ở private subnet** — bảo mật và đơn giản hóa quản lý SSL.
> - **ALB** = Layer 7, content-based routing. **NLB** = Layer 4, hiệu năng cao, giữ source IP.
> - Khi đề thi hỏi "block một IP cụ thể ở tầng ELB" → dùng **Network ACL** (ELB không có Deny rule như Security Group).

![[ELB Quiz.png]]

![[ELB Summary.png]]


## Amazon CloudFront
**Amazon CloudFront** là dịch vụ **CDN (Content Delivery Network)** của AWS, phân phối nội dung đến người dùng từ **Edge Location gần nhất về mặt địa lý**. Thay vì mọi request đều phải đi đến server gốc (origin), CloudFront lưu nội dung vào cache tại hơn 600 Edge Location trên toàn cầu — giảm đáng kể khoảng cách vật lý, từ đó giảm độ trễ và tăng tốc độ tải.
> **Origin** là nguồn nội dung gốc mà CloudFront lấy về để cache và phân phối. Các loại origin được hỗ trợ:

| Origin                              | Ứng dụng điển hình                             |
| ----------------------------------- | ---------------------------------------------- |
| **Amazon S3**                       | Static website, hình ảnh, video, file download |
| **Application Load Balancer (ALB)** | Dynamic content từ web application             |
| **Amazon EC2**                      | Custom origin server                           |
| **API Gateway**                     | REST API, ser
Luồng hoạt động cơ bản:

```
User request
  ↓
Edge Location (CloudFront)
  ├── Cache hit  → trả về ngay, không cần gọi origin
  └── Cache miss → gọi Origin, lấy nội dung, cache lại, trả về user
```

Dưới đây là các tính năng chính của CloudFront:
1. Mã hoá tín hiệu dựa trên SSL: Có thể truyền tin mã hoá SSL - sử dụng chứng chỉ SSL ở trong CloudFront. CloudFront hỗ trợ mã hóa HTTPS trên hai đoạn:
	- **User → Edge Location**: Sử dụng certificate do AWS cấp (miễn phí qua ACM) hoặc certificate tự cung cấp.
	- **Edge Location → Origin (Origin Protocol Policy)**: Có thể cấu hình kết nối từ CloudFront đến origin cũng dùng HTTPS — đảm bảo mã hóa end-to-end.
2. Signed URL và Signed Cookie: Là tính năng phát hành URL cho phép truy cập tạm thời, và chỉ những user được phép trong giới hạn mới vô được URL. **Signed Cookie**: Tương tự nhưng dùng cho nhiều file cùng lúc — phù hợp cho nội dung streaming hoặc subscription-based content. **Ứng dụng**: Video subscription, tài liệu trả phí, phần mềm có bản quyền.
3. Custom Error Page: Khi origin trả về lỗi (4xx, 5xx), CloudFront có thể hiển thị **trang lỗi tùy chỉnh** (branded error page) thay vì lỗi mặc định của server — cải thiện trải nghiệm người dùng và ẩn thông tin kỹ thuật nhạy cảm.
4. Geo Restriction (Giới hạn theo khu vực địa lý): Dựa vào địa chỉ IP của user, CloudFront có thể:
	- **Whitelist**: Chỉ cho phép truy cập từ danh sách quốc gia được chỉ định.
	- **Blacklist**: Chặn truy cập từ danh sách quốc gia bị cấm
	**Ứng dụng**: Tuân thủ bản quyền nội dung theo vùng (geo-licensing), chặn traffic từ quốc gia có rủi ro bảo mật cao.
5. Streaming: Ngoài việc truyền nội dung của dịch vụ web ra thì còn có thể xử lý cả truyền streaming âm thanh hoặc hình ảnh, video.

Nhờ mạng Edge Location toàn cầu, CloudFront có thể xử lý hàng triệu người xem đồng thời với độ trễ thấp.

Tóm tắt

```
CloudFront
├── Global Service (600+ Edge Location)
├── Origin: S3 · ALB · EC2 · API Gateway · Custom HTTP
├── Bảo mật: SSL end-to-end · Signed URL/Cookie · Geo Restriction
├── Performance: Cache · Streaming · Lambda@Edge
└── Monitoring: Real-time logs → S3 · CloudWatch metrics
```
![[CloudFront Flow.png]]
> **Lưu ý khi thi:**
> - CloudFront cache nội dung từ S3, ELB, EC2 → giảm độ trễ, tăng hiệu suất toàn cầu.
> - **Signed URL** = kiểm soát truy cập từng file có thời hạn. **Geo Restriction** = chặn/cho phép theo quốc gia.
> - Khi đề hỏi về **bảo vệ S3 bucket khỏi truy cập trực tiếp** → dùng **Origin Access Control (OAC)** kết hợp CloudFront.
> - Khi đề hỏi về **giảm độ trễ cho user toàn cầu** → CloudFront là đáp án ưu tiên.
> - CloudFront là **Global Service** — không cần chọn Region khi tạo distribution.

![[CloudFront Summary.png]]
## Amazon Route 53
Amazon Route 53 là dịch vụ DNS được quản lý hoàn toàn bởi AWS, có tính khả dụng cao và độ trễ thấp.
Route 53 sử dụng **Hosted Zone** để quản lý các DNS record. Có 2 loại:
- Public Zone - DNS hướng ra ngoài: Phân giải tên miền cho website/ứng dụng công khai
- Private Zone - DNS bên trong VPC: Phân giải tên miền trong hệ thống nội bộ, không lộ ra internet

DNS Record Types
- **A Record** — Trỏ tên miền đến một **địa chỉ IP** cụ thể.
- **CNAME Record** — Trỏ tên miền đến một **tên miền khác** (không dùng được ở Zone Apex).

Route 53 cung cấp loại record đặc biệt là **ALIAS Record**:
- Cho phép trỏ đến endpoint của **CloudFront**, **ELB**, **S3**, v.v. như một **A Record**, thay vì phải dùng CNAME.
- Khác với CNAME, **ALIAS Record có thể dùng tại Zone Apex** (ví dụ: `example.com` thay vì `www.example.com`).
- Không phát sinh thêm phí DNS query khi dùng ALIAS trỏ đến tài nguyên AWS.

Routing Policy: Ngoài chức năng DNS cơ bản, Route 53 còn hỗ trợ **Routing Policy** để kiểm soát cách phân phối traffic khi có query đến. Có thể thiết lập một hoặc nhiều Routing Policy trên mỗi record.

| Routing Policy        | Mô tả                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------- |
| **Latency-Based**     | Route traffic đến region có **độ trễ thấp nhất** với người dùng → Tăng hiệu năng        |
| **Weighted**          | Phân phối traffic theo tỷ lệ phần trăm giữa các endpoint                                |
| **Geolocation**       | Route traffic dựa trên **vị trí địa lý** của người dùng                                 |
| **Failover**          | Route traffic đến endpoint dự phòng khi endpoint chính bị lỗi → Tăng tính khả dụng (HA) |
| **Simple**            | Phân giải đến một endpoint duy nhất, không có logic đặc biệt                            |
| **Geoproximity**      | Route traffic dựa theo vị trí địa lý + có thể điều chỉnh bias                           |
| **Multivalue Answer** | Trả về nhiều giá trị IP, có health check tích hợp                                       |

Thêm vào đó thì không chỉ đơn giản là tính năng của DNS mà cũng có thể sử dụng routing policy để giới hạn truyền tin và cũng có thể thiết lập xem là xử lý như nào đối với query Route 53 khi mà tạo ra record.

> **Ghi nhớ khi thi:**
> 
> - Dùng **Failover Routing** để xây dựng kiến trúc **High Availability (HA)**.
> - Dùng **Latency-Based Routing** để xây dựng mạng có **hiệu năng cao**.

![[Route 53 Summary.png]]
Question 1 — Foundation (Beginner)
**Which statement correctly describes the difference between an ALIAS Record and a CNAME Record in Amazon Route 53?**
- A) CNAME records can be used at the Zone Apex, but ALIAS records cannot.
- B) ALIAS records can point to AWS resources like ELB or CloudFront and can be used at the Zone Apex, while CNAME records cannot be used at the Zone Apex.
- C) ALIAS and CNAME records are functionally identical; the only difference is cost.
- D) CNAME records resolve to IP addresses directly, while ALIAS records resolve to domain names.

> **✔ Answer: B** ALIAS records support Zone Apex and integrate natively with AWS resources at no extra query cost. CNAME records cannot be used at the Zone Apex (e.g., `example.com`).

Question 2 — Applied Architecture (Advanced)

**A company runs a global web application deployed in `ap-northeast-1` (Tokyo) and `us-east-1` (Virginia). They want to ensure users are always routed to the fastest region, but if one region becomes unhealthy, all traffic should automatically shift to the healthy region. Which combination of Route 53 configurations best satisfies these requirements?**

- A) Weighted Routing with equal weights (50/50) on both endpoints, with health checks enabled.
- B) Latency-Based Routing with health checks enabled on both records, combined with Failover Routing as a fallback.
- C) Geolocation Routing targeting Asian users to Tokyo and all others to Virginia, with health checks.
- D) Simple Routing with multiple IP values returned for the same record.

> **✔ Answer: B** Latency-Based Routing directs users to the lowest-latency region for optimal performance. When combined with health checks, Route 53 automatically removes unhealthy endpoints from DNS responses, effectively providing failover behavior. This is the standard pattern for high-performance + high-availability global architectures.