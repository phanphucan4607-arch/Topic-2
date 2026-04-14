```
SSL (Secure Sockets Layer)
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
openssl req -new -key tech.training.vietnix.tech.key -out tech.training.vietnix.tech.csr

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
cat tech.training.vietnix.tech.csr

- Pem file là gì?
File PEM (Privacy Enhanced Mail) là một định dạng tệp phổ biến dựa trên mã hóa Base64, được sử dụng rộng rãi để lưu trữ và truyền tải dữ liệu mật mã như chứng chỉ SSL/TLS, khóa riêng tư (private key), khóa công khai (public key) và các chứng chỉ trung gian. Các tệp này thường có đuôi .pem, .crt, .cer, hoặc .key và được định nghĩa bởi các dòng "-----BEGIN..." và "-----END...".


- Private Key SSL là gì?
Private Key SSL (Khóa bí mật) là một tệp tin mã hóa quan trọng được tạo ra cùng lúc với CSR, có chức năng giải mã dữ liệu đã được mã hóa bởi Public Key (Khóa công khai) trong chứng chỉ SSL. Nó được máy chủ lưu trữ bảo mật tuyệt đối, không chia sẻ ra ngoài để đảm bảo an toàn cho phiên kết nối. 
Các đặc điểm và vai trò của Private Key SSL:
  +  Chức năng: Sử dụng thuật toán SSL (Secure Sockets Layer) toán bất đối xứng để giải mã thông tin từ trình duyệt (client) gửi đến máy chủ (server), thiết lập kết nối SSL/TLS an toàn.
  + Tạo ra: Private Key thường được tạo ra trên máy chủ cùng với CSR (Certificate Signing Request).
  +   Bảo mật: Private Key phải được giữ kín. Nếu mất, bạn phải thu hồi và đăng ký lại chứng chỉ SSL mới.
  + Sử dụng: Private key thường được lưu trữ dưới dạng tệp .key và cài đặt cùng với chứng chỉ SSL trên web server (Nginx, Apache). 
Khi cài đặt SSL, file Private Key (.key) sẽ hoạt động cùng với file Certificate (.crt) để xác thực và mã hóa dữ liệu trên website

- PFX file là gì? Cách chuyển từ CRT sang PFX.


  1. File PFX la gi?
Dinh nghia:** **PFX** (hay **PKCS #12**) la dinh dang tep dung de luu tru toan bo cac thanh phan cua mot chung chi bao mat (bao gom: Server Certificate, Private Key va CA Bundle) vao trong **mot tep duy nhat**.
* **Muc dich:** Thuong duoc su dung de cai dat SSL tren cac may chu **Windows (IIS)**, Azure, hoac dung de di chuyen chung chi giua cac he thong khac nhau mot cach tien loi.
* **Bao mat:** Tep PFX luon yeu cau mot** mat khau bao ve (Password) de dam bao an toan tuyet doi cho Private Key ben trong.

 2. Cach chuyen doi tu CRT sang PFX

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
   Chỉ lọc ra các dịch vụ đang ở trạng thái "LISTEN" (đang chờ kết nối), thay vì hiển thị toàn bộ các kết nối đang thiết lập.
   Lệnh: netstat -l
   <img width="928" height="722" alt="image" src="https://github.com/user-attachments/assets/331b2687-5074-4a97-8034-acfb47857ac8" />

   ### + Không resolve hostname.
   Yêu cầu hệ thống hiển thị địa chỉ dưới dạng số IP thuần túy thay vì cố gắng phân giải ra tên miền (như localhost hay google.com). Việc này giúp lệnh chạy nhanh hơn vì không phải chờ truy vấn DNS.
   ### + Không resolve portname.
   Hiển thị số cổng (như 22, 80, 443) thay vì tên dịch vụ (như ssh, http, https). Khi kết hợp, tham số -n sẽ áp dụng cho cả IP và Port.
   ### + Display process name/PID.
   Hiển thị tên chương trình và ID tiến trình (Process ID) đang chiếm giữ cổng đó. Lưu ý: Bạn cần chạy quyền sudo hoặc root để thấy được thông tin này của các tiến trình hệ thống.
   ### + Chỉ hiển thị socket TCP.
   Lọc danh sách và chỉ hiển thị các kết nối sử dụng giao thức TCP.
Lệnh: netstat -tnl
<img width="774" height="298" alt="image" src="https://github.com/user-attachments/assets/7f075e5c-6711-4193-bb63-9d2c1c1478e6" />

   ### + Chỉ hiển thị socket UDP.
   Lọc danh sách và chỉ hiển thị các kết nối sử dụng giao thức UDP (thường dùng cho các dịch vụ như DNS, DHCP).
Lệnh: netstat -unl
<img width="774" height="298" alt="image" src="https://github.com/user-attachments/assets/540665e2-a995-40ca-bd03-8ce00b9db13d" />

## - Sort Command:
 dùng để sắp xếp dòng văn bản. Mặc định là tăng dần theo bảng chữ cái/ASCII.
   ###  + Theo thứ tự tăng dần.
   Theo thứ tự tăng dần (mặc định): sort filename.txt.
   <img width="666" height="467" alt="image" src="https://github.com/user-attachments/assets/6de5a43f-a08b-4596-bca6-73b376b75e4f" />

   ### + Theo thứ tự giảm dần.
   sort -r filename.txt.'
   
   <img width="666" height="467" alt="image" src="https://github.com/user-attachments/assets/52bce2d3-0ed6-4229-b98b-b67fcf8c01d9" />
   
   ### + Theo column.
   Sử dụng sort -k [số_cột] filename.txt.

    Ví dụ: sort -k 2 data.txt (sắp xếp dựa trên nội dung cột thứ 2).
    
<img width="666" height="467" alt="image" src="https://github.com/user-attachments/assets/33d01fd7-24ca-43ab-97b3-1c25f0439456" />


## - Uniq Command:
công cụ mạnh mẽ dùng để lọc, báo cáo hoặc loại bỏ các dòng lặp lại liền kề trong tệp văn bản. Lệnh này thường được kết hợp với sort để đạt hiệu quả cao nhất.

   ### + Lọc các dòng lặp lại.
   Để loại bỏ các dòng trùng lặp liền kề và chỉ giữ lại một dòng đại diện, bạn sử dụng lệnh uniq. 

    Cú pháp: sort report.txt | uniq
    Ví dụ: Nếu file report có các dòng trùng nhau, dùng lệnh sau để lọc:
    
   ### + Lọc và đếm số lượng dòng lặp lại.
   Để đếm số lần xuất hiện của mỗi dòng (kèm theo hiển thị dòng đó), sử dụng tùy chọn -c. 
    Cú pháp: uniq -c [tệp_đầu_vào]
    Ví dụ: 
    <img width="412" height="176" alt="image" src="https://github.com/user-attachments/assets/08b7b994-296f-4cf8-976c-a4728cafe7ef" />


    sort data.txt | uniq -c
<img width="412" height="176" alt="image" src="https://github.com/user-attachments/assets/e8f1e566-bab2-4ecf-a19e-06626aaa851b" />

Các tùy chọn phổ biến khác (theo Vietnix):

    -d: Chỉ hiển thị các dòng trùng lặp (lọc ra các dòng xuất hiện > 1 lần).
    -u: Chỉ hiển thị các dòng duy nhất (chỉ xuất hiện 1 lần).
    -i: Bỏ qua phân biệt chữ hoa, chữ thường.
    -f n: Bỏ qua
    trường đầu tiên khi so sánh.

## - Wc Command:
à công cụ hiệu quả để đếm số dòng, từ và ký tự trong tệp. Các tùy chọn chính bao gồm -l để đếm số dòng và -m hoặc -c để đếm số ký tự/byte
   ### + Đếm số dòng.
   Sử dụng tùy chọn -l.
    Cú pháp: wc -l tên_tệp
    Ví dụ: wc -l file.txt (Hiển thị số dòng và tên tệp).
    <img width="412" height="176" alt="image" src="https://github.com/user-attachments/assets/8a3b9316-0aa3-4473-aae9-a9759e3e500f" />

   ### + Đếm số ký tự.
   Sử dụng tùy chọn -m.
    Cú pháp: wc -m tên_tệp
    Ví dụ: wc -m file.txt (Hiển thị tổng số ký tự, bao gồm cả khoảng trắng và ký tự xuống dòng).

<img width="363" height="183" alt="image" src="https://github.com/user-attachments/assets/a1f921ca-04da-4b60-8893-f98dca256c09" />


## - Chmod, Chown, Chattr Command:
Lệnh này thay đổi quyền (read-r=4, write-w=2, execute-x=1) cho User (u), Group (g), Others (o)

   ### + Phân quyền bằng số và chữ.
   **Phân quyền bằng số** (Numeric Method): Dùng 3 chữ số đại diện cho u, g, o.

    chmod 755 file.txt: User có toàn quyền (7=4+2+1), Group/Others chỉ được đọc và thực thi (5=4+0+1).
    chmod 644 file.txt: User đọc/ghi (6), Group/Others chỉ đọc (4).
   chmod 777 file.txt: Tất cả mọi người có toàn quyền (rất nguy hiểm).
   **Phân quyền bằng chữ** (Symbolic Method):

    chmod u+x file.txt: Thêm quyền thực thi cho User.
    chmod g-w file.txt: Xóa quyền ghi của Group.
    chmod o+r file.txt: Thêm quyền đọc cho Others.

Tham số -R: Thay đổi quyền cho toàn bộ thư mục và tệp con bên trong

   ### + Đổi owner user/group.
Lệnh dùng để thay đổi người sở hữu (user) và nhóm (group) của file/folder. 
    Chỉ đổi user: chown newuser file.txt
    Đổi cả user và group: chown newuser:newgroup file.txt
    Đổi owner cho toàn bộ thư mục (recursive): chown -R user:group /path/to/folder
    
   <img width="594" height="312" alt="image" src="https://github.com/user-attachments/assets/bd37bee4-a80b-4f82-8a96-f66e9aea9733" />

   ### + Set Immutable Attribute.
  Lệnh chattr dùng để thay đổi các thuộc tính mở rộng của tệp, đặc biệt là +i (immutable) để bảo vệ tệp 
  Đây là mức độ bảo mật cao hơn cả phân quyền thông thường. Khi một file được thiết lập thuộc tính này, không ai (kể cả root) có thể xóa, đổi tên hoặc sửa đổi nội dung của nó.
  **Lệnh thiết lập (Set)**
Sử dụng tham số +i để kích hoạt tính năng bất biến.
chattr +i report.txt

**Lệnh gỡ bỏ (Unset)**
Khi muốn chỉnh sửa lại file, bạn phải gỡ bỏ thuộc tính này bằng tham số -i.
chattr -i report.txt
Kiểm tra xác nhận bằng lsattr

lsattr report.txt

  <img width="554" height="126" alt="image" src="https://github.com/user-attachments/assets/492305a2-52ee-4a3c-ac07-eeb408dab40e" />

==> ------i--- có nghĩa là file được bảo vệ

thực hiện xóa và nó báo không có quyền 

<img width="554" height="126" alt="image" src="https://github.com/user-attachments/assets/213cdf61-c60a-47f3-b57a-4c8947998e26" />


 ## - Find Command:
 Câu lệnh find trên Linux để thực hiện các yêu cầu của bạn, được tối ưu hóa cho 
   ### + Tìm file đuôi `.log`.
   Tìm kiếm tất cả các tập tin có phần mở rộng là .log trong thư mục hiện tại và các thư mục con.
```
find . -type f -name "*.log"
```
Tham số -type f xác định đối tượng tìm kiếm là tập tin (file).

   ### + Tìm folder tên `abc`.
   
  Tìm kiếm thư mục có tên chính xác là abc.
```
find . -type d -name "abc"
```
Tham số -type d xác định đối tượng tìm kiếm là thư mục (directory)
   
   ### + Tìm file tên `abc`.
   ```
