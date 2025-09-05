# Xây dựng mô hình LEMP, Wordpress, Laravel
## 1. Login SSH vào server/VPS.
	ssh root@[host]

## 2. Cập nhật hệ thống.
	apt-get update && apt-get upgrade -y

## 3. Thao tác cài đặt LEMP
### 3.1. Cài đặt NGINX
- Chạy lệnh sau để cài nginx
    ```
    apt-get install nginx -y
- Sau khi cài đặt xong NGINX bạn hãy khởi động và kích hoạt dịch vụ
    ```
  	systemctl enable nginx
  	systemctl start nginx
  	systemctl status nginx
- Mở port trên firewall cho phép truy cập web
    ```
  	iptables -I INPUT -p tcp --dport 80 -j ACCEPT
- Truy cập sẽ hiện giao diện mặc định của nginx
![Giao diện Nginx]()
