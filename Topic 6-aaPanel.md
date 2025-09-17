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
## Cài plugins trên WordPress
### Rank math SEO
- Rank Math là một plugin SEO cho các website WordPress. Công cụ này tập trung chủ yếu vào on-page SEO và nhiều kỹ thuật SEO từ cơ bản đến nâng cao. Rank Math đã có hơn 1.000.000 lượt cài đặt với nhiều tính năng hỗ trợ SEO miễn phí. Đây được xem là một trong những Plugin SEO tốt nhất hiện nay.
- Một số ưu điểm:
	+ Tích hợp công cụ phân tích dữ liệu
	+ Có sẵn Schema và Rich Snippets
	+ Cấu hình các chức năng của Rank Math dễ dàng
	+ Điểm SEO
	+ Tự động tối ưu SEO
- Dùng aaPanel tải file.zip của plugin và giải nén
![seo1](/image/seo1.png)
- Đăng nhập vào trang wordpress admin và kích hoạt plugin
![seo2](/image/seo2.png)
- Đã kích hoạt thành công

![seo3](/image/seo3.png)
### Elementor
- Elementor là plugin page builder dành cho WordPress. Plugin này tạo các trang WordPress một cách dễ dàng bằng visual editor. Elementor sẽ giúp bạn thiết kế website trực quan và có thể dễ nhìn thấy sự thay đổi trên trang web của mình ngay lập tức. Đặc biệt, nếu không am hiểu về code, bạn cũng hoàn toàn có thể sửdụng Elementor để tạo website cho riêng mình.
- Plugin Elementor là một giải pháp all-in-one, cho phép bạn kiểm soát mọi thứ trong việc thiết kế website trên một nền tảng duy nhất. Bạn có thể tùy chỉnh website để phù hợp với thương hiệu của mình bằng các hiệu ứng chuyển động, nhiều font chữ và background. Ngoài ra, Elementor còn có những tiện ích tùy chỉnh liên quan đến văn bản, hình ảnh, đánh giá của khách hàng, thanh trượt, biểu tượng,... được tích hợp sẵn và cực kỳ dễ sử dụng.

![elementor](/image/elementor.png)
- Vì wordpress đang dùng flatsome nên nếu bật elementor sẽ gây ra lỗi giao diện web

![err theme](/image/err_theme1.png)
### Divi
- Divi là theme WordPress được phát triển bởi Elegant Themes, đây là một trong những lựa chọn hàng đầu cho việc xây dựng và thiết kế website trên nền tảng WordPress. Không chỉ sở hữu thiết kế đẹp mắt, tốc độ tải nhanh và tối ưu cho SEO, Divi WordPress theme còn có nhiều tính năng hữu ích, đặc biệt là khả năng tạo ra template không giới hạn.
- Divi tích hợp sẵn plugin Divi Builder và cung cấp nhiều mẫu website có sẵn từ nhiều lĩnh vực khác nhau, giúp người dùng dễ dàng tạo ra các mẫu template riêng tùy vào mục đích sử dụng. Ngoài ra, hệ thống Modules và Premade Layouts đa dạng, mang đến cho người dùng nhiều tùy chọn để sử dụng hoặc chỉnh sửa website theo ý thích.
- Ngoài ra, Divi tích hợp một bộ công cụ giúp người dùng dễ dàng tối ưu hóa website nhằm thứ hạng cao trên các công cụ tìm kiếm. Không chỉ vậy, việc tích hợp các công cụ như WooCommerce cũng giúp người dùng có thể xây dựng các cửa hàng trực tuyến dễ dàng, thuận tiện.
- Tương tự như elementor, vì dùng flatsome nên nếu bật divi sẽ gây ra lỗi giao diện web
![err theme](/image/err_theme2.png)
### MyThemeShop
- MyThemeShop được biết đến là thương hiệu sản xuất các theme và plugin WordPress phân phối cho người dùng trên toàn thế giới. Tính đến thời điểm hiện tại, MyThemeShop đã sáng tạo ra hơn 83 sản phẩm bao gồm 65 theme và nhiều loại plugin mang lại nhiều tính năng ưu việt.
- Theme và plugin của MyThemeShop hỗ trợ nhiều tính năng, tương thích với mọi website và thời gian tải website nhanh chóng. Có thể thấy, được tích hợp tính năng tối ưu hóa cho các công cụ tìm kiếm là ưu điểm vượt trội mang lại doanh thu cho các theme của MyThemeShop. Các theme của MyThemeShop hoạt động rất tốt trong việc nâng cao tốc độ tải trang, và một số theme đã xếp hạng ở vị trí TOP đầu trong một số bài kiểm tra tốc độ tải trang của theme WordPress hiện nay.
- Ta sẽ download một theme bất kỳ từ mythemeshop và upload trên trang wordpress admin. VÌ hiện tại wordpress đang dùng flatsome nên khi kích hoạt theme mới cũng sẽ bị lỗi như elementor và divi
