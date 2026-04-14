```
** SSL (Secure Sockets Layer)
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
openssl pkcs12 -export -out certificate.pfx -inkey private.key -in certificate.crt -certfile ca-bundle.crt
```

```
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
```
```
3. **Mail Server**

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
```

```
**4. DNS**
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
```
# **Linux Command Line**
## - Ping vietnix.vn và giải thích kết quả lệnh `ping` và `hping3`.
<img width="696" height="269" alt="image" src="https://github.com/user-attachments/assets/f969870b-dbbe-473e-9ea3-f13669746eaf" />

<img width="801" height="310" alt="image" src="https://github.com/user-attachments/assets/024ec60a-d29c-4d1d-93be-f646ab49fd73" />

Giai thich cac thong so trong ket qua lenh Ping:

+ ttl (Time To Live): 
  - Nghia la "Vong doi" cua mot goi tin. Day la mot gia tri so nguyen (thuong bat dau tu 64, 128 hoac 255).
  - Moi khi goi tin di qua mot thiet bi mang (nhu Router), chi so nay se bi tru di 1 (goi la mot "hop").
  - Muc dich: Ngan chan viec goi tin bi lap vo han tren mang. Neu ttl giam ve bang 0 ma chua den duoc dich, goi tin se bi huy va thong bao cho nguoi gui.

+ time (ms):
  - Nghia la "Thoi gian phan hoi" (Latency hoac RTT - Round Trip Time).
  - Day la khoang thoi gian tinh tu luc may tinh ban gui goi tin ICMP di cho den khi nhan duoc goi tin phan hoi tu may chu quay ve.
  - Don vi tinh: Miligiay (ms). 
  - Y nghia: Chi so nay cang thap thi toc do ket noi mang den may chu cang nhanh va on dinh.

## - SSH Command:
### + Kết nối bằng Password:
 cài đặt openssh: sudo apt update && sudo apt install openssh-server -y
 Enable + start SSH
 <img width="819" height="311" alt="image" src="https://github.com/user-attachments/assets/767adc30-310c-4b67-8b00-cdb756be211c" />
tiến hành ssh bằng passwork: ssh username@ip_address
<img width="819" height="311" alt="image" src="https://github.com/user-attachments/assets/da5d53c5-dd75-4b41-aa48-e50ad54293a0" />

### + SSH bằng Key
Bước 1: Tạo cặp khóa trên máy cá nhân (Local).
Gõ lệnh: ssh-keygen -t rsa -b 2048
- Hệ thống sẽ hỏi nơi lưu file (thường là ~/.ssh/id_rsa), bạn cứ nhấn Enter.
- Hệ thống hỏi Passphrase (mật khẩu cho file khóa), có thể nhấn Enter để bỏ qua.
  <img width="807" height="492" alt="image" src="https://github.com/user-attachments/assets/622b116f-9d5e-4aa0-a7c5-6930cef7d6e8" />
  
  Hình trên bạn có thể thấy server có ghi đường dẫn lưu file private key (id_rsa) và file public key (id_rsa.pub)
  Tiếp theo ta cần thay đổi tên file “id_rsa.pub” thành “authorized_keys” bằng lệnh
mv /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys

Bước 2: Tiếp theo ta cần thay đổi tên file “id_rsa.pub” thành “authorized_keys” bằng lệnh
mv /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys

Bước 3: Kiểm tra kết nối 
 Lệnh: ssh root@localhost
 <img width="784" height="520" alt="image" src="https://github.com/user-attachments/assets/04513ff1-e07f-440a-a838-1fab9f484a20" />

### + Kết nối bằng port custom.
Bước 1:  Kiểm tra và Mở Port (Firewall)
<img width="506" height="115" alt="image" src="https://github.com/user-attachments/assets/c0d1f27d-b1c3-4fd7-8c8d-6ab6e6fd0c85" />

Bước 2: kiểm tra port đã hoạt động chưa
Kiểm tra port đang mở: Sử dụng lệnh ss -tulpn hoặc netstat -tulpn để xác nhận port đã hoạt động
<img width="925" height="434" alt="image" src="https://github.com/user-attachments/assets/e9c9cac4-4a7c-4887-b1b2-5033aef4d8f2" />

Bước 3: kiểm tra
<img width="925" height="434" alt="image" src="https://github.com/user-attachments/assets/416a8c23-a9f2-4412-aede-1dab837360b8" />

 ## SCP Command: 
 Lệnh SCP cho phép sao chép dữ liệu giữa máy cá nhân và máy chủ từ xa dựa trên nền tảng mã hóa của SSH.
  ###  + Copy 1 file.
  Sử dụng để sao chép một file duy nhất từ máy local lên server hoặc ngược lại.
  Cấu trúc lệnh
