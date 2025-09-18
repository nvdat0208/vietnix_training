## Tạo website
- Trên giao diện CyberPanel, chọn Website - Create website, nhập các thông tin để tạo website
![cyber_web1](/image/cyber_web1.png)

- Trỏ domain vào source code
![cyber_web2](/image/cyber_web2.png)
## Tạo database
- Vào mục Database - Create database, tạo database cho website
![cyber_sql1](/image/cyber_sql1.png)
- Database sau khi được tạo sẽ nằm ở List database, tiếp tục import dữ liệu cho database
![cyber_sql2](/image/cyber_sql2.png)
- Vào mục Database - PHPMYADMIN, đăng nhập vào phpmyadmin, chọn database và import file sql có sẵn trên máy tính
![cyber_sql3](/image/cyber_sql3.png)
## Thêm chứng chỉ SSL cho domain
- Sau khi tạo xong, SSL cho domain sẽ được tự động thêm
![cyber_ssl1](/image/cyber_ssl1.png)
- Vào mục SSL - Manage SSL, kiểm tra domain đã được thêm SSL
![cyber_ssl2](/image/cyber_ssl2.png)
- Đăng nhập vào website kiểm tra, ta sẽ thấy đang hiện giao diện mặc định của CyberPanel do source web chưa trỏ đúng cấu hình. Ta sẽ trỏ lại đúng đường dẫn source
![cyber_ssl2](/image/cyber_ssl3.png)
- Trên terminal, đến đường dẫn ``/usr/local/lsws/conf/vhosts/laravel.datnguyen.vietnix.tech``, tìm mục docRoot và chỉnh lại đúng thư mục source. Sau đó lưu lại và khởi động lại

![cyber_ssl4](/image/cyber_ssl4.png)

![cyber_ssl5](/image/cyber_ssl5.png)
- Thao tác tương tự với Wordpress
![cyber_ssl6](/image/cyber_ssl6.png)
## Tạo proxypass openlitespeed
### Truy cập trang quản trị OpenLiteSpeed
- Chạy lệnh sau để lấy tài khoản
	```
	/usr/local/lsws/admin/misc/admpass.sh
	```
- Hệ thống sẽ yêu cầu nhập user và pass, tài khoản này sẽ dùng để đăng nhập openlitespeed.
![ls1](/image/ls1.png)
- Truy cập vào ``103.90.226.73:7080`` để đăng nhập trang quản trị
![ls2](/image/ls2.png)

![ls3](/image/ls3.png)
### Tạo proxy
- Thực hiện các bước sau để tạo proxy:
	- Chọn Server Configuration từ menu bên trái.
	- Chọn tab External App.
	- Nhấn vào dấu cộng (Add) để thêm một External App mới.
![ls_proxy1](/image/ls_proxy1.png)
- Tại mục Type, bạn chọn Web Server và nhấn dấu mũi tên để chuyển sang bước tiếp theo.
![ls_proxy2](/image/ls_proxy2.png)
- Cuối cùng, bạn điền đầy đủ các thông tin cần thiết cho proxy và nhấn Save:
	- Name: proxy_pass
	- Address: localhost:5000
	- Max Connections: 100
	- Initial Request Timeout (sec): 60s
	- Retry Timeout (secs): 0s
	- Response Buffering: No
![ls_proxy3](/image/ls_proxy3.png)

![ls_proxy4](/image/ls_proxy4.png)
- Cấu hình context trong virtual host, tạo 1 proxy/api cho domain
![ls_proxy6](/image/ls_proxy6.png)

![ls_proxy7](/image/ls_proxy7.png)

![ls_proxy8](/image/ls_proxy8.png)

![ls_proxy9](/image/ls_proxy9.png)
### Tạo app chạy trên port 5000
- Cài đặt Flask để tạo app

	``pip install flask``
- Tạo app với flask
	```
	from flask import Flask
	app = Flask(__name__)
	
	@app.route('/api/')
	def hello():
	    return 'Hello world!'
	
	if __name__ == '__main__':
	    app.run(host='127.0.0.1', port=5000)
	```
- Truy cập đường dẫn https://wp.datnguyen.vietnix.tech/api/ để kiểm tra thử
![ls_proxy10](/image/ls_proxy10.png)