find . -type f -name "abc"
 ```
   ### + Tìm file `abc` và đặt quyền read only.
   Lệnh find có thể kết hợp tham số -exec để xử lý ngay lập tức kết quả tìm thấy.
Ví dụ: Tìm file abc và đặt quyền thực thi (755) cho nó:
```
find . -type f -name "abc" -exec chmod 755 {} \;
```
**Giải thích ký hiệu đặc biệt trong -exec**
{}: Là ký hiệu đại diện cho từng file mà lệnh find tìm thấy.
;: Là ký hiệu báo hiệu kết thúc câu lệnh thực thi đi kèm.

## - Cp Command:
   ### + Copy file.
   Sao chép file tại cùng thư mục (đổi tên):
```
cp file_goc.txt file_dich.txt
```
Sao chép file sang thư mục khác:
```
cp file_goc.txt /path/to/folder/
```
Sao chép nhiều file vào một thư mục:
```
cp file1.txt file2.txt /path/to/folder/
```
   ### + Copy folder.
 Để sao chép thư mục, bạn bắt buộc phải dùng tùy chọn -r (recursive - đệ quy) hoặc -a (archive - giữ nguyên thuộc tính). 

Sao chép thư mục và toàn bộ nội dung bên trong

```
    cp -r folder_goc/ folder_dich/