scp [đường_dẫn_file_nguồn] [username]@[IP_Server]:[đường_dẫn_đích]

Ví dụ thực tế
Sao chép file report.txt từ máy hiện tại lên thư mục /home/an/ của server:
scp report.txt an@192.168.0.223:/home/an/

  ### + Copy 1 folder
**Copy và giữ tên gốc
**Sử dụng khi bạn muốn sao chép file vào một thư mục khác mà không muốn thay đổi tên của nó.
Cấu trúc lệnh
cp [tên_file_nguồn] [đường_dẫn_thư_mục_đích]/
Ví dụ thực tế
Sao chép file report.txt vào thư mục Documents:
cp report.txt Documents/

**copy sang thư mục khác và đổi tên mới
**Nếu bạn muốn đưa file vào thư mục Documents nhưng với tên khác để dễ quản lý:
cp report.txt /home/an/Documents/ket_qua_bao_cao.txt

### + Copy thư mục
Các cách copy thư mục phổ biến trên Linux:

    Copy thư mục cùng tên tại vị trí mới:
    
    cp -r /path/to/source_folder /path/to/destination/

    Copy thư mục sang tên mới (Copy & Rename):

    cp -r /path/to/source_folder /path/to/new_folder_name

    Copy giữ nguyên thuộc tính file (Time, Owner, Permissions):
  
    cp -rp /path/to/source_folder /path/to/destination/

## - Rsync Command: 
Rsync là công cụ đồng bộ hóa dữ liệu chuyên nghiệp trong Linux, thường được sử dụng cho các tác vụ sao lưu (backup) nhờ khả năng kiểm tra sự khác biệt giữa nguồn và đích.
   ###  + Copy file.
   Cấu trúc lệnh
rsync [tham_số] [tên_file_nguồn] [đường_dẫn_đích]

Ví dụ thực hành trên máy cục bộ
Sao chép file report.txt vào thư mục Documents và giữ nguyên các thuộc tính của file:
rsync -v report.txt /home/an/Documents/
<img width="912" height="253" alt="image" src="https://github.com/user-attachments/assets/8cd6bdaf-84ea-4757-b2ce-0e31c217d71e" />

   ### + Copy folder.
   Cấu trúc lệnh
rsync [tham_số] [thư_mục_nguồn] [đường_dẫn_đích]

Ví dụ thực hành
Sao chép toàn bộ thư mục my_data vào thư mục Backup:
rsync -av my_data/ /home/an/Backup/

Giải thích các thành phần quan trọng

Tham số -a (Archive)
Đây là tham số quan trọng nhất khi copy thư mục. Nó là sự kết hợp của nhiều tham số khác giúp bảo toàn mọi thứ: sao chép đệ quy, giữ nguyên quyền hạn (permissions), giữ nguyên chủ sở hữu (owner), giữ nguyên các liên kết (symlinks) và thời gian chỉnh sửa của file gốc.

Tham số -v (Verbose)
Hiển thị danh sách các file đang được sao chép để người dùng dễ theo dõi.

Lưu ý cực kỳ quan trọng về dấu gạch chéo (/)

Trong Rsync, việc có hay không có dấu / ở cuối tên thư mục nguồn sẽ tạo ra kết quả hoàn toàn khác nhau:

Nếu có dấu gạch chéo: my_data/
Rsync sẽ chỉ copy nội dung bên trong thư mục my_data vào thư mục đích.
   
   ### + `rsync incremental`.
   Tạo bản sao lưu mới nhưng chỉ tốn thêm dung lượng cho các file có sự thay đổi so với bản cũ.
rsync -av --link-dest=[đường_dẫn_bản_cũ] [nguồn] [đường_dẫn_bản_mới]
Cơ chế này sử dụng "hard link" để liên kết các file không đổi với bản cũ, giúp tiết kiệm tối đa không gian ổ cứng nhưng vẫn đảm bảo mỗi bản sao lưu đều trông như một bản đầy đủ.

## - Cat Command:
   ### + Xem nội dung 1 file.
   Hiển thị toàn bộ nội dung của tập tin ra màn hình Terminal.
