**SSL**
`
## SSL (Secure Sockets Layer)

```text
- SSL là gì?
Là giao thức bảo mật internet được phát triển bởi Netscape vào năm 1995, nhằm mã hóa dữ liệu truyền giữa máy chủ và trình duyệt để đảm bảo tính riêng tư, xác thực và toàn vẹn thông tin.

- Có bao nhiêu cách xác thực SSL?
Tại Vietnix, việc xác thực chứng chỉ SSL (đặc biệt là dòng DV - Domain Validation phổ biến) thường bao gồm 3 phương thức chính:
  + Xác thực qua DNS (CNAME): Thêm bản ghi CNAME vào cấu hình DNS của tên miền.
  + Xác thực qua Email: Sử dụng email quản trị tên miền (admin@domain.com) để xác nhận.
  + Xác thực qua File: Tải file xác thực do Vietnix cung cấp lên thư mục gốc của website.

 - CSR file dùng để làm gì?

 - Gen file CSR và request SSL cho domain `tech.training.vietnix.tech` bằng OpenSSL Trên Linux.
Bước 1: Tạo Private Key (.key)
Chạy lệnh sau để tạo khóa riêng tư (độ dài 2048bit):
_openssl genrsa -out tech.training.vietnix.tech.key 2048
_
Bước 2: Tạo CSR
Chạy lệnh sau để tạo file CSR từ khóa vừa tạo:
_openssl req -new -key tech.training.vietnix.tech.key -out tech.training.vietnix.tech.csr
_

Bước 3: Điền thông tin CSR
Sau khi chạy lệnh trên, terminal sẽ yêu cầu điền các thông tin, hãy điền như sau:

   +  Country Name (2 letter code): VN
   + State or Province Name: Ho Chi Minh (hoặc tên tỉnh/thành của bạn)
   + Locality Name: Ho Chi Minh (hoặc tên quận/huyện)
   + Organization Name: Vietnix (hoặc tên công ty/tổ chức)
   + Organizational Unit Name: IT Department
   + Common Name: tech.training.vietnix.tech (QUAN TRỌNG: Phải trùng tên miền)
   + Email Address: Điền email của bạn
   + A challenge password: Bỏ qua, nhấn Enter.

Kết quả

   + tech.training.vietnix.tech.key: File private key, cần bảo mật tuyệt đối.
   + tech.training.vietnix.tech.csr: File CSR, dùng để gửi cho nhà cung cấp SSL (như Vietnix) để đăng ký chứng chỉ. 

Bạn có thể kiểm tra nội dung CSR bằng lệnh:
_cat tech.training.vietnix.tech.csr_

- Pem file là gì?
File PEM (Privacy Enhanced Mail) là một định dạng tệp phổ biến dựa trên mã hóa Base64, được sử dụng rộng rãi để lưu trữ và truyền tải dữ liệu mật mã như chứng chỉ SSL/TLS, khóa riêng tư (private key), khóa công khai (public key) và các chứng chỉ trung gian. Các tệp này thường có đuôi .pem, .crt, .cer, hoặc .key và được định nghĩa bởi các dòng "-----BEGIN..." và "-----END...".


- Private Key SSL là gì?
Private Key SSL (Khóa bí mật) là một tệp tin mã hóa quan trọng được tạo ra cùng lúc với CSR, có chức năng giải mã dữ liệu đã được mã hóa bởi Public Key (Khóa công khai) trong chứng chỉ SSL. Nó được máy chủ lưu trữ bảo mật tuyệt đối, không chia sẻ ra ngoài để đảm bảo an toàn cho phiên kết nối. 
Các đặc điểm và vai trò của Private Key SSL:
  +  Chức năng: Sử dụng thuật toán bất đối xứng để giải mã thông tin từ trình duyệt (client) gửi đến máy chủ (server), thiết lập kết nối SSL/TLS an toàn.
  + Tạo ra: Private Key thường được tạo ra trên máy chủ cùng với CSR (Certificate Signing Request).
  +   Bảo mật: Private Key phải được giữ kín. Nếu mất, bạn phải thu hồi và đăng ký lại chứng chỉ SSL mới.
  + Sử dụng: Private key thường được lưu trữ dưới dạng tệp .key và cài đặt cùng với chứng chỉ SSL trên web server (Nginx, Apache). 
