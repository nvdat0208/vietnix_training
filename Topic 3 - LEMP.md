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

## 4. Cài đặt Wordpress
- Tải WordPress và giải nén:
	```
	cd /tmp
	wget https://wordpress.org/latest.tar.gz
	tar -xzvf latest.tar.gz
	sudo mv /tmp/wordpress /var/www/wordpress
- Chỉnh sửa quyền sở hữu thư mục để Nginx có thể truy cập
  	```
	chown -R www-data:www-data /var/www/wordpress
	chmod -R 755 /var/www/wordpress/
- Tạo file cấu hình Nginx cho WordPress
  	```
	nano /etc/nginx/sites-available/wordpress
- Dán nội dung sau vào file:
```
server {
    listen 80;
    listen [::]:80;
    server_name your_domain.com www.your_domain.com; # Thay thế bằng tên miền hoặc IP của bạn
    root /var/www/wordpress;
    index index.php index.html index.htm;
    location / {
        try_files $uri $uri/ /index.php?$args;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }
}
```

- Kích hoạt site và khởi động lại Nginx:
	```
	ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
	nginx -t # Kiểm tra cú pháp
	systemctl restart nginx
- Truy cập web sẽ hiện giao diện của Wordpress
![Giao diên wordpress](/image/wordpress.png)

## 5. Cài đặt Laravel
### 5.1. Cài đặt Composer
- Chạy lệnh sau để cài đặt
	```
	apt install composer -y
### 5.2. Cài đặt Laravel
- Tạo dự án Laravel mới
	```
	cd /var/www
	composer create-project --prefer-dist laravel/laravel vietnix_training
	chown -R www-data:www-data /var/www/vietnix_training
	chmod -R 775 /var/www/vietnix_training/storage
### 5.3. Cấu hình Nginx cho Laravel
- Tạo file cấu hình Nginx mới cho Laravel:
	```
	nano /etc/nginx/sites-available/laravel
- Dán nội dung sau vào file:
```
server {
    listen 80;
    listen [::]:80;
    server_name laravel.datnguyen.vietnix.tech; # Thay thế bằng tên miền hoặc IP của bạn
    root /var/www/vietnix_training/public; # Sửa lại đường dẫn

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php index.html index.htm;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
}
```

- Kích hoạt site và khởi động lại Nginx:
	```
	ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/
	nginx -t
	systemctl restart nginx
- Truy cập web sẽ hiện giao diện của Laravel
![Giao diện Laravel](/image/laravel.png)

# 6. Cài SSL cho 2 domain với ZeroSSL
## 6.1. Đăng ký tài khoản ZeroSSL
- Truy cập www.sslforfree.com để đăng ký 1 tài khoản
![Giao diện ssl1](/image/ssl1.png)

## 6.2. Tạo chứng chỉ SSL
- Chọn New certificates
- Validity: chọn thời hạn 90 ngày
- CSR & Contact: chọn Auto-Generate để hệ thống tự tạo CSR
- Domains: nhập domain. sau đó bấm Next Step
![Giao diện ssl1](/image/ssl2.png)

## 6.3. Xác thực tên miền
- Chọn xác thực bằng file upload
- Tải Auth file từ trang web
- Lưu Auth file vào thư mục /.well-known/pki-validation/, thư mục này sẽ bên trong thư mục source web
- Vào trình duyệt, gõ domain + thư mục /.well-known/pki-validation/ để xem Auth file có đang đặt đúng vi trí
![Giao diện ssl1](/image/ssl3.png)

![Giao diện ssl1](/image/ssl4.png)

## 6.4. Tải xuống chứng chỉ
- Sau khi xác thực hoàn tất.
![Giao diện ssl1](/image/ssl5.png)

## 6.5. Cài đặt chứng chỉ trên hosting
- Sau khi xác thực, bấm Download Certificate để tải chứng chỉ cài đặt SSL cho domain
![Giao diện ssl1](/image/ssl6.png)

- Sau khi tải về máy tính, giải nén ra 3 file: ca_bundle.crt, certificate.crt, private.key
![Giao diện ssl1](/image/ssl7.png)

- Dùng scp để tải 3 file đó lên server, trên server tạo thư mục /etc/ssl/lavarel để chứa các file cert
	```
 	scp ca_bundle.crt certificate.crt private.key root@103.90.226.73
 - Thay đổi quyền private.key để chỉ root mới có thể đọc được
	```
 	chmod 600 /etc/ssl/lavarel/private.key
 - Thay đổi cấu hình file cho việc cài SSL
	- thay đổi port sang 443
	- chỉnh cấu hình port 80 sẽ tự chuyển hướng qua 443
   	- thêm các đường dẫn file cert và các cấu hình ssl
```
server {
    listen 80;
    listen [::]:80;
    server_name laravel.datnguyen.vietnix.tech;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name laravel.datnguyen.vietnix.tech; # Thay thế bằng tên miền hoặc IP của bạn
    # Chỉ thị SSL
    ssl_certificate /etc/ssl/laravel/certificate.crt;
    ssl_certificate_key /etc/ssl/laravel/private.key;
    ssl_trusted_certificate /etc/ssl/laravel/ca_bundle.crt;

    # Cài đặt SSL nâng cao
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256';
    ssl_prefer_server_ciphers on;

    root /var/www/vietnix_training/public; # Sửa lại đường dẫn

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php index.html index.htm;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
}
```
- Mở port trên firewall cho phép SSL
    ```
  	iptables -I INPUT -p tcp --dport 443 -j ACCEPT
- Sau khi lưu file, hãy kiểm tra cú pháp để đảm bảo không có lỗi và khởi động lại dịch vụ Nginx.
	```
 	sudo nginx -t
	sudo systemctl restart nginx
- Truy cập vào website để kiểm tra
![Giao diện ssl1](/image/ssl8.png)

- Thao tác cấu hình SSL tương tự cho Worldpress
# 7. Tạo tài khoản FTP
- Cài đặt vsftp
	```
 	apt install vsftpd -y
 - Mở port cho phép dịch vụ ftp
   	```
    iptables -I INPUT -p tcp --dport 21 -j ACCEPT
- Cấu hình ftp cơ bản
	```
 	nano /etc/vsftpd.conf
 ``listen=YES``
 
``listen_ipv6=YES``
 
``anonymous_enable=NO``
 
``local_enable=YES``
 
``write_enable=YES``
 
``chroot_local_user=YES``

- Lưu file và khởi động lại dịch vụ
	```
 	systemctl restart vsftpd
- Tạo tài khoản ftp và phân quyền
```
useradd ftp_dat
usermod -d /var/www/dat/ ftp_dat
chown -R ftp_dat:ftp_dat /var/www/dat/
chmod -R 755 /var/www/dat/
```
# 8. Bật remote Mysql cho root trên MariaDB
- Đăng nhập vào MariaDB với tư cách người dùng root
```
mysql -u root -p
```

- Tạo một người dùng root mới, ở đây là root_remote
```
CREATE USER 'root_remote'@'%' IDENTIFIED BY 'P@ssW0rd';
```

- Cấp quyền cho root_remote
```
GRANT ALL PRIVILEGES ON *.* TO 'root_remote'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

- Thoát khỏi MariaDB bằng lệnh ``exit``
