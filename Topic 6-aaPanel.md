## Cài đặt aaPanel
- Tải aaPanel về VPS
	```
	wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && sudo bash install.sh
	```
- Trong khi tải, hệ thống yêu cầu xác nhận một số thiết lâp, chỉ cần bấm ``yes``.
- Sau khi cài xong, màn hình sẽ hiện link đăng nhập, user và password.

![aapanel acc](/image/aa_acc.png)
- Mở trình duyệt và đăng nhập vào aaPanel. Sau khi đăng nhập, hệ thống sẽ yêu cầu chọn loại web server, ta chọn one-click install vào LAMP. Quá trình có thể mất vài phút.

![aapanel lamp](/image/aa_lamp.png)
## Tạo domain
- Để thêm một website mới, chọn Website trên thanh công cụ bên trái. Nhấn vào Add site.
![aapanel website1](/image/aa_website1.png)
- Nhập domain vào ô đầu tiên, các mục còn lại sẽ tự động được điền. Đặc biệt, tại mục Database, chọn MySQL. Lưu lại thông tin Username và Password vì sẽ cần thiết cho các bước tiếp theo. Cuối cùng, nhấn Confirm để thêm.
![aapanel website1](/image/aa_website2.png)
- Hoàn thành bước thêm website. Tải lên mã nguồn của WordPress cho website mới.
![aapanel website1](/image/aa_website3.png)
## Tải mã nguồn
- Vào mục Files và đi theo đường dẫn mà Document Root của 2 website, xóa tất cả các file mặc định được tạo khi tạo mới website trên aaPanel.
![aapanel source1](/image/aa_source1.png)
- Sau khi đã xóa hết, chọn Upload để tải lên file nén chứa mã nguồn, nhấn Comfirm upload để bắt đầu quá trình tải lên.
![aapanel source2](/image/aa_source2.png)
![aapanel source3](/image/aa_source3.png)
- Sau đó, chọn Unzip để giải nén mã nguồn, chọn Confirm để bắt đầu quá trình giải nén.
![aapanel source4](/image/aa_source4.png)
- Hoàn tất quá trình giải nén, bạn sẽ thấy một thư mục mới có tên là "source wordpress" đã được tạo ra. Vào thư mục này, chọn tất cả các file và nhấn vào Cut, sau đó Paste tất cả file vào đúng Document Root của website.
## Thêm chứng chỉ SSL cho website
- Vào mục Website trên aaPanel, trên các domain đã được tạo, bấm vào ``Conf`` để thêm chứng chỉ SSL
![aapanel ssl1](/image/aa_ssl1.png)
- mục SSL sẽ hiện ra trên màn hình, ta thêm Private key và Certificate được yêu cầu, ở đây sẽ sử dụng SSL đã đăng ký với ZeroSSL trước đó.
![aapanel ssl2](/image/aa_ssl2.png)
- Sau khi thêm, bấm Save and enable SSL, hệ thống sẽ báo thành công.
![aapanel ssl3](/image/aa_ssl3.png)
- Ra màn hình Website của aaPanel, ta sẽ thấy 2 domain đã được cấp SSL với thời hạn còn lại của SSL.
![aapanel ssl4](/image/aa_ssl4.png)
## Upload cơ sở dữ liệu cho website
- Vào mục Database - MySQL, chọn database cần import dữ liệu, bấm Import
![aapanel db1](/image/aa_db1.png)
- Để import dữ liệu cho database, ta cần upload file sql đã chuẩn bị trên máy tính, sau đó import vào database.
![aapanel db2](/image/aa_db2.png)
- Sau khi xong, khai báo lại cấu hình database cho domain
	```
	DB_CONNECTION=mysql
	DB_HOST=127.0.0.1
	DB_PORT=3306
	DB_DATABASE=sql_laravel_datn
	DB_USERNAME=sql_laravel_datn
	DB_PASSWORD=ba3d87b6aff5a
	```
- Khởi động lại Apache và đăng nhập vào web để kiểm tra
![aapanel wp](/image/aa_wp.png)
