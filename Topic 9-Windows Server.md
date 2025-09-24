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
