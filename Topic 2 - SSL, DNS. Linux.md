# Topic 2 - Nội dung tìm hiểu
## SSL
### Định nghĩa
SSL (Secure socket layer) là một giao thức bảo mật dựa trên mã hóa được sự dụng với mục đích đảm bảo riêng tư, xác thực và tính toàn vẹn dữ liệu.
Các website có triển khai SSL sẽ có HTTPS trong URL thay vì HTTP. SSL mã hóa dữ liệu được truyền qua giữa web và trình duyệt.
SSL cũng gán chữ ký số vào dữ liệu để cung cấp tính toàn vẹn dữ liệu để dữ liệu không bị giả mạo trước khi đến được người nhận.

### Xác thực SSL
Có 3 cách xác thực SSL:
- Xác thực tên miền: xác thực ít nghiêm ngặt và rẻ nhất, người yêu cầu chỉ cần chứng quyền kiểm soát domain của họ, phù hợp với blog cá nhân, trang web nhỏ và không xử lý dữ liệu nhạy cảm.
- Xác thực tổ chức: xác minh nghiêm ngặt hơn, dành cho các doanh nghiệp vừa và nhỏ, không chỉ cần chứng minh quyền kiểm soát domain mà còn cần thông tin của tổ chức như tên, địa chỉ, số điện thoại.
- Xác thực mở rộng: xác minh đáng tin cậy nhất, dành cho các doanh nghiệp lớn như ngân hàng, tài chính. tổ chức cung cấp SSL cần kiểm tra kỹ toàn bộ thông tin về doanh nghiệp trước khi cung cấp SSL.
CSR (Certificate Signing Request) là một file chứa thông tin mã hóa chứa thông tin mã hóa của chủ sở hữu domain và khóa công khai, được dùng để gửi yêu cầu cho bên cung cấp SSL. Trong CSR gồm các thông tin:
    - khóa công khai
    - domain
    - thông tin cá nhân hoặc tổ chức đăng ký SSL

### Cách tạo CSR và request SSL
Gen CSR và request SSL cho tech.training.vietnix.tech bằng OpenSSL
- Khởi tạo private key:

```mkdir domain```

```cd domain```

```openssl genrsa -out tech.training.vietnix.tech.key 2048```
- Khởi tạo CSR với private key

```openssl req -new -key tech.training.vietnix.tech.key -out domain.csr```
- Sau khi chạy lệnh tạo CSR, cung cấp các thông tin sau
	- Country Name (2 letter code)
	- State or Province Name (full name)
	- Locality Name (eg, city)
	- Organization Name (eg, company)
	- Organizational Unit Name (eg, section)
	- Common Name (eg, fully qualified host name)
	- Email Address
- Kết quả có 2 file: private key và csr
- Copy nội dung trong CSR và gửi cho bên cung cấp SSL

### File PEM
PEM (Privacy Enhanced Mail)là định dạng file thường được dùng để lưu trữ và truyền tải các chứng chỉ, khóa bảo mật. Ban đầu được sử dụng cho bảo email mật, sau này được sử dụng trong các cấu hình web server và các giao thức bảo mật như SSL/TLS. PEM thường có định dạng .pem, .crt, .cer, .key và được mã hóa bằng Base64. Định dạng này cho phép dữ liệu bảo mật dễ dàng trao đổi với nhau giữa các ứng dụng.

### Private key SSL
Private key SSL là một file văn bản chứa mã hóa được tạo ra trong quá trình tạo yêu cầu cấp chứng chỉ (CSR). Nó là một của cặp khóa bất đối xứng. Được dùng để giải mã thông tin được mã hóa bằng public key tương ứng, xác thực danh tính của server trong quá trình trao đổi dữ liệu.

### PFX và cách chuyển CRT sang PFX
PFX (Personal Information Exchange) là định dạng file được sử dụng để trao đổi và lưu trữ các chứng chỉ kỹ thuật số. Nó chứa public key và private key của chứng chỉ trong 1 file duy nhất và được bảo vệ bởi mật khẩu mạnh. Khi cần chuyển chứng chỉ từ 1 máy chủ sang máy chủ khác, chỉ cần xuất chứng chỉ ra dạng PFX mà không cần làm lại các bước tạo khóa và yêu cầu chúng chỉ.
Để chuyển CRT sang PFX, cần có: file crt, private key, file chuỗi chứng chỉ
- Chạy lệnh:
```openssl pkcs12 -export -out chung_chi.pfx -inkey private_key.key -in chung_chi.crt -certfile chuoi_chung_chi.crt```
- Sau khi chạy lệnh, nhập 1 mật khẩu cho PFX, mật khẩu này sẽ dùng để import PFX qua máy chủ khác.