cat report.txt
Nếu muốn hiển thị kèm số dòng để dễ theo dõi, bạn có thể sử dụng thêm tham số -n: cat -n report.txt.
   <img width="912" height="253" alt="image" src="https://github.com/user-attachments/assets/9596e88d-be63-4712-aeec-c680d15ad729" />

   ### + Xem dòng thứ `<n>` trong file.
   Mặc định cat sẽ đọc hết file, nên để xem chính xác dòng thứ <n>, ta kết hợp với lệnh sed hoặc head/tail.

Sử dụng sed (khuyên dùng)
sed -n '<n>p' [tên_file]
Ví dụ xem dòng thứ 5 của file: sed -n '5p' report.txt
<img width="946" height="527" alt="image" src="https://github.com/user-attachments/assets/4a3a57ac-0c40-4051-801f-5f904260c2c5" />

Sử dụng head và tail
head -n <n> [tên_file] | tail -1
   ### + Ghi nhiều dòng vào 1 file bằng EOF.
   cat << EOF > report.txt
    Noi dung dong 1
  Noi dung dong 2
  Noi dung dong 3
  EOF
  <img width="946" height="527" alt="image" src="https://github.com/user-attachments/assets/bab67e5b-ea3b-478f-896e-75efa75a4447" />

##- Echo Command:
  ###  + Chèn thêm 1 dòng vào cuối file.
  Cấu trúc lệnh
  echo "Nội dung cần thêm" >> [tên_file]  
  Ví dụ thực tế
    Thêm dòng "Kết thúc báo cáo" vào cuối file report.txt:
    echo "Kết thúc báo cáo" >> report.txt
  <img width="937" height="344" alt="image" src="https://github.com/user-attachments/assets/e9453085-03e3-4d22-9299-0ede65b2e581" />

   ### + Overwrite nội dung file.
   Thao tác này sẽ xóa toàn bộ nội dung cũ đang có trong tập tin và thay thế bằng nội dung mới hoàn toàn.
   Cấu trúc lệnh
echo "Nội dung mới" > [tên_file]

Ví dụ thực tế
Làm mới lại toàn bộ file report.txt bằng một dòng tiêu đề duy nhất:
echo "Day là file moi" > report.txt
<img width="937" height="344" alt="image" src="https://github.com/user-attachments/assets/46a70f1f-cf8b-4182-b237-4369897afbcb" />

## - Tail/Head Command:
Đây là bộ đôi lệnh dùng để trích xuất nội dung ở phần đầu hoặc phần cuối của một tập tin, rất hữu ích khi làm việc với các file dữ liệu lớn hoặc file log.
   ### + Sự khác biệt giữa `tail` và `head`.
 Hai lệnh này có chức năng đối lập nhau về vị trí trích xuất dữ liệu.
**Lệnh head**
Dùng để xem những dòng đầu tiên của tập tin.
Mặc định sẽ hiển thị 10 dòng đầu.
Ví dụ xem 5 dòng đầu: head -n 5 report.txt
**Lệnh tail**
Dùng để xem những dòng cuối cùng của tập tin.
Mặc định sẽ hiển thị 10 dòng cuối.
Ví dụ xem 5 dòng cuối: tail -n 5 report.txt

   ### + Sự khác biệt giữa `tail` và `tailf`.
   Đây là sự khác biệt về cách thức theo dõi tập tin trong thời gian thực.

**Lệnh tail (Thông thường)**
Chỉ hiển thị các dòng cuối cùng tại thời điểm bạn chạy lệnh rồi kết thúc tiến trình. Nếu file có thêm dữ liệu mới sau đó, màn hình sẽ không cập nhật.

**Lệnh tailf (hoặc tail -f)**
Chữ f viết tắt của Follow. Lệnh này sẽ giữ cho kết nối luôn mở và theo dõi tập tin liên tục.
Khi có bất kỳ dòng mới nào được ghi vào file (ví dụ hệ thống đang ghi log), tailf sẽ ngay lập tức hiển thị dòng đó lên màn hình.
Để thoát khỏi chế độ theo dõi này, bạn nhấn phím Ctrl + C.

**Lưu ý chuyên sâu**
Hiện nay, lệnh tail -f được sử dụng phổ biến hơn vì nó có khả năng tiếp tục theo dõi ngay cả khi file bị xóa đi và tạo lại (nếu dùng tham số -F), trong khi tailf là lệnh cũ và ít tính năng hơn trên một số hệ điều hành mới.

## - Sed Command:
   ### + Find and replace string trong file.
   Sed là một công cụ mạnh mẽ dùng để biến đổi văn bản. Tính năng được sử dụng nhiều nhất của nó là tìm kiếm và thay thế các chuỗi ký tự mà không cần mở file trực tiếp.

