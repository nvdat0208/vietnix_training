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
![Giao diện Nginx](/image/nginx.png)

### 3.2. Cài đặt máy chủ cơ sở dữ liệu
- Chạy lệnh sau để cài MariaDB
	```
	apt install mariadb-server mariadb-client -y
- Sau khi cài đặt xong NGINX bạn hãy khởi động và kích hoạt dịch vụ
	```
	systemctl enable mariadb
	systemctl start mariadb
	systemctl status mariadb
- Cấu hình MariaDB
	```
	mysql_secure_installation
- Nhập password root để đăng nhập, khi được yêu cầu thay đổi, nhập lại lần nữa
- Chọn yes ở tất cả các lựa chọn

  ![Giao diện mariadb](/image/mariadb.png)

  ![Giao diện mariadb](/image/mariadb2.png)

### 3.3. Cài đặt PHP
- Chạy lệnh sau để cài PHP8.1 và các extension cần thiết
	```
	apt install php8.1 php8.1-fpm php8.1-mysql php-common php8.1-cli php8.1-common php8.1-opcache php8.1-readline php8.1-mbstring php8.1-xml php8.1-gd php8.1-curl php8.1-soap php8.1-mbstring -y
- Sau khi cài đặt xong NGINX bạn hãy khởi động và kích hoạt dịch vụ
	```
	systemctl enable php8.1-fpm
	systemctl start php8.1-fpm
	systemctl status php8.1-fpm
 ### 3.4. Cài đặt phpMyAdmin
- Chạy lệnh sau để cài phpMyAdmin
	```
	apt install phpmyadmin -y
- Sau khi chạy lệnh, xuất hiện một hộp thoại chọn webserver để cài. Nếu không có webserver ở đây, chọn Tab di chuyển xuống chữ OK để cài tiếp.

![Giao diện mariadb](/image/phpmyadmin1.png)
- Sau đó việc cài đặt sẽ tiếp tục và xuất hiện thêm một hộp thoại với thông tin người dùng với tên phpmyadmin, chọn Yes

![Giao diện mariadb](/image/phpmyadmin2.png)
- Sau đó đặt mật khẩu cho phpmyadmin. nhập mật khâu xong, bấm tab để di chuyển xuống nút OK.

![Giao diện mariadb](/image/phpmyadmin3.png)
- Nhập lại mật khẩu một lần nữa để xác nhận.

![Giao diện mariadb](/image/phpmyadmin4.png)
- Quá trình cài đặt hoàn tất, một cơ sở dữ liệu mới có tên là phpmyadmin sẽ được tạo và người dùng cơ sở dữ liệu phpmyadmin có các đặc quyền cần thiết để quản lý cơ sở dữ liệu này. Đăng nhập vào MariaDB và kiểm tra những đặc quyền mà người dùng phpmyadmin được cấp.
	```
	mysql -u root
![Giao diện mariadb](/image/phpmyadmin5.png)
