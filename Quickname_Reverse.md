## PHP-FPM và mod_fastcgi
```
apt install php-fpm
```
```
wget https://mirrors.edge.kernel.org/ubuntu/pool/multiverse/liba/libapache-mod-fastcgi/libapache2-mod-fastcgi_2.4.7~0910052141-1.2_amd64.deb
```
```
dpkg -i libapache2-mod-fastcgi_2.4.7~0910052141-1.2_amd64.deb
```
```
a2enmod actions
```
```
mv /etc/apache2/mods-enabled/fastcgi.conf /etc/apache2/mods-enabled/fastcgi.conf.default
```
```
vi /etc/apache2/mods-enabled/fastcgi.conf
```
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
```
apache2ctl configtest
systemctl restart apache2
```
```
vi /etc/apache2/sites-available/vhost-default.conf
```
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
```
apache2ctl configtest
systemctl restart apache2
```
## Cấu hình Apache
```
echo "<?php phpinfo();?>" | tee /var/www/html/info.php
```
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
```
apache2ctl configtest
systemctl restart apache2
```
## Tạo virtual host cho Apache
```
mkdir -p /var/www/wordpress/
mkdir -p /var/www/laravel/
```
```
echo "<h1 style='color: green;'>Hello world</h1>" | tee index.html
echo "<?php phpinfo();?>" | tee info.php
```
```
vi etc/apache2/sites-available/vhost-laravel.conf
```
```
<VirtualHost *:8080>
     ServerName laravel.datnguyen.vietnix.tech
     ServerAlias www.laravel.datnguyen.vietnix.tech
     DocumentRoot /var/www/laravel/public/
     # Các cấu hình khác cho WordPress (vd: RewriteEngine On)
     ErrorLog ${APACHE_LOG_DIR}/domain1_error.log
     CustomLog ${APACHE_LOG_DIR}/domain1_access.log combined
 </VirtualHost>
 
 <VirtualHost *:8443>
     ServerName laravel.datnguyen.vietnix.tech
     DocumentRoot /var/www/laravel/public/
 
     SSLEngine on
     SSLCertificateFile /etc/laravel_ssl/certificate.crt
     SSLCertificateKeyFile /etc/laravel_ssl/private.key
 
     <Directory /var/www/laravel/public/>
         AllowOverride All
         Require all granted
     </Directory>
 </VirtualHost>
```
```
vi /etc/apache2/sites-available/vhost-wp.conf
```
```
<VirtualHost *:8080>
     ServerName wp.datnguyen.vietnix.tech
     ServerAlias www.wp.datnguyen.vietnix.tech
     DocumentRoot /var/www/wordpress/
     # Cấu hình Laravel, trỏ tới public/
     <Directory /var/www/wordpress/>
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
     SSLCertificateFile /etc/wp_ssl/certificate.crt
     SSLCertificateKeyFile /etc/wp_ssl/private.key
 
     <Directory /var/www/wordpress/>
         AllowOverride All
         Require all granted
     </Directory>
 </VirtualHost>
```
```
a2enmod ssl
systemctl restart apache2
```
```
a2ensite vhost-laravel.conf
a2ensite vhost-wp.conf
```
## Cấu hình Nginx
```
rm /etc/nginx/sites-enabled/default
```
```
vi /etc/nginx/sites-available/laravel
```
```
server {
 listen 80;
 server_name laravel.datnguyen.vietnix.tech;
 # SSL
 ssl_certificate /etc/laravel_ssl/certificate.crt;
 ssl_certificate_key /etc/laravel_ssl/private.key;
 ssl_trusted_certificate /etc/laravel_ssl/ca_bundle.crt;

 ssl_protocols TLSv1.2 TLSv1.3;
 ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256';
 ssl_prefer_server_ciphers on;

 charset utf-8;
 location ~* \.(jpg|jpeg|png|gif|bmp|ico|svg|tif|tiff|css|js|woff|woff2|ttf|eot|otf|mp4|mp3|wav|pdf|zip|tar|gz|rar|7z)$ {
     access_log off;
     expires max;
     try_files $uri =404;
 }
 location / {
     proxy_pass http://103.90.226.73:8080;
     proxy_set_header Host $host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header X-Forwarded-Proto $scheme;
 		}
 }
```
```
vi /etc/nginx/sites-available/wordpress
```
```
server {
 listen 80;
 server_name wp.datnguyen.vietnix.tech;

 # SSL
 ssl_certificate /etc/wp_ssl/certificate.crt;
 ssl_certificate_key /etc/wp_ssl/private.key;
 ssl_trusted_certificate /etc/wp_ssl/ca_bundle.crt;

 ssl_protocols TLSv1.2 TLSv1.3;
 ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256';
 ssl_prefer_server_ciphers on;

 charset utf-8;
 location ~* \.(jpg|jpeg|png|gif|bmp|ico|svg|tif|tiff|css|js|woff|woff2|ttf|eot|otf|mp4|mp3|wav|pdf|zip|tar|gz|rar|7z)$ {
     access_log off;
     expires max;
     try_files $uri =404;
 }
 location / {
     proxy_pass http://103.90.226.73:8080;
     proxy_set_header Host $host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header X-Forwarded-Proto $scheme;
 		}
 }
```
```
ln -s /etc/nginx/sites-available/vhost-laravel.conf /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/vhost-wp.conf /etc/nginx/sites-enabled/
systemctl restart nginx
```
## cấu hình mod_rpaf
```
wget https://github.com/gnif/mod_rpaf/archive/stable.zip
unzip stable.zip
```
```
cd mod_rpaf-stable
```
```
apt install libtool-bin
```
```
libtool --finish /usr/lib/apache2/modules
```
```
make
```
```
make install
```
```
vi /etc/apache2/mods-available/rpaf.load
```
```
vi /etc/apache2/mods-available/rpaf.conf
```
```
 <IfModule mod_rpaf.c>
     RPAF_Enable             On
     RPAF_Header             X-Real-Ip
     RPAF_ProxyIPs           103.90.226.73 
     RPAF_SetHostName        On
     RPAF_SetHTTPS           On
     RPAF_SetPort            On
 </IfModule>
```
```
a2enmod rpaf
apachectl -t
systemctl reload apache2
```
## Chặn truy cập trực tiếp vào Apache
```
iptables -I INPUT -p tcp --dport 8080 ! -s 103.90.226.73 -j REJECT --reject-with tcp-reset
iptables -I INPUT -p tcp --dport 8443 ! -s 103.90.226.73 -j REJECT --reject-with tcp-reset
```
## Tạo default vhost
```
mkdir -p /var/www/default_host
chown -R www-data:www-data /var/www/default_host
chmod -R 755 /var/www/default_host
```
```
 echo "<h1 style='color: red;'>You banned</h1>" | sudo tee /var/www/default_host/index.html
```
```
vi /etc/nginx/sites-available/default
```
```
server {
listen 80 default_server;
listen [::]:80 default_server;
server_name _;

root /var/www/default_host;
index index.html;

location / {
  try_files $uri $uri/ =404;
  }
}
server {
listen 443 ssl http2 default_server;
listen [::]:443 ssl http2 default_server;
server_name _;

# Thay thế các đường dẫn chứng chỉ dưới đây bằng đường dẫn của bạn
ssl_certificate /etc/wp_ssl/certificate.crt;
ssl_certificate_key /etc/wp_ssl/private.key;

root /var/www/default_host;
index index.html;

location / {
  try_files $uri $uri/ =404;
  }
}
```
## Import data
```
mysql -u root -p
CREATE DATABASES wordpress;
CREATE DATABASES laravel;
```
```
CREATE USER 'user_laravel'@'localhost' IDENTIFIED BY 'P@ssW0rd';
CREATE USER 'user_wordpress'@'localhost' IDENTIFIED BY 'P@ssW0rd';
GRANT ALL PRIVILEDGES ON laravel.* TO 'user_laravel'@'localhost';
GRANT ALL PRIVILEDGES ON wordpress.* TO 'user_wordpress'@'localhost';
FLUSH PRIVILEGES;
```
## PHP 7.x
```
sudo apt update
sudo apt install php7.4 php7.4-fpm php7.4-mysql php7.4-xml php7.4-mbstring php7.4-curl
```
```
<FilesMatch \.php$>
SetHandler "proxy:unix:/run/php/php7.4-fpm.sock|fcgi://localhost/"
</FilesMatch>
```
