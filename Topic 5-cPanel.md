## 1. Tìm hiểu Làm quen với cPanel
- cPanel là một hệ thống quản trị web hosting mạnh mẽ, được xây dựng trên nền tảng Linux, cung cấp giao diện đồ họa trực quan và dễ sử dụng. Giao diện này được thiết kế đơn giản, linh hoạt, cho phép người dùng dễ dàng quản lý mọi khía cạnh của website và hosting.
- cPanel hoạt động dựa trên một hệ thống phân cấp ba lớp: Nhà cung cấp dịch vụ hosting (Hosting Company), Đại lý bán lại (Reseller) và Người dùng cuối (End User). Cả Hosting Company và Reseller đều sử dụng giao diện Web Host Manager (WHM) để quản lý máy chủ, trong khi End User tương tác trực tiếp với giao diện cPanel. WHM cấp cho Hosting Company quyền quản trị cao nhất, đồng thời cho phép họ tùy chỉnh quyền truy cập của Reseller đối với các tính năng nhất định.
### 1.1. Quản lý tập tin
- File Manager: Cho phép truy cập, tạo,sửa và xóa file trực tiếp trên hosting.
- Disk Usage: Theo dõi và giám dung lượng ổ cứng.
- Backup and Backup Wizard: Sao lưu và khôi phục dữ liệu trên hosting một cách dễ dàng.
- Images: Cho phép thay đổi kích thước, chuyển đổi định dạng và xem ảnh trên cPanel.
- Web Disk: Quản lý không gian lưu trữ như trên máy tính cá nhân, hỗ trợ chỉnh sửa, di chuyển, upload và download file.
- Directory Privacy: Tăng cường bảo mật bằng cách đặt mật khẩu cho thư mục.
- FTP Accounts: quản lý và thêm tài khoản FTP dễ dàng.
- Hỗ trợ tính năng bổ sung như File and Directory Restoration, Git Version Control...
### 1.2. Quản lý cơ sở dữ liệu
- phpMyAdmin: giúp quản trị cơ sở dữ liệu MySQL một cách trực quan và dễ dàng.
- Remote Database Access: Cho phép kết nối và truy cập đến cơ sở dữ liệu từ xa, hỗ trợ các ứng dụng trên máy chủ khác sử dụng chung dữ liệu.
- Manage My Databases: Cung cấp giao diện quản lý toàn diện các cơ sở dữ liệu trên website, bao gồm: tạo, sửa và quản lý tài khoản user.
- Database Wizard: Hỗ trợ tạo và quản lý cơ sở dữ liệu trên cPanel.
### 1.3. Quản lý tên miền
- Site Publisher: Cho phép tạo trang web đơn giản hoặc trang chờ trong khi phát triển website chính.
- Redirects: thiết lập chuyển hướng từ URL này sang URL khác.
- Zone Editor: Kiểm soát và quản lý các bản ghi DNS.
- Sitejet Builder : Tạo trang web đơn giản bằng giao diện kéo thả trực quan mà không cần kiến thức lập trình.
- Dynamic DNS: Quản lý tên miền với địa chỉ IP động.
- WordPress Managment: Cung cấp các công cụ để cài đặt, cập nhật và quản lý theme, plugin cho website WordPress.
- Domains: Cho phép tạo và quản lý các tên miền phụ (subdomain), tên miền alias và tên miền addon.
### 1.4. Tính năng email
- Email Accounts: tạo, quản lý và kiểm soát các tài khoản email.
- Autoresponders: Gửi phản hồi tự động đến các email nhận được, hữu ích cho việc thông báo vắng mặt.
- Track Delivery: Giám sát quá trình gửi email.
- Forwarders: Thiết lập chuyển tiếp email cho các địa chỉ email cụ thể.
- Default Address: Chỉ định địa chỉ email mặc định nhận tất cả email gửi đến địa chỉ không tồn tại trên tên miền.
- Global Email Filters: Thiết lập bộ lọc email.
- Encryption: Nâng cao bảo mật email bằng cách sử dụng khóa công khai.
- Mailing Lists: Gửi email hàng loạt đến nhiều người nhận cùng lúc.
- Email Filters: Lọc email đến theo các quy tắc tùy chỉnh
- BoxTrapper: Chặn email không xác định và yêu cầu xác thực trước khi gửi vào hộp thư đến.
- Address Importer: Thêm địa chỉ email từ file bên ngoài như csv hay xls.
- Email Disk Usage: Theo dõi dung lượng lưu trữ email của từng tài khoản.
- Email deliverability: Xác định và khắc phục vấn đề liên quan đến DNS record để đảm bảo email gửi/nhận thành công.
### 1.5. Tính năng bảo mật
- SSH Access: Kết nối an toàn đến máy chủ thông qua dòng lệnh.
- Hotlink Protection: Ngăn chặn các website khác nhúng trực tiếp nội dung từ website của bạn, giúp tiết kiệm băng thông.
- ModSecurity Domain Manager: Kích hoạt hoặc vô hiệu hóa ModSecurity, một hệ thống tường lửa web.
- IP Blocker: Chặn truy cập từ các địa chỉ IP cụ thể.
- Two-Factor Authentication: Tăng cường bảo mật đăng nhập bằng cách yêu cầu xác minh hai lớp.
- SSL/TLS: Quản lý, cài đặt SSL/TLS để mã hóa kết nối và bảo vệ dữ liệu truyền tải.
- Security Policy: Thiết lập các câu hỏi xác minh cho các truy cập từ IP không xác định.
- SSL/TLS Status: Kiểm tra trạng thái của SSL/TLS hiện tại.
### 1.6. Các ứng dụng phần mềm
- WordPress Manager by Softaculous: Cài đặt, quản lý và cập nhật WordPress từ cPanel.
- Site Software: Cài đặt các ứng dụng web phổ biến như phần mềm thương mại điện tử và diễn đàn.
- Optimize Website: Tối ưu thời gian phản hồi của Web Server Apache để cải thiện tốc độ tải trang.
- Application Manager: Đăng ký, quản lý và triển khai các ứng dụng tùy chỉnh thông qua Phusion Passenger.
- MultiPHP Manager: Lựa chọn và áp dụng nhiều phiên bản PHP khác nhau cho từng website cụ thể.
- Softaculous Apps Installer: Cài đặt, quản lý các ứng dụng trên Softaculous.
- Setup Node.js App: Triển khai và quản lý ứng dụng Node.js (yêu cầu kiến thức chuyên môn).
- Select PHP Version: Tùy chọn phiên bản PHP chạy song song.
- X-Ray App: Ứng dụng theo dõi và phân tích tiến trình PHP theo thời gian thực để khắc phục sự cố và tối ưu hiệu suất.
## 2. Move dữ liệu từ VPS sang CPanel, SSL dùng cert ZeroSSL đã tạo trước đó
