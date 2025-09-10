# Xây dựng mô hình reverse proxy kết hợp nginx và apache

## 1. Cấu hình Apache (Backend Reverse Proxy)
### 1.1. Cài đặt Apache

	``apt install apache2``
- Khai báo port 8080 và 8443 để không trùng với Nginx
	```
	Listen 8080

	<IfModule ssl_module>
        Listen 8443
	</IfModule>

	<IfModule mod_gnutls.c>
        Listen 8443
	```
- Khởi động dịch vụ Apache
	```
	systemctl enable apache2
	systemctl start apache2
	```
	
### 1.2. Cấu hình PHP-FPM và mod_fastcgi cho Apache
- Cài đặt php-fpm và các module FastCGI Apache
	```
	apt install php-fpm
	wget https://mirrors.edge.kernel.org/ubuntu/pool/multiverse/liba/libapache-mod-fastcgi/libapache2-mod-fastcgi_2.4.7~0910052141-1.2_amd64.deb
	dpkg -i libapache2-mod-fastcgi_2.4.7~0910052141-1.2_amd64.deb
	```
- Apache phục vụ các trang PHP bằng cách sử dụng mod_php theo mặc định, để hoạt động với PHP-FPM bạn cần cấu hình một số chi tiết.

- Thêm khối cấu hình cho mod_fastcgi, khối này phụ thuộc vào mod_action. Theo mặc định, mod_action sẽ bị tắt, hãy bật nó lên

	``a2enmod actions``

- Backup file fastcgi gốc và tạo 1 file mới
	```
	mv /etc/apache2/mods-enabled/fastcgi.conf /etc/apache2/mods-enabled/fastcgi.conf.defaultvi
	vi /etc/apache2/mods-enabled/fastcgi.conf
	```
- Thêm nội dung cho file vừa tạo
	```
	<IfModule mod_fastcgi.c>
	  AddHandler fastcgi-script .fcgi
	  FastCgiIpcDir /var/lib/apache2/fastcgi
	  AddType application/x-httpd-fastphp .php
	  Action application/x-httpd-fastphp /php-fcgi
	  Alias /php-fcgi /usr/lib/cgi-bin/php-fcgi
	  FastCgiExternalServer /usr/lib/cgi-bin/php-fcgi -socket /run/php/php8.1-fpm.sock -pass-header Authorization
	  <Directory /usr/lib/cgi-bin>
	    Require all granted
	  </Directory>
	</IfModule>
	```
- Kiểm tra cú pháp và reload lại apache
	```
	apache2ctl configtest
	systemctl restart apache2
	```

- Xác minh lại chức năng của PHP, tạo 1 file default virtual host
	``vi /etc/apache2/sites-available/vhost-default.conf``
- Thêm nôị dung sau
	```
	<VirtualHost *:8080>
	    ServerName default-server
	    DocumentRoot /var/www/html
	
	    <Directory /var/www/html>
	        AllowOverride All
	        Require all granted
	    </Directory>
	
	    # Bạn có thể thêm một tệp index.html đơn giản
	    DirectoryIndex info.php
	</VirtualHost>
	```
- Lưu file, kiểm tra cú pháp, khởi động lại apache2
	```
	apache2ctl configtest
	systemctl restart apache2
	```
- Truy cập http://ip:8080/info.php để đảm bảo php hoạt động trên Apache
  ![Giao diện mariadb](/image/PHP.png)