```
Sao chép thư mục và giữ nguyên quyền, thời gian (thường dùng để backup):

```
    cp -a folder_goc/ folder_dich/
```

## - Mv Command:
   ### + Di chuyển/đổi tên file/folder.
Lệnh mv được sử dụng để di chuyển tập tin/thư mục từ vị trí này sang vị trí khác, hoặc dùng để đổi tên chúng. Khác với cp, lệnh mv sẽ không để lại bản sao ở vị trí cũ.

Đổi tên file hoặc thư mục
Nếu bạn chỉ định đích đến là một tên mới trong cùng một thư mục, hệ thống sẽ thực hiện việc đổi tên.
Lệnh thực hiện:
```
mv ten_cu.txt ten_moi.txt
```

Lệnh này áp dụng tương tự cho cả thư mục: mv folder_cu folder_moi.

Di chuyển file hoặc thư mục
Nếu đích đến là một đường dẫn thư mục khác, tập tin sẽ được chuyển hẳn sang vị trí đó.
Lệnh thực hiện:
```
mv report.txt /home/an/documents/
```

Sau khi chạy lệnh, file report.txt tại thư mục cũ sẽ không còn tồn tại.

Di chuyển và đổi tên cùng lúc
Bạn có thể kết hợp cả hai hành động trong một câu lệnh duy nhất bằng cách chỉ định đường dẫn mới kèm tên mới.

Lệnh thực hiện:
```
mv report.txt /home/an/backup/report_final.txt
```
**Các tham số an toàn quan trọng**

-i (Interactive): Hỏi xác nhận trước khi ghi đè nếu ở đích đến đã có một file trùng tên. Điều này cực kỳ quan trọng để tránh mất dữ liệu ngoài ý muốn.
mv -i file.txt /destination/

-n (No-clobber): Ngăn chặn việc ghi đè lên file đã tồn tại. Nếu file đích đã có, lệnh sẽ không thực hiện gì cả.

-f (Force): Ngược lại với -i, lệnh này sẽ ghi đè ngay lập tức mà không đưa ra bất kỳ cảnh báo nào.

## - Cut Command:
Lệnh cut được sử dụng để cắt các phần cụ thể từ mỗi dòng của tập tin hoặc kết quả đầu ra của một lệnh khác. Khi làm việc với ký tự, chúng ta sử dụng tham số -c
   ### + Lấy ký tự thứ `<n>`.
   Chỉ trích xuất duy nhất một ký tự tại vị trí thứ n (tính từ 1).
Lệnh thực hiện:
```
cut -c 1 rceport.txt
```
<img width="562" height="172" alt="image" src="https://github.com/user-attachments/assets/c77d2f80-3fb3-4e6b-bf91-b2052b0f1184" />
==> kết quả sẽ trả về chữ đầu tiên ở mỗi dòng 
  
   ### + Lấy từ ký tự `<n>` trở về sau.
   Lệnh này giúp loại bỏ một số lượng ký tự nhất định ở đầu dòng và giữ lại toàn bộ phần nội dung còn lại cho đến cuối dòng.

   ```
  cut -c n- <tên_file>