## Domain
### Định nghĩa
Domain hay tên miền là địa chỉ của một website trên internet, được thể hiện bằng một chuỗi ký tự người dùng có thể đọc được và dễ nhớ. Domain xác định vị trí của website bằng cách ánh xạ với địa chỉ IP của web server. Người dùng có thể truy cập đến website dễ dàng bằng cách nhập domain vào trinh duyệt thay vì phải ghi nhớ dãy số của IP.

### Trạng thái tên miền
Trạng thái tên miền hay các mã EPP là các mã cho biết trạng thái hiện tại của tên miền và khả thực hiện các thao tác quản trị như gia hạn, chuyển đổi, cập nhật thông tin. các mã này được thiết lập bởi Registry (Đơn vị cấp phát tên miền) va Registra (Nhà đăng ký tên miền). Các trạng thái tên miền phổ biến:
- **OK/Active**: tên miền đang hoạt động bình thường, mọi dịch vụ gắn với tên miền đều hoạt động tốt.
- **clientTransferProhibited**: cấm chuyển đổi nhà đăng ký, giúp ngăn chặn việc chuyển đổi trái phép. Nếu muốn chuyển đổi, phải liên hệ với Nhà đăng ký và yêu cầu họ xóa bỏ trạng thái này.
- **clientUpdateProhibited**: Cấm cập nhật thông tin, điều này giúp ngăn chặn các cập nhật trái phép do gian lận. Nếu muốn chuyển đổi, phải liên hệ với Nhà đăng ký và yêu cầu họ xóa bỏ trạng thái này.
- **clientDeleteProhibited**: cấm xóa tên miền, găn chặn việc xóa trái phép do chiếm quyền điều khiển hoặc gian lận.
- **clientHold**: Nhà đăng ký tạm ngưng phân giải DNS của tên miền, lý do thường gặp là vi phạm chính sách sử dụng dịch vụ, chậm thanh toán phí...
- **serverHold**: Cơ quan quản lý tên miền cấp cao (Registry) đã tạm ngưng phân giải DNS, lý do thường gặp là vấn đề pháp lý, tranh chấp, hoạt động bất hợp pháp...
- **redemptionPeriod**: Tên miền đã hết hạn và rơi vào trạng thái cần đóng phí chuộc nếu muốn tiếp tục sử dụng. Thời gian này thường sẽ kéo dài 30 ngày. phải liên hệ với nhà đăng ký của mình để giải quyết trước khi tên miền của bạn bị xóa, sẽ phải đóng một khoản phí chuộc tên miền.
- **pendingDelete**: Tên miền hết hạn đăng ký, chuẩn bị xóa. Sau khi xóa, tên miền sẽ tự do, ai cũng có thể đăng ký lại.
- **pendingTransfer**: Có yêu cầu chuyển nhượng tên miền sang Nhà đăng ký khác. Nếu bạn không chuyển đổi tên miền của mình, hãy liên hệ với nhà đăng ký ngay lập tức để yêu cầu họ từ chối yêu cầu chuyển tên miền và đưa về trạng thái clientTransferProhibited.

### Subdomain
Subdomain là phần mở rộng của domain chính, dùng để chia một website thành các mục riêng biệt hoặc một website riêng độc lập về nội dung nhưng vẫn được quản lý dưới domain chính. Vì là một phần của domain nên việc tạo subdomain thường miễn phí và không giới hạn số lượng. Cấu trúc của subdomain là một thành phần được thêm vào phía trước domain chính và được ngăn cách bởi dấu chấm: [subdomain].[domain].[đuôi domain].

