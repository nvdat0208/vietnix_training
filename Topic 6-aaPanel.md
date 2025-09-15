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
## Tải mã nguồn
- Vào mục Files và đi theo đường dẫn mà Document Root của 2 website, xóa tất cả các file mặc định được tạo khi tạo mới website trên aaPanel.
![aapanel source1](/image/aa_source1.png)
- Sau khi đã xóa hết, chọn Upload để tải lên file nén chứa mã nguồn, nhấn Comfirm upload để bắt đầu quá trình tải lên.
![aapanel source2](/image/aa_source2.png)
![aapanel source3](/image/aa_source3.png)
- Sau đó, chọn Unzip để giải nén mã nguồn, chọn Confirm để bắt đầu quá trình giải nén.
![aapanel source4](/image/aa_source4.png)
- Hoàn tất quá trình giải nén, bạn sẽ thấy một thư mục mới có tên là "source wordpress" đã được tạo ra. Vào thư mục này, chọn tất cả các file và nhấn vào Cut, sau đó Paste tất cả file vào đúng Document Root của website.
![aapanel source6](/image/aa_source6.png)
