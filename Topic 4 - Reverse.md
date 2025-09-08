# Xây dựng mô hình reverse proxy kết hợp giữa 2 webserver là nginx và apache

## 1. Cấu hình Apache (Backend Reverse Proxy)
- Cài đặt Apache

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
	
- Tạo file virtual host cho wordpress
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
- Virtual Host cho Laravel
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
## 2. Cấu hình Nginx (Frontend Reverse Proxy)
- Tạo file cấu hình cho laravel
	``/etc/nginx/sites-available/vhost-laravel.conf``

	```
	server {
	    listen 80;
	    listen 443 ssl;
	    server_name laravel.datnguyen.vietnix.tech;
	
	    # Cấu hình SSL
	    ssl_certificate /etc/ssl/laravel/certificate.crt;
	    ssl_certificate_key /etc/ssl/laravel/private.key;
	
	    location / {
	        proxy_pass http://localhost:8080;
	        proxy_set_header Host $host;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_set_header X-Forwarded-Proto $scheme;
	    }
	}
	```
	
- Tạo file cấu hình cho wordpress
	``/etc/nginx/sites-available/vhost-wordpress.conf``
	```
	server {
	    listen 80;
	    listen 443 ssl;
	    server_name wp.datnguyen.vietnix.tech;
	
	    # Cấu hình SSL
	    ssl_certificate /etc/ssl/wordpress/certificate.crt;
	    ssl_certificate_key /etc/ssl/wordpress/private.key;
	
	    location / {
	        proxy_pass http://localhost:8080;
	        proxy_set_header Host $host;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_set_header X-Forwarded-Proto $scheme;
	    }
	}
	```
	
- Kích hoạt các Virtual Hosts:
	```
	ln -s /etc/nginx/sites-available/vhost-laravel.conf /etc/nginx/sites-enabled/
	ln -s /etc/nginx/sites-available/vhost-wordpress.conf /etc/nginx/sites-enabled/
	systemctl restart nginx
	```
