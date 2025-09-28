## Cập nhật hệ thống
```
apt update -y
```
```
apt upgrade -y
```
```
dpkg --configure -a
```
```
apt --fix-broken install
```
## Alias
```
nano ~/.bashrc
```
```
source ~/.bashrc
```
```
alias restart-nginx='systemctl restart nginx'  
alias status-nginx='systemctl status nginx'  
alias restart-apache2='systemctl restart apache2'  
alias status-apache2='systemctl status apache2'  
alias status-mariadb='systemctl status mariadb'  
alias restart-mariadb='systemctl restart mariadb'  
alias status-php8.1='systemctl status php8.1-fpm'  
alias restart-php8.1='systemctl restart php8.1-fpm'  
alias status-php7.4='systemctl status php7.4-fpm'  
alias restart-php7.4='systemctl restart php7.4-fpm'
```
## Net tools
```
apt install net-tools -y
```
```
iptables -I INPUT -p tcp --dport 80 -j ACCEPT  
iptables -I INPUT -p tcp --dport 443 -j ACCEPT
```
```
netstat -tlpn
```
## Import du lieu
```
mkdir -p /etc/laravel_ssl/
mkdir -p /etc/wp_ssl/
```
```
scp ca_bundle.crt certificate.crt private.key root@103.90.226.73:/etc/wp_ssl/
scp ca_bundle.crt certificate.crt private.key root@103.90.226.73:/etc/laravel_ssl/
```
```
chmod 600 /etc/ssl/lavarel/private.key
```
## Nginx
```
apt install nginx -y
```
```
systemctl enable nginx  
systemctl status nginx
```
## Apache2
```
apt install apache2 -y
```
```
vi /etc/apache2/ports.conf
```
```
systemctl enable apache2
systemctl status apache2
```
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
## MariaDB
```
apt install mariadb-server mariadb-client -y
```
```
systemctl enable mariadb
systemctl start mariadb
```
``` 
mysql_secure_installation
```
## PHP
```
apt install php8.1 php8.1-fpm php8.1-mysql php-common php8.1-cli php8.1-common php8.1-opcache php8.1-readline php8.1-mbstring php8.1-xml php8.1-gd php8.1-curl php8.1-soap php8.1-mbstring -y
```
```
systemctl enable php8.1-fpm
systemctl start php8.1-fpm
```
## phpMyAdmin
```
apt install phpmyadmin -y
```
## Wordpress
```
cd /tmp
```
```
wget https://wordpress.org/latest.tar.gz
```
```
tar -xzvf latest.tar.gz
```
```
mv wordpress /var/www/
```
```
chown -R www-data:www-data /var/www/wordpress
```
```
chmod -R 755 /var/www/wordpress/
```
```
nano /etc/nginx/sites-available/wordpress
```
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
    server_name wp.datnguyen.vietnix.tech;
    root /var/www/wordpress;
    index index.php index.html index.htm;

    # SSL
    ssl_certificate /etc/wp_ssl/certificate.crt;
    ssl_certificate_key /etc/wp_ssl/private.key;
    ssl_trusted_certificate /etc/wp_ssl/ca_bundle.crt;

    # SSL nâng cao
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256';
    ssl_prefer_server_ciphers on;

    location / {
        try_files \$uri \$uri/ /index.php?\$args;
    }

    location ~ \\.php\$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }
}
```
```
ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
nginx -t # Kiểm tra cú pháp
systemctl restart nginx
```
## Laravel
```
cd /var/www/
```
```
composer create-project --prefer-dist laravel/laravel laravel
```
```
chown -R www-data:www-data /var/www/laravel
```
```
chmod -R 775 /var/www/laravel/storage
```
```
nano /etc/nginx/sites-available/laravel
```
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
    root /var/www/laravel/public; # Sửa lại đường dẫn

    # SSL
    ssl_certificate /etc/laravel_ssl/certificate.crt;
    ssl_certificate_key /etc/laravel_ssl/private.key;
    ssl_trusted_certificate /etc/laravel_ssl/ca_bundle.crt;

    # SSL nâng cao
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256';
    ssl_prefer_server_ciphers on;

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
```
ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/
nginx -t
systemctl restart nginx
```
