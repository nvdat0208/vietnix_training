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
## Thêm website
- Để thêm một website mới, chọn Website trên thanh công cụ bên trái. Nhấn vào Add site.
![aapanel website1](/image/aa_website1.png)
- Nhập domain vào ô đầu tiên, các mục còn lại sẽ tự động được điền. Đặc biệt, tại mục Database, chọn MySQL. Lưu lại thông tin Username và Password vì sẽ cần thiết cho các bước tiếp theo. Cuối cùng, nhấn Confirm để thêm.
![aapanel website1](/image/aa_website2.png)
- Hoàn thành bước thêm website. Tải lên mã nguồn của WordPress cho website mới.
![aapanel website1](/image/aa_website3.png)