```
Ví dụ thực tế với file của bạn:
Giả sử dòng văn bản là dw (2 ký tự).
Nếu bạn chạy: cut -c 2- report.txt
Hệ thống sẽ bỏ ký tự thứ 1, và lấy từ ký tự thứ 2 trở đi. Kết quả trả về sẽ là chữ
<img width="562" height="172" alt="image" src="https://github.com/user-attachments/assets/ac648dd8-50ef-484c-be89-400cc8027578" />

   ### + Lấy đến ký tự thứ `<n>`.
   Lệnh này giúp bạn trích xuất một chuỗi ký tự tính từ vị trí đầu tiên (vị trí 1) cho đến điểm dừng là ký tự thứ n mà bạn mong muốn.
   (Lưu ý: Dấu gạch ngang phải đặt trước con số).

Ví dụ thực tế với file của bạn:
Giả sử dòng văn bản trong file là LinuxServer.
Nếu bạn muốn lấy 5 ký tự đầu tiên, bạn chạy:

```
cut -c -5 report.txt
```

<img width="562" height="172" alt="image" src="https://github.com/user-attachments/assets/968f9390-1472-42ea-8152-cc8af1b659eb" />


## - Dig Command:
Lệnh dig là công cụ mạnh mẽ nhất trên Linux để vấn tin các máy chủ DNS. Nó giúp bạn kiểm tra các bản ghi (records) của tên miền để xác định cấu hình mạng hoặc khắc phục sự cố phân giải tên miền.
   ### + Kiểm tra record A, MX, NS.
   Mỗi loại bản ghi mang một thông tin khác nhau về tên miền:
    Record A (Address): Trả về địa chỉ IP của tên miền.
    Record MX (Mail Exchange): Trả về máy chủ xử lý email của tên miền đó.
    Record NS (Name Server): Trả về các máy chủ quản lý DNS của tên miền.

  Lệnh thực hiện:
  
dig google.com A

<img width="789" height="631" alt="image" src="https://github.com/user-attachments/assets/6f8073a7-e42b-4678-8c93-9a212c881483" />


dig google.com MX

<img width="741" height="530" alt="image" src="https://github.com/user-attachments/assets/c2efcb7f-ce9b-4d30-905a-0d5cb4e28255" />

dig google.com NS

<img width="741" height="530" alt="image" src="https://github.com/user-attachments/assets/bb6343f0-533a-4c69-aca7-7967348506e8" />


### + Kiểm tra record A, MX, NS với custom DNS.
Đôi khi, DNS của nhà mạng có thể bị cập nhật chậm hoặc bị chặn. Để kiểm tra xem bản ghi thực tế trên một máy chủ DNS cụ thể (như Google 8.8.8.8 hoặc Cloudflare 1.1.1.1) là gì, bạn sử dụng ký hiệu

Lệnh thực hiện:
```
dig @8.8.8.8 google.com A
dig @1.1.1.1 google.com MX
dig @8.8.4.4 google.com NS
```

## - Tar/Zip/Unzip Command:
   ###  + Nén/giải nén `tar.gz`.
 **Nén thư mục/tệp tin:**
Sử dụng lệnh tar với các tham số -cvzf.
tar -cvzf archive.tar.gz folder_name/
(Trong đó: c là create, v là verbose (hiện tiến trình), z là gzip, f là file).

**Giải nén:**
Sử dụng tham số -xvzf.
tar -xvzf archive.tar.gz
(Trong đó: x là extract).

   
   ### + Nén/giải nén `.zip`.
   Định dạng .zip có tính tương thích cao, dễ dàng mở được trên cả Windows và macOS mà không cần cài thêm phần mềm.

**Nén thư mục/tệp tin:**
Sử dụng lệnh zip. Nếu nén thư mục, bạn bắt buộc phải có tham số -r (recursive).
zip -r data.zip folder_name/

**Giải nén:**
Sử dụng lệnh unzip.
unzip data.zip

## - Mount/Umount Command:
Việc gắn kết (Mount) là quá trình làm cho hệ thống tập tin trên các thiết bị lưu trữ (như ổ cứng, USB) có thể truy cập được từ cây thư mục của Linux.

   ### + Thêm ổ cứng `sdb` ~ 5gb.
Sau khi hệ thống nhận diện được ổ cứng vật lý, ta cần khởi tạo nó để có thể lưu trữ dữ liệu.
    Bước 1: Phân vùng ổ đĩa (Partitioning)
    fdisk /dev/sdb (Sau đó nhấn n để tạo mới, p cho phân vùng chính, và w để lưu).
    Bước 2: Định dạng hệ thống tập tin (Formatting)
    Để Linux đọc được, ta định dạng theo chuẩn ext4:
    mkfs.ext4 /dev/sdb1
   
   ### + Kiểm tra số lượng ổ cứng.
Trước khi thực hiện bất kỳ thao tác nào, chúng ta cần liệt kê các thiết bị lưu trữ đang kết nối với máy chủ.
    Lệnh thực hiện: lsblk
    Chi tiết: Lệnh này hiển thị danh sách các thiết bị khối (block devices) theo dạng cây, giúp xác định rõ tên ổ cứng (như sdb) và các phân vùng đi kèm.
   
   ### + Mount vào `/mnt/test`.
Để truy cập và ghi dữ liệu vào ổ cứng mới (ví dụ /dev/sdb), chúng ta cần thực hiện "Mount" nó vào một thư mục rỗng.
    Tạo điểm gắn kết (Mount Point): mkdir -p /mnt/test
    Thực hiện lệnh: mount /dev/sdb /mnt/test
    Kiểm tra kết quả: Sử dụng lệnh df -h để xác nhận ổ cứng đã được gắn thành công vào thư mục /mnt/test.
   
   ### + Umount `/mnt/test`.
   Khi cần tháo ổ đĩa hoặc bảo trì, chúng ta thực hiện ngắt kết nối để đảm bảo an toàn cho dữ liệu.
    Lệnh thực hiện: umount /mnt/test
    Lưu ý: Nếu hệ thống báo lỗi target is busy, hãy đảm bảo rằng không có cửa sổ terminal nào đang đứng trong thư mục /mnt/test hoặc có ứng dụng nào đang mở file tại đó.
    | Mục tiêu | Lệnh thực thi | Ý nghĩa |
| :--- | :--- | :--- |
| **Liệt kê ổ đĩa** | `lsblk` | Xem danh sách tất cả ổ cứng đang có. |
| **Gắn kết** | `mount /dev/sdb /mnt/test` | Kết nối ổ `sdb` vào thư mục `/mnt/test`. |
| **Ngắt kết nối** | `umount /mnt/test` | Tháo ổ đĩa ra khỏi hệ thống tập tin. |
| **Xem trạng thái** | `df -h` | Kiểm tra dung lượng và các điểm đã mount. |



## - Symbolic Links, Hard Links Command:
   ## Symbolic Links và Hard Links

---

### Định nghĩa Symbolic Link (Symlink)

Symbolic Link (liên kết mềm) là một file đặc biệt trỏ đến một file hoặc thư mục khác thông qua đường dẫn.

Đặc điểm:

* Hoạt động giống shortcut trong Windows
* Có thể trỏ đến file hoặc thư mục
* Nếu file gốc bị xóa → link sẽ bị lỗi (broken link)
* Có thể trỏ sang phân vùng khác

---

### Định nghĩa Hard Link

Hard Link là một liên kết trực tiếp đến dữ liệu của file trên ổ đĩa.

Đặc điểm:

* Trỏ trực tiếp đến inode của file
* Không thể phân biệt file gốc và hard link
* Nếu xóa file gốc → file vẫn tồn tại nếu còn hard link
* Không tạo được cho thư mục
* Không dùng được khác phân vùng

---

### Ví dụ Symbolic Link

**Tạo symlink:**

```bash id="1x2n0k"
ln -s file_goc.txt symlink.txt
```

Kiểm tra:

```bash id="0xw4n6"
ls -l
```

Kết quả:

```bash id="h7b2qk"
symlink.txt -> file_goc.txt
```

---

### Ví dụ Hard Link

Tạo hard link:

```bash id="7d3m5p"
ln file_goc.txt hardlink.txt
```

Kiểm tra inode:

```bash id="q2z9lx"
ls -li
```

 Hai file sẽ có cùng inode

### So sánh nhanh

| Tiêu chí | Symbolic Link (Soft Link) | Hard Link |
| :--- | :---: | :---: |
| **Kiểu liên kết** | Theo đường dẫn (Path) | Theo mã Inode |
| **Dùng cho folder** | Có | Không |
| **Khác phân vùng** | Có | Không |
| **Xóa file gốc** | Link bị lỗi (Broken) | Vẫn dùng bình thường |
| **Lệnh thực hiện** | `ln -s nguồn đích` | `ln nguồn đích` |

### Kết luận

* Symbolic Link linh hoạt, dễ dùng
* Hard Link an toàn hơn vì không phụ thuộc file gốc

## - Ls Command:
   | Tham số | Lệnh thực thi | Ý nghĩa |
| :--- | :--- | :--- |
| **Cơ bản** | `ls` | Liệt kê tên file và thư mục |
| **Chi tiết** | `ls -l` | Hiển thị thuộc tính, quyền hạn và kích thước |
| **Tất cả** | `ls -a` | Hiển thị cả các file ẩn (bắt đầu bằng dấu `.`) |
| **Kết hợp** | `ls -la` | Hiển thị chi tiết tất cả các file bao gồm cả file ẩn |
| **Dễ đọc** | `ls -lh` | Hiển thị kích thước file theo đơn vị dễ đọc (KB, MB, GB) |

## - Ps Command:
   ### + Show tiến trình.

   Khi một tiến trình bị treo hoặc bạn muốn tắt nó, bạn sẽ dùng lệnh kill kết hợp với PID lấy từ lệnh ps ở trên.
    Tắt tiến trình một cách "lịch sự":
    ệnh ps giúp bạn chụp lại trạng thái các tiến trình đang chạy tại thời điểm thực hiện lệnh.

    Hiển thị tất cả tiến trình đang chạy trên hệ thống
    ps -aux
        -a: Hiển thị tiến trình của tất cả người dùng.
        -u: Hiển thị chi tiết (User, CPU, RAM...).
        -x: Hiển thị các tiến trình không gắn với terminal (chạy ngầm).
    
   ## + Kill tiến trình.
kill -9 <PID>
Khi một tiến trình bị treo hoặc bạn muốn tắt nó, bạn sẽ dùng lệnh kill kết hợp với PID lấy từ lệnh ps ở trên

## - Top Command:
   ### + Kiểm tra tài nguyên CPU.
   Kiểm tra kiến truc CPU trong linux với lscpu: $ lscpu

   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/38ce1ee9-918f-44ff-93b5-7134b1fbb422" />
   
   ### + Giải thích các thông số.
1. Thông số cốt lõi (Core Architecture)

    Architecture: x86_64: Đây là kiến trúc 64-bit phổ biến nhất hiện nay, cho phép hệ thống quản lý lượng RAM lớn (vượt xa mức 4GB của kiến trúc 32-bit).

    CPU op-mode(s): 32-bit, 64-bit: CPU này có khả năng tương thích ngược, chạy được cả các phần mềm cũ 32-bit và phần mềm hiện đại 64-bit.

    Address sizes: 48 bits physical, 48 bits virtual: Khả năng đánh địa chỉ bộ nhớ. Với 48-bit physical, về lý thuyết CPU có thể nhận tới 256 TB RAM vật lý.

2. Cấu trúc phân lớp (Topology)

Đây là phần quan trọng nhất để hiểu cách CPU vận hành:

    CPU(s): 12: Tổng số đơn vị xử lý mà hệ điều hành nhìn thấy (nhân logic).

    Thread(s) per core: 2: Mỗi nhân vật lý có 2 luồng xử lý (SMT - Simultaneous Multithreading).

    Core(s) per socket: 6: Có 6 nhân vật lý nằm trên một đế chip.

    Socket(s): 1: Máy chỉ có 1 ổ cắm CPU (thường là laptop hoặc PC phổ thông).

        Công thức tính: 1 Socket×6 Cores×2 Threads=12 CPUs.

3. Tốc độ và Hiệu năng

    CPU max MHz: 4390.4141: Tốc độ tối đa khi ép xung tự động (Turbo Boost) là khoảng 4.4 GHz.

    CPU min MHz: 423.1730: Khi máy rảnh, nó sẽ giảm xung xuống cực thấp (khoảng 423 MHz) để tiết kiệm pin và giảm nhiệt.

    BogoMIPS: 4591.73: Một chỉ số đo tốc độ tính toán sơ bộ của Linux (thường dùng để tham khảo độ "trâu" của chip khi khởi động).

4. Bộ nhớ đệm (Caches - Sum of all)

Phần này cho thấy tổng dung lượng bộ nhớ đệm trên toàn bộ chip:

    L1d / L1i (192 KiB): Bộ nhớ đệm cấp 1 (dữ liệu và lệnh). Đây là bộ nhớ nhanh nhất, nằm sát nhân CPU nhất.

    L2 (3 MiB): Tốc độ trung bình, dung lượng lớn hơn L1.

    L3 (16 MiB): Bộ nhớ đệm dùng chung cho tất cả các nhân. L3 lớn giúp xử lý đa nhiệm và chơi game mượt mà hơn.

5. Ảo hóa (Virtualization)

    Virtualization: AMD-V: CPU này hỗ trợ công nghệ ảo hóa phần cứng của AMD. Nếu bạn dùng VMware, VirtualBox hoặc Docker, tính năng này bắt buộc phải có để đạt hiệu suất cao.

## - Free Command:
   ### + Giải thích các thông số về RAM.

   <img width="885" height="159" alt="image" src="https://github.com/user-attachments/assets/e562f0b2-da53-4a05-886d-ba914bb5ee3b" />
 Phân tích số của bạn
total: 23,937,784 KB ≈ ~22.8 GB RAM
used: ~3.7 GB
free: ~13.4 GB
buff/cache: ~5.4 GB
available: ~19.1 GB

 Điều này có nghĩa:

Máy bạn đang dùng rất ít RAM (~3.7GB)
Linux tận dụng ~5.4GB làm cache để tăng tốc
Nhưng vẫn còn ~19GB có thể dùng ngay khi cần

→ Hệ thống đang rất dư RAM, không có vấn đề gì.

## - Df Command:
   ### + Xem dung lượng disk.
dùng lệnh df để kiểm tra dung lượng đĩa cứng ssax sử dụng và còn trống của các phân vùng trên hệ thống

```
df -h
```

<img width="873" height="345" alt="image" src="https://github.com/user-attachments/assets/5887c62a-0dc0-4cd3-8b07-b743092b7b97" />

  
   ### + Phân vùng `/` là gì.  
Trong Linux, phân vùng / được gọi là Root Directory (Thư mục gốc). Đây là điểm bắt đầu của toàn bộ hệ thống tệp tin.

    Tất cả điều nằm ở đây: Khác với windows phân chia ổ C: D: E:, trong linux, mọi thư mục (/etc, /bin, /home, /var...) và mọi ổ cứng khác khi được gắn vào máy đều phải nằm "dưới" thư mục góc /.
    Tầm quan trọng: Nếu phân vùng / bị đàyf (100%), hẹ điều hành sẽ gặp lỗi nghiêm trọng: không thể khởi động, không thể đăng nhập hoặc các dịch vụ (web, Database) sẽ ngừng hoày động vì không thể ghi tiệp tạm.
    
    





   
   



