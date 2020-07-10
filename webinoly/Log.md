## Log manager and real-time viewer (trình quản lý nhật ký và trình xem thời gian thực)
Lệnh **Log** cho phép chúng ta hình dung các bản ghi trong thời gian thực được tạo như 1 lịch sử của các sự kiện khác nhau trên máy chủ của chúng ta, bằng cách truy cập của khác truy cập vào các trang của trang web được lưu trữ hoặc bằng các công cụ khác nhau được thực thi bên  trong

### Cú pháp

`sudo log <domain> <option>`

**Options** :

 * -error
 * -le
 * -mail
 * -mysql
 * -php
 * -purge
 * -ssh
 * -wp
 * -only-error

### Cách xem nhật ký truy cập của trang web của mình
Để xem nhật ký cho một trang web cụ thể

`sudo log example.com`

### Xem nhật ký lỗi

`sudo log example.com -error`

### Gỡ lỗi WordPress
Khi ta cấu hình chế độ gỡ lỗi trong WordPress trong các `wp-config.php` tập tin chúng ta có tùy chọn để tạo một `debug.log` tập tin, thường nằm ở thư mục `/wp-content`

Để xem trong thời gian thực từ dòng lệnh các sự kiện được tạo trong tệp này , chúng ta sử dụng lệnh

`sudo log example.com -wp`

Ngoài ra, chúng ta có tùy chọn (bật /tắt) chế độ gỡ lỗi 

`sudo log example.com -wp=on`

### Nginx Access Logs
Mặc định, nhật ký Nginx Access bị tắt

Để kích hoạt nhật lý truy cập chung và áp dụng cho tất cả các trang web mới được tạo sau này

`sudo log -only-error=off`

Để kích hoạt nhật ký của một trang web cụ thể

`sudo log example.com -only-error=off`

Xóa toàn bộ file log đã được nén lại thành file *.gz (log được rotate) trong thư mục /var/log bằng cách sử dụng lệnh

`log -purge`