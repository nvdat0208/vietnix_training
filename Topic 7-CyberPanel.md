## Tạo website
- Trên giao diện CyberPanel, chọn Website - Create website, nhập các thông tin để tạo website
![cyber_web1](/image/cyber_web1)

- Trỏ domain vào source code
![cyber_web2](/image/cyber_web2)
## Tạo database
- Vào mục Database - Create database, tạo database cho website
![cyber_sql1](/image/cyber_sql1)
- Database sau khi được tạo sẽ nằm ở List database, tiếp tục import dữ liệu cho database
![cyber_sql2](/image/cyber_sql2)
- Vào mục Database - PHPMYADMIN, đăng nhập vào phpmyadmin, chọn database và import file sql có sẵn trên máy tính
![cyber_sql3](/image/cyber_sql3)
## Thêm chứng chỉ SSL cho domain
- Thao tác thêm chứng chỉ SSL cho domain
![cyber_ssl1](/image/cyber_ssl1)
- Vào mục SSL - Manage SSL, kiểm tra domain đã được thêm SSL
![cyber_ssl2](/image/cyber_ssl2)
- Đăng nhập vào website kiểm tra, ta sẽ thấy đang hiện giao diện mặc định của CyberPanel do source web chưa trỏ đúng cấu hình. Ta sẽ trỏ lại đúng đường dẫn source
![cyber_ssl2](/image/cyber_ssl3)
- Trên terminal, đến đường dẫn ``/usr/local/lsws/conf/vhosts/laravel.datnguyen.vietnix.tech``, tìm mục docRoot và chỉnh lại đúng thư mục source. Sau đó lưu lại và khởi động lại 
![cyber_ssl4](/image/cyber_ssl4)

![cyber_ssl5](/image/cyber_ssl5)
