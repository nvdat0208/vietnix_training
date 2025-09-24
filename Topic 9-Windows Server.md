## Thực hiện cho phép/chặn port trên windows
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
