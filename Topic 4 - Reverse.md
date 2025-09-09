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
- Truy cập http://host:8080/info.php để đảm bảo php hoạt động trên Apache
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