Find and replace string trong file

Thao tác này cho phép bạn quét toàn bộ tập tin và thay thế một từ hoặc một cụm từ bằng một nội dung khác.

Cấu trúc lệnh cơ bản
_sed 's/[từ_cũ]/[từ_mới]/g' [tên_file]__
Ví dụ thực tế
Thay thế từ "Linux" thành "Ubuntu" trong file report.txt:
sed -i 's/hihi/haha/g' report.txt
<img width="937" height="344" alt="image" src="https://github.com/user-attachments/assets/23024904-b19e-4c48-9cfc-81dc5c3e5f13" />

**Giải thích các ký hiệu**
s (Substitute)
Thông báo cho sed rằng đây là thao tác thay thế.

**Dấu gạch chéo (/)**
Dùng để phân tách các thành phần trong lệnh. Bạn có thể thay đổi dấu này bằng các ký tự khác như # hoặc : nếu trong chuỗi văn bản của bạn đã có sẵn dấu /.

**g (Global)**
Tham số này rất quan trọng. Nếu không có g, sed chỉ thay thế từ đầu tiên mà nó tìm thấy ở mỗi dòng. Có g, hệ thống sẽ thay thế tất cả các từ khớp trong toàn bộ file.

**Lưu ý về việc ghi vào file (Tham số -i**)
Khi chạy lệnh trên, sed chỉ hiển thị kết quả đã thay thế ra màn hình chứ chưa lưu vào file gốc. Để áp dụng thay đổi và lưu trực tiếp vào file, bạn phải thêm tham số -i (in-place).


## - Traceroute/Tracert Command:
   ### + Thực hiện và giải thích kết quả
Lệnh này được sử dụng để theo dõi đường đi của các gói tin từ máy tính của bạn đến một địa chỉ máy chủ đích, giúp xác định xem kết nối bị chậm hoặc nghẽn ở phân đoạn nào.
Thực hiện lệnh
**Trên hệ điều hành Linux:**
traceroute google.com
<img width="928" height="722" alt="image" src="https://github.com/user-attachments/assets/8afb0ba4-5429-4e4c-9ff0-f2d9b002ad63" />

Phân tích kết quả Traceroute thực tế (23 Hops)

Dựa trên hình ảnh thực hành, chúng ta thấy lộ trình từ máy chủ đến Google dài hơn và có nhiều biến động về độ trễ.

Chặng đầu: Mạng nội bộ và Nhà mạng (Hop 1 - 6)
Gói tin đi qua Gateway 192.168.0.1 và các máy chủ trung gian của nhà mạng tại Việt Nam. Độ trễ bắt đầu tăng từ 50ms lên đến 145ms ở chặng số 6, dấu hiệu của việc dữ liệu bắt đầu đi vào các tuyến cáp quang biển hoặc nút thắt quốc tế.

Chặng giữa: Các nút thắt bảo mật và mất gói (Hop 7 - 8 và 14 - 22)
Tại các bước nhảy như 8 và từ 14 đến 22, hệ thống chỉ hiển thị dấu sao (* * *).
Giải thích: Điều này xảy ra khi các thiết bị định tuyến trung gian (Firewall hoặc Core Router) được cấu hình để ẩn danh hoặc không phản hồi các gói tin thăm dò nhằm bảo mật hệ thống. Tuy nhiên, gói tin vẫn được chuyển tiếp đi tiếp nên ta mới có kết quả ở bước nhảy cuối cùng.

Chặng cuối: Đích đến Google (Hop 23)
Tại bước nhảy thứ 23, gói tin cuối cùng đã chạm tới máy chủ của Google tại địa chỉ sf-in-f139.1e100.net (IP 74.125.24.139).
Độ trễ: Kết quả cuối cùng dao động từ 131ms đến 174ms. Đây là mức độ trễ trung bình khi truy cập các máy chủ đặt tại khu vực quốc tế (thường là Mỹ hoặc các trung tâm dữ liệu lớn của Google).

Kết luận kỹ thuật
Tổng cộng gói tin đã trải qua 23 bước nhảy.
Lộ trình có nhiều điểm chặn (dấu sao), nhưng kết nối vẫn thông suốt đến đích.


**Trên hệ điều hành Windows:**
tracert google.com

## - Netstat Command:
   ### + Hiển thị các socket đang listen.
   ### + Không resolve hostname.
   ### + Không resolve portname.
   ### + Display process name/PID.
   ### + Chỉ hiển thị socket TCP.
   ### + Chỉ hiển thị socket UDP.







  







   
   