### Virtual host
Virtual host là một tính năng trên web server cho phép nhiều domain hoạt động trên một máy chủ và tất cả ánh xạ đến một địa chỉ IP duy nhất, tùy theo cách cấu hình, server sẽ có thể tự điều hướng request của người dùng đúng đến domain mà họ cần truy cập. Đây là giải pháp giúp tiết kiệm chi phí, tận dung tối đa tài nguyên của server và dễ dàng quản lý các domain.

## DNS
### Định nghĩa
DNS (Domain Name System) là hệ thống phân giải tên miền, dùng để chuyển đổi các tên miền website mà chúng ta đang sử dụng sang một địa chỉ IP dạng số tương ứng với tên miền đó và ngược lại. Khi người dùng nhập địa chỉ web trên trình duyệt, DNS sẽ tìm địa chỉ IP tương ứng với web và trả về kết quả hiển thị tương ứng của trang web cần tìm. Thay vì phải ghi nhớ địa chỉ IP, người dùng có thể dễ dàng truy cập các trang web thông qua tên miền.

### Các loại bản ghi DNS
- **CNAME**: canonical name, dùng để tạo ra tên miền bí danh (alias), cho phép bạn trỏ một tên miền này đến một tên miền khác. Nếu server có nhiều tên miền cùng trỏ về một IP, thay vì phải tạo thêm A record, ta có thể trỏ tên miền alias về tên miền chính. 
- **A record**: dùng để trỏ domain tới một địa chỉ IPv4.
- **AAAA record**: dùng để trỏ domain tới một địa chỉ IPv6.
- **MX record**: dùng để trỏ domain tới một mail server.
- **TXT record**: có thể thêm giá trị TXT, Host mới, TTL và Point To để chứa các thông tin định dạng văn bản domain.
- **SRV record**: bản ghi này dùng để xác định dịch vụ trên server tương ứng với port nào.
- **NS record**: chỉ định Name Server cho từng tên miền phụ và bên cạnh đó có thể tạo tên Name Server, TTL hay host mới.

### Nguyên tắc hoạt động
DNS được xây dựng theo một cấu trúc phân cấp, bao gồm nhiều loại máy chủ khác nhau, mỗi loại có một vai trò riêng biệt. Cấu trúc này giúp việc tìm kiếm địa chỉ IP diễn ra nhanh chóng và hiệu quả trên toàn cầu. Các máy chủ DNS chính bao gồm:
- **DNS Recursor**: tiếp nhận yêu cầu từ người dùng và thực hiện quá trình tìm kiếm thông tin. Nó liên tục truy vấn các máy chủ khác để tìm địa chỉ IP chính xác.
- **Root Nameserver**: Nó không chứa thông tin cụ thể của các tên miền, mà chỉ biết địa chỉ của các máy chủ TLD.
- **TLD (Top-Level Domain) Nameserver**: Máy chủ quản lý các domain cấp cao nhất như .com, .org, .vn... Nó biết máy chủ có thẩm quyền (Authoritative Nameserver) nào chịu trách nhiệm cho các tên miền cụ thể dưới quyền nó.
- **Authoritative Nameserver**: Máy chủ này chứa bản ghi DNS chính thức (A, CNAME, MX...) cho một tên miền và cung cấp địa chỉ IP.

### Cách DNS phân giải địa chỉ
- Khi người dùng truy cập một website, máy tính sẽ gửi yêu cầu đến máy chủ DNS cục bộ để tìm địa chỉ IP của website đó. Máy chủ DNS cục bộ sẽ kiểm tra cơ sở dữ liệu của nó, nếu có sẽ trả về địa chỉ IP cho máy tính của người dùng.
- Nếu DNS cục bộ không có địa chỉ IP của website, nó sẽ hỏi Root Nameserver. Root Nameserver sẽ trả về địa chỉ từ TLD Nameserver.
- DNS cục bộ tiếp tục hỏi TLD Nameserver. TLD sẽ trả về địa chỉ từ Authoritative Namesẻver.
- Tiếp tục hỏi Authoritative, nó sẽ tra cứu A record và trả về IP tương ứng domain. DNS cục bộ chuyển IP này cho máy tính để truy cập đến website.