Khi cài đặt SSL, file Private Key (.key) sẽ hoạt động cùng với file Certificate (.crt) để xác thực và mã hóa dữ liệu trên website

- PFX file là gì? Cách chuyển từ CRT sang PFX.
#### 1. File PFX la gi?
* **Dinh nghia:** **PFX** (hay **PKCS #12**) la dinh dang tep dung de luu tru toan bo cac thanh phan cua mot chung chi bao mat (bao gom: Server Certificate, Private Key va CA Bundle) vao trong **mot tep duy nhat**.
* **Muc dich:** Thuong duoc su dung de cai dat SSL tren cac may chu **Windows (IIS)**, Azure, hoac dung de di chuyen chung chi giua cac he thong khac nhau mot cach tien loi.
* **Bao mat:** Tep PFX luon yeu cau mot** mat khau bao ve (Password) de dam bao an toan tuyet doi cho Private Key ben trong.

#### 2. Cach chuyen doi tu CRT sang PFX

De chuyen doi thanh cong, ban can chuan bi du 3 thanh phan sau:
1.  **Certificate file:** `.crt` hoac `.cer`
2.  **Private Key file:** `.key` (Khoa rieng tao ra cung luc voi CSR).
3.  **CA Bundle file:** `.ca-bundle` hoac `.crt` (Chung chi trung gian).

---

#### **Cach 1: Su dung cong cu chuyen doi Online**
1. Truy cap cac trang web nhu *SSL Shopper* hoac *Sectigo SSL Converter*.
2. Chon loai chuyen doi: **PEM/CRT to PFX**.
3. Tai len (Upload) cac tep tuong ung:
    * **Certificate File:** File `.crt` cua ban.
    * **Private Key File:** File `.key` tuong ung.
    * **CA Certificate File:** File CA Bundle (neu co).
4. Nhap mat khau cho file PFX.
5. Nhan **Convert** va tai file `.pfx` ve may.

#### **Cach 2: Su dung lenh OpenSSL (Khuyen dung)**
Neu ban dang thao tac tren Linux, hay dung lenh sau de gop file nhanh chong va chuyen nghiep:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey private.key -in certificate.crt -certfile ca-bundle.crt




**Doamin**
- Domain là gì?
Domain (tên miền) là địa chỉ định danh duy nhất của một website trên internet, giúp người dùng truy cập trang web dễ dàng thay vì nhập dãy số IP phức tạp.

- Các trạng thái của domain
+ Active (Hoạt động): Tên miền đang hoạt động bình thường, đã được đăng ký và thanh toán đầy đủ.
+ ClientHold / ServerHold (Tạm khóa): Tên miền bị khóa, website/email không hoạt động, thường do quá hạn thanh toán hoặc tranh chấp.
+ ClientTransferProhibited / ServerTransferProhibited (Khóa chuyển đổi): Bảo mật tên miền, ngăn chặn việc chuyển đổi nhà đăng ký trái phép.
+ ClientUpdateProhibited / ServerUpdateProhibited (Khóa cập nhật): Ngăn chặn thay đổi thông tin tên miền (Nameserver, thông tin chủ thể).
+ PendingDelete (Chờ xóa): Tên miền đã hết hạn và không gia hạn, đang trong chu kỳ xóa vĩnh viễn (thường là 5 ngày sau khi kết thúc Redemption Period).
+ RedemptionPeriod (Chu kỳ chuộc): Tên miền đã hết hạn, website ngừng hoạt động nhưng vẫn có thể khôi phục với chi phí cao.
+ OK (Bình thường): Trạng thái cơ bản, tên miền không bị khóa và cho phép thực hiện các thao tác quản trị.

- Subdomain là gì?
Là một phần mở rộng cảu doamin chính, được tạo ra bằng cách thêm một tiền tố đứng trước tên miền chính ( root doamin) và năng cách bằng dấu chấm.

- Virtual Hosts là gì?
Là tính năng của phần mềm máy chủ Web (Apache, nginx) cho phép một máy chủ vật lý duy nhất lưu trữ và vận hành đồng thời cùng website hoặc tên miền khác nhau.

**Mail Server**
- Tìm hiểu MX Record.
là loại ghi quan trọng giúp định tuyến email đến máy chủ thư (mail serve) của tên miền. Nó cho phép thư điện tử thay mặt doamin, thường bao gồm tên máy chủ thư và mức độ ưu tiên
 Các thành phần chính MX record
 + mane/host: thường là @ hoặc tên miên chính (ví dụ: hihidomain.com)
 + Type: MX.
 + Mail Server/Destination: Địa chỉ máy chủ thư ( ví dụ: mail.hiihidoamin.com) 
 + Priority/Preference:  Số ưu tiên (thường là 0, 10, 20 ....) số càng nhỏ mức độ ưu tiên càng lớn
 + TTL (Time to Live): Thời gian sao lưu cache của bản ghi.

Chức năng và Tầm quan trọng
 + Định tuyến Email: Khi ai đó gửi mail đến info@yourdomain.com, Internet dùng MX record để tìm máy chủ chứa hộp thư này.
 + Dự phòng (Redundancy): Có thể thiết lập nhiều bản ghi MX với độ ưu tiên khác nhau. Nếu máy chủ chính (ưu tiên 0) ngừng hoạt động, mail sẽ được chuyển đến máy chủ phụ (ưu tiên 10).
 + Tránh mất dữ liệu: Đảm bảo email không bị trả lại khi máy chủ thư chính tạm ngưng hoạt động.

 - Tìm hiểu DKIM, SPF, PTR.
**SPF (Sender Policy Framework)
**
    + Định nghĩa: Là bản ghi DNS dạng TXT liệt kê các địa chỉ IP hoặc máy chủ được phép gửi email thay mặt cho tên miền của bạn.
    + Cơ chế: Khi nhận email, máy chủ đích (như Gmail, Yahoo) kiểm tra bản ghi SPF của tên miền gửi. Nếu IP gửi không nằm trong danh sách, email bị đánh dấu spam hoặc từ chối.
    + Lợi ích: Ngăn chặn kẻ gian mạo danh tên miền của bạn để gửi email giả mạo. 

**DKIM (DomainKeys Identified Mail)
**
  +   Định nghĩa: Phương thức xác thực email bằng chữ ký số, chứng minh email thực sự xuất phát từ tên miền của bạn và không bị thay đổi trên đường truyền.
  + Cơ chế: Email được ký bằng khóa bí mật (private key) trên server gửi và xác thực bằng khóa công khai (public key) lưu trong DNS.
  + Lợi ích: Đảm bảo tính toàn vẹn của nội dung email và tăng độ tin cậy đối với các bộ lọc spam.

** PTR Record (Pointer Record)
**
   + Định nghĩa: Bản ghi DNS ngược, ánh xạ địa chỉ IP ngược về tên miền (hostname).
   +  Cơ chế: Kiểm tra xem IP gửi email có thực sự liên kết với tên miền được khai báo hay không (Forward-Confirmed reverse DNS).
   + Lợi ích: Xác minh IP hợp pháp, giảm nguy cơ email bị các ISP (như Gmail) chặn vì lý do IP nghi vấn.

**Tóm tắt sự kết hợp
**Để tối ưu hóa khả năng gửi email (Email Deliverability), bạn nên cấu hình kết hợp cả 3:
    SPF: "Ai được phép gửi?"
    DKIM: "Nội dung có bị giả mạo không?"
    PTR: "IP gửi có hợp lệ không?"

**DNS**
Là quá trình phân giải tên miền, giúp chuyển đổi các tiên 