### 1.3. Tạo virtual host cho Apache
- Tạo ra 2 file index.html và info.php trong thư mục /var/www/domain/ của 2 domain cho mục đích kiểm tra cấu hình
	```
	# domain của Wordpress
	echo "<h1 style='color: green;'>Wordpress</h1>" | tee index.html
	echo "<?php phpinfo();?>" | tee info.php
	```
	```
	# domain của Laravel
	echo "<h1 style='color: orange;'>Laravel</h1>" | tee index.html
	echo "<?php phpinfo();?>" | tee info.php
- Tạo virtual host cho wordpress
	``/etc/apache2/sites-available/vhost-laravel.conf``
- Nhập vào nội dung sau:
	```
	<VirtualHost *:8080>
	    ServerName laravel.datnguyen.vietnix.tech
	    ServerAlias www.laravel.datnguyen.vietnix.tech
	    DocumentRoot /var/www/vietnix_training/public/
	    # Các cấu hình khác cho WordPress (vd: RewriteEngine On)
	    ErrorLog ${APACHE_LOG_DIR}/domain1_error.log
	    CustomLog ${APACHE_LOG_DIR}/domain1_access.log combined
	</VirtualHost>
	
	<VirtualHost *:8443>
	    ServerName laravel.datnguyen.vietnix.tech
	    DocumentRoot /var/www/vietnix_training/public/
	
	    SSLEngine on
	    SSLCertificateFile /etc/ssl/laravel/certificate.crt
	    SSLCertificateKeyFile /etc/ssl/laravel/private.key
	
	    <Directory /var/www/vietnix_training/public/>
	        AllowOverride All
	        Require all granted
	    </Directory>
	</VirtualHost>
	```
- Tạo virtual Host cho Laravel
	``/etc/apache2/sites-available/vhost-wordpress.conf``
	```
	<VirtualHost *:8080>
	    ServerName wp.datnguyen.vietnix.tech
	    ServerAlias www.wp.datnguyen.vietnix.tech
	    DocumentRoot /var/www/wordpress/public_html
	    # Cấu hình Laravel, trỏ tới public/
	    <Directory /var/www/domain2/public_html>
	        AllowOverride All
	        Require all granted
	    </Directory>
	    ErrorLog ${APACHE_LOG_DIR}/domain2_error.log
	    CustomLog ${APACHE_LOG_DIR}/domain2_access.log combined
	</VirtualHost>
	
	<VirtualHost *:8443>
	    ServerName wp.datnguyen.vietnix.tech
	    DocumentRoot /var/www/wordpress/
	
	    SSLEngine on
	    SSLCertificateFile /etc/ssl/wordpress/certificate.crt
	    SSLCertificateKeyFile /etc/ssl/wordpress/private.key
	
	    <Directory /var/www/domain1/wordpress/>
	        AllowOverride All
	        Require all granted
	    </Directory>
	</VirtualHost>
	```
- Kích hoạt các vhost
	```	
	a2ensite vhost-laravel.conf
	a2ensite vhost-wordpress.conf  
	```
- Khởi động lại Apache ``systemctl restart apache2``
- Khỏi động module cho SSL ``a2enmod ssl``
- Truy cập 2 domain qua cổng 8080 để xem kết quả

  ![Giao diện mariadb](/image/check_apache.png)
 
  ![Giao diện mariadb](/image/check_php.png)
## 2. Cấu hình Nginx (Frontend Reverse Proxy)
### 2.1. Cấu hình các virtual host cho Nginx
- Xóa liên kết tượng trưng của server ảo mặc định vì sẽ không sử dụng nó nữa:
	``rm /etc/nginx/sites-enabled/default``
- Tạo thư mục root cho cả hai trang web:
	``mkdir -v /usr/share/nginx/wordpress /usr/share/nginx/laravel``
- Tương tự với Apache, hãy tạo các tệp index và phpinfo() để kiểm tra sau khi hoàn thành cài đặt
	```
	echo "<h1 style='color: blue;'>Nginx wordpress 1</h1>" | sudo tee /usr/share/nginx/wordpress/index.html
    echo "<h1 style='color: orange;'>Nginx laravel 1</h1>" | sudo tee /usr/share/nginx/laravel/index.html
    echo "<?php phpinfo(); ?>" | sudo tee /usr/share/nginx/wordpress/info.php
    echo "<?php phpinfo(); ?>" | sudo tee /usr/share/nginx/laravel/info.php
	```

- Tạo file virtual host cho 2 domain
	```
	vi /etc/nginx/sites-available/nginx1.your_domain
	vi /etc/nginx/sites-available/nginx1.your_domain
	```
- Thêm nội dung vào file
	```
	server {
    listen 80 default_server;

    root /usr/share/nginx/wordpress;
    index index.php index.html index.htm;

    server_name wp.datnguyen.vietnix.tech;
    
    location / {
        try_files $uri $uri/ /index.php;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        include snippets/fastcgi-php.conf;
   	 }
	}
	```	
	```
	server {
    listen 80 default_server;

    root /usr/share/nginx/laravel;
    index index.php index.html index.htm;

    server_name laravel.datnguyen.vietnix.tech;
    
    location / {
        try_files $uri $uri/ /index.php;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        include snippets/fastcgi-php.conf;
   	 }
	}
	```	
- Kích hoạt các Virtual Hosts:
	```
	ln -s /etc/nginx/sites-available/vhost-laravel.conf /etc/nginx/sites-enabled/
	ln -s /etc/nginx/sites-available/vhost-wordpress.conf /etc/nginx/sites-enabled/
	systemctl restart nginx
	```
- Truy cập 2 domain theo http://domain/info.php để kiểm tra
![Giao diện php](/image/check_php_nginx.png)
## 2.2. Cấu hình Nginx cho các virtual server của Apache
- Tạo file cấu hình cho proxy nginx ``vi/etc/nginx/sites-available/proxy``
- Thêm đoạn code sau vào file, đoạn code này chỉ định domain của các virtual server trong Apache và chuyển tiếp yêu cầu của chúng đến Apache.
	```
	server {
    listen 80;
    server_name laravel.datnguyen.vietnix.tech wp.datnguyen.vietnix.tech;

    location / {
        proxy_pass http://103.90.226.73:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    		}
	}
	```
- Lưu file, tạo symbolic link, kiểm tra cú pháp và reload nginx
	```
	ln -s /etc/nginx/sites-available/proxy /etc/nginx/sites-enabled/proxy
	nginx -t
	systemctl restart nginx
	```
- Truy cập http://domain/info.php , Kéo xuống phần biến số PHP và kiểm tra các giá trị:SERVER_SOFTWARE và DOCUMENT_ROOT xác nhận rằng yêu cầu này đã được xử lý bởi Apache. Các biến số HTTP_X_REAL_IP và HTTP_X_FORWARDED_FOR đã được thêm bởi Nginx và sẽ hiển thị công khai địa chỉ IP của máy tính bạn đang sử dụng để truy cập URL (nếu bạn truy cập Apache trực tiếp trên cổng 8080 thì bạn sẽ không thấy các biến số trên).
![Kiem tra bien so php](/image/check_php_proxy.png)
## 2.3. Cài đặt và cấu hình mod_rpaf
- Module rpaf là một plugin cho Apache, được sử dụng trong reverse proxy để lấy địa chỉ IP thực của người dùng từ header X-Forwarded-For. Mô-đun này thay đổi địa chỉ IP từ xa (remote address) của máy khách mà các mô-đun Apache khác nhìn thấy khi nhận được yêu cầu qua một proxy tin cậy, đảm bảo nhật ký và thống kê truy cập hiển thị chính xác IP của người dùng cuối.
- Chuyển đến thư mục home và cài đặt các gói cần thiết để xây dựng module
	```
	wget https://github.com/gnif/mod_rpaf/archive/stable.zip
	unzip stable.zip
	```
- Chuyển vào thư mục mới chứa các file: ``cd mod_rpaf-stable``
- Biên dịch và cài đặt module:
	```
	make
	make install
	```
- Tạo một tệp trong thư mục mods-available để tải module rpaf
	``sudo nano /etc/apache2/mods-available/rpaf.load``
- Thêm đoạn code sau vào file để tải module, lưu và thoát:
	```
	LoadModule rpaf_module /usr/lib/apache2/modules/mod_rpaf.so
	```
- Tạo một file khác trong thư mục này có tên rpaf.conf chứa các chỉ thị cấu hình cho mod_rpaf
	```
	sudo nano /etc/apache2/mods-available/rpaf.conf
	```
- Thêm đoạn code sau:
	```
	<IfModule mod_rpaf.c>
        RPAF_Enable             On
        RPAF_Header             X-Real-Ip
        RPAF_ProxyIPs           your_server_ip 
        RPAF_SetHostName        On
        RPAF_SetHTTPS           On
        RPAF_SetPort            On
    </IfModule>
    ```
	- RPAF_Header: Tiêu đề sử dụng cho địa chỉ IP thực của máy client.
	- RPAF_ProxyIPs: Địa chỉ IP của proxy để điều chỉnh yêu cầu HTTP.
	- RPAF_SetHostName: Cập nhật tên của virtual server để ServerName và ServerAlias hoạt động.
	- RPAF_SetHTTPS: Đặt biến môi trường HTTPS dựa trên giá trị chứa trong X-	Forwarded-Proto.
	- RPAF_SetPort: Đặt biến môi trường SERVER_PORT. Điều này hữu ích khi Apache đứng sau một proxy SSL.
- Lưu file, kích hoạt module, kiểm tra và reload apache2:
	```
	sudo a2enmod rpaf
	sudo apachectl -t
	sudo systemctl reload apache2
	```domain
- Truy cập vào http://domain/info.php , kiểm tra phần PHP Variables, biến REMOTE_ADDR sẽ là địa chỉ IP công khai của máy tính của bạn.
![kiem tra rpaf](/image/rpaf.png)