## Mail Server
### Định nghĩa
MX Record (Mail Exchanger Record) là một loại bản ghi trong hệ thống DNS dùng để xác định máy chủ thư (mail server) chịu trách nhiệm nhận email cho một tên miền cụ thể. Với mỗi tên miền, người dùng có thể gán nhiều MX Record. Vì vậy, dù email tạm thời bị gián đoạn hoặc không hoạt động trong một thời gian, nhưng dữ liệu vẫn không hề bị mất. Các thành phần chính của một MX Record:
- Tên (Name): Tên miền mà bản ghi MX áp dụng.
- Loại (Type): Luôn là "MX".
Giá trị ưu tiên (Priority): Một số nguyên (càng nhỏ thì độ ưu tiên càng cao).
	- Một tên miền có thể có nhiều bản ghi MX với các giá trị ưu tiên khác nhau.
	- Hệ thống gửi email sẽ thử kết nối với máy chủ có độ ưu tiên thấp nhất trước (số nhỏ nhất).
	- Nếu máy chủ đó không phản hồi, nó sẽ chuyển sang máy chủ có độ ưu tiên cao hơn tiếp theo. Điều này giúp đảm bảo rằng email vẫn được gửi đi ngay cả khi máy chủ chính gặp sự cố.
- Máy chủ trao đổi (Exchange): Tên miền của máy chủ thư. Đây phải là một tên miền (chẳng hạn như mail.ten-mien.com), không phải là một địa chỉ IP. Tên miền này cũng phải có bản ghi A riêng để trỏ tới địa chỉ IP của máy chủ.

### DKIM, SPF, và PTR
DKIM, SPF, và PTR là ba cơ chế bảo mật quan trọng trong hệ thống email, giúp chống lại spam, lừa đảo (phishing), và giả mạo (spoofing). Mặc dù có chung mục đích, mỗi cơ chế lại hoạt động theo một cách khác nhau.
- **SPF** (Sender Policy Framework) là một bản ghi DNS loại TXT, xác định danh sách các máy chủ (địa chỉ IP) được phép gửi email từ một tên miền cụ thể. Khi một email được gửi đến, máy chủ nhận sẽ kiểm tra bản ghi SPF của tên miền gửi để xem địa chỉ IP của máy chủ gửi có nằm trong danh sách được cho phép hay không. Nếu địa chỉ IP không hợp lệ, email có khả năng bị đánh dấu là spam hoặc bị từ chối.
- **DKIM** (DomainKeys Identified Mail) là một phương thức xác thực email dựa trên chữ ký số. Nó sử dụng một cặp khóa mã hóa: một khóa riêng tư (private key) nằm trên máy chủ gửi email và một khóa công khai (public key) được công bố trong bản ghi DNS loại TXT của tên miền. Khi một email được gửi đi, máy chủ gửi sẽ sử dụng khóa riêng tư để tạo một chữ ký số cho email đó và đính kèm vào tiêu đề thư. Máy chủ nhận sẽ dùng khóa công khai để xác minh chữ ký.
- **PTR** (Pointer Record) là một loại bản ghi DNS ngược (reverse DNS). Trong khi các bản ghi DNS thông thường ánh xạ tên miền đến địa chỉ IP bản ghi PTR làm điều ngược lại: ánh xạ địa chỉ IP trở lại tên miền.

## Linux Command
### Ping và hping3
Giải thích kết quả của 2 lệnh
ping: 64 bytes from 103.90.224.90 (103.90.224.90): icmp_seq=1 ttl=53 time=3.73 ms
- 64 bytes from: phản hồi từ máy đích
- icmp_seq: thứ tự gói tin, bắt đầu bằng 1
- ttl: giá trị còn lại sau khi đã đi qua các hop
- time: thời gian round-trip của gói tin
hping3: len=46 ip=103.90.224.90 ttl=53 id=46341 icmp_seq=0 rtt=7.8 ms
- len=46: độ dài của gói tin
- ip=: địa chỉ máy đích
- ttl=: tương tự như lệnh pingm
- icmp_seq: tương tự lệnh ping, bắt đầu bằng 0
- rtt: round trip time của gói tin
Time trong ping là khoảng thời gian mà gói tin , đi từ máy tính đến máy chủ đích và quay trở lại. Dùng để đo độ trễ của kết nối (latency), thời gian càng thấp thì kết nối càng nhanh và ổn định, thời gian càng cao thì kết nối càng chậm và chập chờn.
TTL (time to live) là một giá trị được thiết lập trong header của gói tin ip. Khi có 1 gói tin gửi đi, nó sẽ được set 1 giá trị TTL tùy theo hệ điều hành (windows: 128, Linux: 64 hoặc 255). Mỗi khi qua một router, TTL sẽ giảm đi một. Điều này để tránh loop bên trong mạng, nếu TTL về bằng 0, router sẽ hủy gói tin và báo lỗi về cho máy tính. Giá trị TTL trong kết quả ping là giá trị còn lại của gói tin khi nó quay trở lại máy tinh.

