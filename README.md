```text
1.**` SSL (Secure Sockets Layer)
**- SSL là gì?
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
Dinh nghia:** **PFX** (hay **PKCS #12**) la dinh dang tep dung de luu tru toan bo cac thanh phan cua mot chung chi bao mat (bao gom: Server Certificate, Private Key va CA Bundle) vao trong **mot tep duy nhat**.
* **Muc dich:** Thuong duoc su dung de cai dat SSL tren cac may chu **Windows (IIS)**, Azure, hoac dung de di chuyen chung chi giua cac he thong khac nhau mot cach tien loi.
* **Bao mat:** Tep PFX luon yeu cau mot** mat khau bao ve (Password) de dam bao an toan tuyet doi cho Private Key ben trong.

#### 2. Cach chuyen doi tu CRT sang PFX

De chuyen doi thanh cong, ban can chuan bi du 3 thanh phan sau:
1.  **Certificate file:** `.crt` hoac `.cer`
2.  **Private Key file:** `.key` (Khoa rieng tao ra cung luc voi CSR).
3.  **CA Bundle file:** `.ca-bundle` hoac `.crt` (Chung chi trung gian).


#### **Cach 1: Su dung cong cu chuyen doi Online**
1. Truy cap cac trang web nhu *SSL Shopper* hoac *Sectigo SSL Converter*.
2. Chon loai chuyen doi: **PEM/CRT to PFX**.
3. Tai len (Upload) cac tep tuong ung:
    * **Certificate File:** File `.crt` cua ban.
    * **Private Key File:** File `.key` tuong ung.
    * **CA Certificate File:** File CA Bundle (neu co).
4. Nhap mat khau cho file PFX.
5. Nhan **Convert** va tai file `.pfx` ve may.

 **Cach 2: Su dung lenh OpenSSL (Khuyen dung)**
Neu ban dang thao tac tren Linux, hay dung lenh sau de gop file nhanh chong va chuyen nghiep:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey private.key -in certificate.crt -certfile ca-bundle.crt```


2.**Doamin**
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

3**Mail Server**
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

4. ****DNS**
là hệ thống phân giải tên miền, giúp chuyển đổi các tên miền thân thiện (như vietnix.vn) thành địa chỉ IP máy tính hiểu được

- Cac loai record DNS (Ban ghi DNS) pho bien:

+ A Record (Address): Dung de tro ten mien ve mot dia chi IPv4 (vi du: 103.200.23.xx). Day la ban ghi quan trong nhat de website hoat dong.
+ AAAA Record: Tuong tu nhu A Record nhung dung de tro ten mien ve dia chi IPv6.
+ CNAME Record (Canonical Name): Cho phep mot ten mien khac lam bi danh (alias) for ten mien chinh (vi du: tro www.vietnix.vn ve vietnix.vn).
+ MX Record (Mail Exchange): Xac dinh may chu se chiu trach nhiem nhan email cua ten mien do.
+ TXT Record (Text): Dung de ghi chu hoac cung cap thong tin xac thuc cho cac dich vu ben ngoai (pho bien nhat la dung cho SPF va DKIM de bao mat email).
+ NS Record (Name Server): Chi dinh may chu DNS nao dang quan ly cac ban ghi cua ten mien.
+ PTR Record (Pointer): Con goi la DNS nguoc, dung de phan giai dia chi IP ve ten mien (nguoc lai voi A Record).
+ SOA Record (Start of Authority): Chua cac thong tin quan trong ve khu vuc DNS (DNS Zone) nhu email nguoi quan tri, thoi gian lam moi (refresh time)...

- Nguyen tac lam viec cua DNS (Domain Name System):
+ Buoc 1: Truy van tu trinh duyet (Query): Khi ban nhap mot ten mien (vi du: vietnix.vn) vao trinh duyet, may tinh se kiem tra trong bo nho dem (Cache) truoc. Neu khong co, no se gui yeu cau den DNS Resolver (thuong la cua ISP hoac Google 8.8.8.8).
+ Buoc 2: Truy van Root Server: DNS Resolver se hoi Root Name Server de biet may chu quan ly duoi mo rong (.vn, .com, .net...). Root Server se tra ve dia chi cua TLD Name Server tuong ung.
+ Buoc 3: Truy van TLD Server: Resolver tiep tuc gui yeu cau den may chu TLD (Top-Level Domain). Tai day, TLD Server se cung cap dia chi cua Authoritative Name Server - noi thuc su quan ly ban ghi cua ten mien vietnix.vn.
+ Buoc 4: Truy van Authoritative Server: Day la buoc cuoi cung. Authoritative Server se tra ve dia chi IP chinh xac cua website (vi du: 103.200.23.xx) cho Resolver.
+ Buoc 5: Tra ket qua va Caching: Resolver tra dia chi IP ve cho trinh duyet de tai website, dong thoi luu dia chi IP nay vao bo nho dem (Cache) trong mot khoang thoi gian (TTL) de su dung cho lan truy cap sau ma khong can lap lai quy trinh tren.

- Cach phan giai dia chi DNS:
+ Buoc 1: Truy van de quy (Recursive Query): Day la buoc dau tien khi may tinh cua nguoi dung gui yeu cau den DNS Resolver (thuong la IP 8.8.8.8 hoac IP cua nha mang). Resolver co trach nhiem di tim bang duoc dia chi IP cho nguoi dung.
+ Buoc 2: Truy van tuong tac (Iterative Query): DNS Resolver se thay mat nguoi dung thuc hien cac truy van lan luot qua cac cap may chu:
    + Hoi Root Server: De biet may chu quan ly duoi ten mien (vi du: .vn).
    + Hoi TLD Server: De biet may chu dang quan ly ten mien cu the (vi du: vietnix.vn).
    + Hoi Authoritative Server: De lay dia chi IP cuoi cung cua ban ghi A record.
+ Buoc 3: Luu vao bo nho dem (DNS Caching): Sau khi tim thay dia chi IP, ket qua se duoc luu lai tai DNS Resolver va tren may tinh cua nguoi dung (Local Cache). Thoi gian luu tru nay phu thuoc vao chi so TTL (Time To Live) trong cau hinh ban ghi.
+ Buoc 4: Tra ket qua ve trinh duyet: DNS Resolver gui dia chi IP ve cho trinh duyet. Trinh duyet su dung IP nay de thiet lap ket noi TCP/IP voi Web Server va hien thi noi dung website.
