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