### SSH
- kết nối băng password: mở terminal hoặc ssh client, cú pháp "ssh user@host -p 22"
- kết nối bằng key:
	- tạo một cặp khóa gồm public/private key, dùng terminal hoặc ssh client (putty).
	- sau khi có cặp khóa, public key bỏ vào file /root/.ssh/authorized_keys
	- thao tác nhập cú pháp kết nối ssh, không cần phải nhập mật khẩu
- kết nối bằng port custom
	- thay đổi số cổng trên file cấu hình của openssh trên server
	- mở port trên firewall của server cho phép kết nối ssh
	- nhập cú pháp và số cổng đã thay đổi cho dịch vụ ssh

### SCP
- copy 1 file: scp [user]@[host]:[file nguồn] [đường dẫn đích]
- copy 1 folder: scp -r [user]@[host]:[file nguồn] [đường dẫn đích]

### Rsync
- copy 1 file: rsync -avh [file nguồn] [file đích]
- copy 1 folder: rsync -avh [foleder nguồn] [foleder đích]
- Rsync incremental: cơ chế sao chép gia tăng của lệnh rsync. Cho phép rsync chỉ truyền tải những thay đổi hoặc những phần mới của file thay vì sao chép toàn bộ file từ đầu. Thay vì so sánh toàn bộ file, nó sử dụng một thuật toán gọi là "rolling checksum" để chia các file thành các khối dữ liệu nhỏ
	- Tính checksum: rsync tính toán checksum cho các khối dữ liệu của file đích.
	- So sánh: Nó gửi các checksum này đến máy chủ nguồn.
	- Truyền tải chênh lệch: Máy chủ nguồn so sánh checksum của mình với checksum nhận được. Khi phát hiện một khối dữ liệu không khớp, nó chỉ gửi những khối đã thay đổi hoặc những khối thiếu về máy đích.

### Cat
- Xem nội dung 1 file: cat [file]
- Xem dòng thứ [n] trong file: cú phát "cat -n [file]" để đánh số cho từng dòng  
- Ghi nhiều dòng vào 1 file bằng EOF: cat > [file] << EOF

### Echo
- Chèn thêm 1 dòng vào cuối file: echo [nội dung cần thêm] >> [file]
- Overwrite nội dung file: echo [nội dung ghi đè] > [file]

