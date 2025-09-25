## Thực hiện cho phép/chặn port trên windows firewall
- Truy cập Server Manager - Local Server - Windows Firewall
![allow_port1](/image/allow_port1.png)
- Tại giao diện Windows firewall, chọn Advanced Settings
![allow_port2](/image/allow_port2.png)
- Bấm New Rule để tạo rule mới, Chọn Inbound Rules - New Rule - Port
![allow_port3](/image/allow_port3.png)
- Chọn Protocol là TCP và điền thông tin port vào mục Specific local port
![allow_port4](/image/allow_port4.png)
- Chỉ là action, ở đây chọn Allow the connection
![allow_port5](/image/allow_port5.png)
- Chọn profiles mà rule sẽ áp dụng lên
![allow_port6](/image/allow_port6.png)
- Đặt tên cho Rule mới, bấm Finish để tạo rule
![allow_port7](/image/allow_port7.png)
- Cấu hình chặn port tương tự như trên, ở mục Action ta sẽ chọn Block the connection
![allow_port8](/image/allow_port8.png)
## Thực hiện cho phép/ IP trên windows firewall
- Tương tự như cho phép port, ta tạo mới 1 rule, chọn Custom
![allow_ip1](/image/allow_ip1.png)

![allow_ip2](/image/allow_ip2.png)
- Ở mục Protol and ports, Protocol type chọn TCP, Local port chọn All ports, Remote port chọn Specific ports và số port vừa tạo rule ở trên
![allow_ip3](/image/allow_ip3.png)
- Mục Scope, chọn "Which remote IP addresses does this rule apply to", thêm IP mà ta cho phép truy cập
![allow_ip4](/image/allow_ip4.png)

![allow_ip5](/image/allow_ip5.png)

![allow_ip6](/image/allow_ip6.png)
- Mục Action, chọn Allow the connection
![allow_ip7](/image/allow_ip7.png)
- Chọn profiles mà rule sẽ áp dụng lên
![allow_ip8](/image/allow_ip8.png)
- Đặt tên cho rule và Finish để tạo
![allow_ip9](/image/allow_ip9.png)
- Tương tự, nếu chặn IP ta sẽ chọ Block the connection ở mục Action
![allow_ip10](/image/allow_ip10.png)
## Cài đặt web server IIS
- Vào Dashboard của Server Manager, chọn Add roles and features
![iis1](/image/iis1.png)
- Tiếp tục bấm Next cho tới mục Select server roles
![iis2](/image/iis2.png)

![iis3](/image/iis3.png)

![iis4](/image/iis4.png)
- Bấm chọn vào Web server(IIS), sau đó bấm Next đến Installation progress, chọn Install, chờ cài đặt
![iis5](/image/iis5.png)

![iis6](/image/iis6.png)

![iis7](/image/iis7.png)
## Cài đặt PHP
- Ta cài đặt thêm CGI và Management console để chạy PHP
![php1](/image/php1.png)
- Lên trang chủ PHP, tải phiên bản PHP7.4 (NTS) cho wordpress và giải nén ở ``C:\PHP\``
![php2](/image/php2.png)

![php3](/image/php3.png)
- Thêm PHP vào biến môi trường của windows
![php4](/image/php4.png)

![php5](/image/php5.png)

![php6](/image/php6.png)
- Thêm php-cgi trên ISS
![php7](/image/php7.png)

![php8](/image/php8.png)
## Cài đặt MySQL server
- Vào trang chủ MySQL, tải và cài đặt MySQL Server
![sql1](/image/sql1.png)

![sql2](/image/sql2.png)

![sql3](/image/sql3.png)

![sql4](/image/sql4.png)

![sql5](/image/sql5.png)

![sql6](/image/sql6.png)
