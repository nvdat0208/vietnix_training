## Cài đặt VestaCP
- Truy cập vào ``https://vestacp.com/install/`` và điền các thông tin sau, sau đó bấm Generate Install Command để nhận được các lệnh cài đặt Vesta.
![vesta1](/image/vesta1.png)

![vesta2](/image/vesta2.png)
- Chạy các lệnh cài đặt, bấm ``Y`` đê xác nhận cài đặt Vesta và các phần mềm liên quan, chọn port 8083 cho Vesta
![vesta3](/image/vesta3.png)
- Sau khi cài đặt xong, màn hình sẽ hiện link truy cập và tài khoản đăng nhập
![vesta4](/image/vesta4.png)

![vesta5](/image/vesta5.png)
## Tạo domain cho website
- Tạo 2 domain cho website
![vesta_web1](/image/vesta_web1.png)
- Thêm các thông tin cho website
![vesta_web2](/image/vesta_web2.png)
- Thêm chứng chỉ SSL, sau đó bấm Add để tạo
![vesta_web3](/image/vesta_web3.png)
- 2 website mới được tạo sẽ hiện trên dashboard
![vesta_web4](/image/vesta_web4.png)
## Tải source code lên VestaCP
- Vào File Manager, đến đường dẫn mặc định của Vesta ``/home/admin/web/laravel.datnguyen.vietnix.tech/public_html/domain/``
![vesta_sour1](/image/vesta_sour1.png)
- Tải source code lên thư mục tương ứng với từng domain và giải nén
![vesta_sour2](/image/vesta_sour2.png)

![vesta_sour3](/image/vesta_sour3.png)
## Tạo database
- Vào mục DB, tạo 2 database cho website
![vesta_db1](/image/vesta_db1.png)
- Thêm các thông tin sau cho database
![vesta_db2](/image/vesta_db2.png)
- Sau khi tạo sẽ hiển thị trên dashboard
![vesta_db3](/image/vesta_db3.png)
## Đăng nhập để kiểm tra
- Truy cập thử 2 domain để kiểm tra, ta sẽ thấy trang ``wp.datnguyen.vietnix.tech`` vào bình thường, còn domain laravel hiện tại vẫn chưa vào được do chưa đúng document root và phiên bản PHP
![wordpress](/image/wordpress.png)

![laravel](/image/laravel.png)
- VestaCP chỉ cài mặc định php7.2, để laravel có thể chạy được, ta cần cài thêm php8.1
- Ubuntu 18.04 đã kết thúc thời gian hỗ trợ tiêu chuẩn, Điều này có nghĩa là các kho lưu trữ chính thức và hầu hết các Kho lưu trữ Gói Cá nhân (PPA), bao gồm cả Ondřej Surý PPA phổ biển dành cho PHP, không còn cung cấp các bản cập nhật hoặc gói cho Ubuntu 18.04. Điều này bao gồm cả PHP 8.1.
- Việc cố gắng cài đặt PHP 8.1 trực tiếp bằng lệnh apt install php8.1 sau khi thêm Ondřej Surý PPA trên Ubuntu 18.04 có thể dẫn đến lỗi "Không tìm thấy gói".
- Ta cần cài đặt và cấu hình thủ công php8.1, sử dụng lệnh wget
	```
	cd /usr/local/src/
	wget https://www.php.net/distributions/php-8.1.30.tar.gz
	tar -xvf php-8.1.30.tar.gz
	```
- Cài đặt các thư viện cần thiết để biên dịch php8.1
	```
	apt install build-essential autoconf bison re2c libxml2-dev libsqlite3-dev 			libonig-dev libcurl4-openssl-dev libjpeg-dev libpng-dev libwebp-dev 				libfreetype6-dev libzip-dev libssl-dev pkg-config
	```
- Thực hiện biên dịch php8.1
	```
   cd /usr/local/php-8.1.30/
	./configure --prefix=/usr/local/php81
	  --enable-fpm
	  --with-openssl
	  --with-zlib
	  --with-curl
	  --with-mysqli
	  --with-pdo-mysql
	  --enable-mbstring
	  --with-zip
	  
	make -j$(nproc)
	sudo make install
	```
- Thực hiện kiểm tra php8.1 đã được cài đặt
	
	``php -v``

	![php8.1](/image/php8.1.png)