### Tail/Head
- Sự khác biệt giữa `tail` và `head: head hiển thị nội dung file từ 10 dòng đầu tiên, tail hiển thị 10 dòng cuối cùng trở lên
- Sự khác biệt giữa `tail` và `tailf`: tailf hiển thị các dòng cuối của file một cách liên tục, thường được dùng trong các file log để theo dõi khi có sự thay đổi trong hệ thống.

### Sed
- Find and replace string trong file: sed -i 's/string_cu/string_moi/' [file]

### Traceroute(linux)/ Tracert(windows)
traceroute to vietnix.vn (14.225.253.240), 64 hops max

  1   192.168.0.1  1,273ms  0,996ms  0,937ms
  
  2   27.71.251.149  3,395ms  3,217ms  2,651ms
  
  3   10.255.39.245  2,742ms  2,766ms  2,700ms
  
  4   *  *  * 
  
  5   27.68.236.34  4,841ms  2,627ms  2,525ms 
  
  6   203.113.187.98  2,219ms  2,359ms  2,259ms
  
  7   113.171.7.205  2,819ms  3,459ms  * 
  
  8   113.171.45.34  6,211ms  2,993ms  4,666ms 
  
  9   *  *  * 
  
 10   *  *  * 
 
 11   *  *  * 
 
 12   14.225.253.240  2,974ms  2,636ms  2,810ms
- Giải thích kết quả:
	+ máy tính sẽ gửi các gói tin qua các hop để dò đường đi. Ở gói tin đầu tiên có TTL=1
	+ sau khi đi qua đầu tiên TTL giảm =0, gói tin bị hủy, router gửi thông báo icmp time exceeded cho máy tính.
	+ máy tính ghi nhận ip từ thông báo, tiếp tục gửi TTL=2.
	+ quá trình lặp lại như cũ, đến khi máy tính ghi nhận ip của máy đích cuối cùng, ta sẽ có kết quả như trên.
	+ ở vài máy đích hiển thị dấu *, các nguyên nhân có thể xảy ra: firewall chặn ping, gói tin bị rớt...

### Netstat
- Hiển thị các socket đang listen: netstat -l
- Không resolve hostname/portname: netstat -n
- Display process name/PID: netstat -p
- Chỉ hiển thị socket TCP: netstat -t
- Chỉ hiển thị socket UDP: netstat -u

### Sort
- Theo thứ tự tăng dần: sort [file] // sort mặc định sắp xếp tăng dần 0-9, a-z
- Theo thứ tự giảm dần: sort -r [file]
- Theo column: sort -k [số cột] [file] // các cột phân tách nhau bằng khoảng trắng

### Uniq
- Lọc các dòng lặp lại: sort [file] | uniq
- Lọc và đếm số lượng dòng lặp lại: sort [file] | uniq -c

### Wc
- Đếm số dòng: wc -l [file]
- Đếm số ký tự: wc -m [file]

### Chmod, Chown, Chattr
- Sử dụng chmod để phân quyền bằng số và chữ
  
  ``read=4 hoặc r, write=2 hoặc w, executive=1 hoặc x, u=owner, g=group, o=other``
  
  ``chmod 755 file.txt`` : gán quyền rwx(7) cho chủ sở hữu file, quyền wx(5) cho group và other
  
  ``chmod u+rwx, go+rwx file.txt`` : tương tự như gán quyền bằng số, quyền rwx cho chủ sở hữu file, quyền wx cho group và other
- Sử dụng chown để thay đổi owner user/group cho tệp/thư mục

  ``chown [user] [file/thư mục] hoặc chown [user]:[group] [file/thư mục]``
- Sử dụng chattr để set immutable attribute - file sẽ không bị thay đổi, chỉnh sửa ngoại trừ root có thể thao tác
  ``chattr +i file.txt``

### Find Command:
- Tìm file đuôi `.log`: ``find *.log /var/log/`` tìm tất cả các file đuôi .log trong thư mục /var/log/
- Tìm folder tên `abc`: ``find /home/dat/ -type d -name "abc"``
- Tìm file tên `abc`: ``find /home/dat/ -name "abc"``
- Tìm file `abc` và đặt quyền read only: ``find /home/dat/ -name "abc" -exec chmod 444 {} +``

## Cp Command:
- Copy file: ``mv [tên/đường dẫn file][tên/đường dẫn của file mới]``
- Copy folder: ``mv [tên/đường dẫn file][tên/đường dẫn của file mới]``

## Mv Command:
- Di chuyển/đổi tên file/folder: ``mv [tên/đường dẫn của file][tên/đường dẫn mới của file]``
- Di chuyển/đổi tên file/folder: ``mv [tên/đường dẫn file hiện tại][tên/đường dẫn mới của file]``

## Cut Command:
- Lấy ký tự thứ `<n>`.
- Lấy từ ký tự `<n>` trở về sau.
- Lấy đến ký tự thứ `<n>`.

## Dig Command:
 - Kiểm tra record A, MX, NS.
 - Kiểm tra record A, MX, NS với custom DNS.

## Tar/Zip/Unzip Command:
- Nén/giải nén `tar.gz`.
- Nén/giải nén `.zip`.

## Mount/Umount Command:
- Thêm ổ cứng `sdb` ~ 5gb.
- Kiểm tra số lượng ổ cứng.
- Mount vào `/mnt/test`.
- Umount `/mnt/test`.

## Symbolic Links, Hard Links Command:
- Định nghĩa Sym Link.
- Định nghĩa Hard Link.
- Ví dụ về Sym Link và Hard Link.

## Ls Command:
- Liệt kê file/thư mục.: ``ls``
- Liệt kê file/thư mục và thuộc tính: ``ls -l hoặc ll``
- Show file ẩn: `ls -a``

## Ps Command:
- Show tiến trình: ``ps -e``
- Kill tiến trình: ``kill -9 PID``

## Top Command:
- Kiểm tra tài nguyên CPU: ``top``
```
op - 10:44:14 up  2:11,  1 user,  load average: 0,27, 0,50, 0,45
Tasks: 318 total,   1 running, 317 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0,7 us,  2,1 sy,  0,0 ni, 97,2 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st
MiB Mem :  15848,4 total,   9927,1 free,   2427,0 used,   3494,4 buff/cache
MiB Swap:   2048,0 total,   2048,0 free,      0,0 used.  12618,3 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   6631 dat       20   0   16084   4352   3584 R  12,5   0,0   0:00.02 top
      1 root      20   0  166964  11460   8132 S   0,0   0,1   0:02.33 systemd
      2 root      20   0       0      0      0 S   0,0   0,0   0:00.01 kthreadd
      3 root      20   0       0      0      0 S   0,0   0,0   0:00.00 pool_workqueue_release
      4 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker/R-rcu_g
      5 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker/R-rcu_p
```

- Giải thích các thông số:
	``PID``: ID của tiến trình.

	``USER``: Tên người dùng sở hữu tiến trình.

	``PR (priority)``: Mức độ ưu tiên của tiến trình.

	``S (status)``: Trạng thái của tiến trình (e.g., S = sleeping, R = running).

	``%CPU``: Phần trăm CPU mà tiến trình đang sử dụng.

	``%MEM``: Phần trăm RAM mà tiến trình đang sử dụng.

	``TIME+``: Tổng thời gian CPU mà tiến trình đã sử dụng.

	``COMMAND``: Tên của lệnh hoặc chương trình.

## Free Command:
- Giải thích các thông số về RAM: ``free -h``
```
               total        used        free      shared  buff/cache   available
Mem:            15Gi       2,3Gi       9,8Gi       467Mi       3,3Gi        12Gi
Swap:          2,0Gi          0B       2,0Gi
```

``Mem``: bộ nhớ RAM của server.

``Swap``: bộ nhớ swap, được lấy một phần từ ổ đĩa để dự phòng khi RAM đầy.

``total``: tổng dung lượng bộ nhớ.

``used``: dung lượng đã dùng.

``free``: dung lượng còn trống.

``shared``: dùng để nạp các thư viện hoặc dữ liệu mà được nhiều tiến trình sử dụng, khi nhiều tiến trình dùng chung dữ liệu, nó sẽ truy cập vào đây, điều này giúp tiết kiệm cho RAM.

``buff/cache``: dùng để lưu dữ liệu thường xuyên được truy cập, điều này giúp tăng tốc cho hệ thống.

``available``: là tổng của free và buff/cache, dùng để ước lượng cho việc cấp phát cho các ứng dụng mới mà không cần đến swap. Nếu available thấp, phải xem xét đén việc giải phóng RAM.
## Df Command:
- Xem dung lượng disk: ``df -h``
```
 Filesystem      Size  Used Avail Use% Mounted on
tmpfs           1,6G  2,3M  1,6G   1% /run
/dev/nvme0n1p5   98G   21G   72G  23% /
tmpfs           7,8G   40M  7,7G   1% /dev/shm
tmpfs           5,0M  4,0K  5,0M   1% /run/lock
efivarfs        192K   28K  160K  15% /sys/firmware/efi/efivars
/dev/nvme0n1p1   96M   37M   60M  39% /boot/efi
tmpfs           1,6G  9,4M  1,6G   1% /run/user/1000
```

- Phân vùng `/` là thư mục gốc của hệ điều hành Linux, tất cả mọi thứ của Linux như dữ liệu, chương trình, cấu hình đều nằm bên trong '/', nó quản lý hoàn toàn hệ thống thư mục của Linux. '/' có thể chiếm toàn bộ ổ đĩa hoặc chỉ một phần, nó là một phân vùng logic, các thư mục con của '/' có thể được mount đến các phân vùng khác với nó.
