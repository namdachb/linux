# Tổng quan webinoly
## Webinoly là gì?
Webinoly là một UNIX script dành cho hệ điều hành Ubuntu giúp chúng ta tự động cài đặt một webserver sử dụng (Nginx + MariaDB(MySQL)+ PHP + HTTP/2) hoàn chỉnh phục vụ html, PHP và dành riêng cho WordPress

Webinoly cung cấp một bộ công cụ rất tiện lợi giúp việc tạo và quản trị website. Với webinoly việc quản trị website trở nên dễ dàng hơn rất nhiều. Chỉ bằng một lệnh chúng ta đã có 1 trang web hay cũng chỉ với 1 lệnh chúng ta có thể tạo được SSL cho trang web của mình

Webinoly mới chỉ hỗ trợ trên hệ điều hành là Ubuntu 16.04 và Ubuntu 18.04

## Tính năng Webinoly
 * Chứng chỉ SSL miễn phí cho các trang web của bạn với LetsEncrypt
 * HTTP/2 tăng đáng kể tốc độ phục vụ nội dung của bạn
 * PHP v7.3 và hỗ trợ cho các phiên bản trước nếu cần (7.2, 7.1, 7.0 và 5.6)
 * FastCgi Cache và Redis Object Cache cho các trang web WordPress của bạn
 * Tự động tối ưu hóa máy chủ của bạn để tận dụng tối đa các tài nguyên có sẵn

## Quản lý trang web một cách dễ dàng
 * Các lệnh duy nhất để tạo, xóa, vô hiệu hóa các trang web
 * Hỗ trợ cho mọi trang web HTML, PHP, WordPress hoặc bất kỳ cấu hình nào trong môi trường LEMP
 * Cài đặt chứng chỉ SSL với cấu hình máy chủ tự động
 * Trình quản lý chuyển hướng Nginx, sao lưu, SMTP và nhiều tính năng khác
 * Tích hợp dữ liệu gốc để theo dõi và phân tích
 * Sửa đổi cấu hình máy chủ của bạn bất cứ lúc nào theo yêu cầu của bạn


# Cách sử dụng Webinoly
**Cấu hình đề nghị**

Để cài đặt và quản lý website sử dụng webinoly, chúng ta nên sử dụng server có cấu hình tối thiểu như sau

  * Hệ điều hành: Ubuntu 18.04
  * CPU: 1 Core
  * RAM: 1GB
  * DISK: 10GB
  * NETWORK: 1 interface (trong bài mình sử dụng IP có địa chỉ 192.168.213.193)

## Cài đặt
Việc cài đặt webinoly rất đơn giản chỉ cần 1 lệnh duy nhất

`wget -qO weby qrok.es/wy && sudo bash weby 3`

Việc cài đặt sẽ mất một vài phút. Sau khi cài xong ta sẽ thấy kết quả như sau

![Imgur](https://i.imgur.com/th1ordD.png)

> Lưu ý: Ta cần lưu lại thông tin đăng nhập database để thuận tiện cho việc quản trị sau này

## Sử dụng

### Tạo một site wordpress
Trước tiên bạn cần trỏ domain. Domain phải được trỏ về chính server của bạn

Tạo ra site wordpress bằng một lệnh duy nhất

`site wp.namdac.com -wp`

Thông báo trả về như sau là bạn đã thành công

`Site wp.namdac.com has been successfully created!`

Để bỏ quả bước xác thực HTTP khi truy cập trang admin của trang vừa tạo

`httpauth wp.namdac.com -wp-admin=off`

Bây giờ gõ sử dụng trình duyệt web để truy nhập domain để cấu hình website của bạn

![Imgur](https://i.imgur.com/OdYOfWt.png)

Sau khi cấu hình xong trên web là chúng ta đã có 1 website bằng wordpress

Chúng ta cũng có thể tạo các site khác trên server. Các site không nhất thiết phải là wordpress mà chỉ là các site có sử dụng **html php** hay **mysql**. Khi tạo site ta chỉ cần thay thế option **-wp** thành **-html** hoặc **-php** hoặc **-mysql**. Ví dụ ta tạo 1 site chỉ dùng **html** ta dùng lệnh sau

`site html.namdac.com -html`

### Để thêm SSL cho website của bạn

`site wp.namdac.com -ssl=on`

Hệ thống sẽ kiểm tra các chứng chỉ và nó sẽ renew nếu chứng chỉ nào còn thời hạn dưới 30 ngày

`site -ssl=renew`

Bạn cũng có thể sử dụng chứng chỉ có sẵn cho website của bnaj

`site example.com -ssl=on -ssl-key=/path/cert.key -ssl-crt=/path/cert.crt -ssl-ocsp=/path/cert.pem`

> Lưu ý: bạn thay thế đường dẫn đến các chứng chỉ của bạn. Tham số `-ssl-ocsp` là một tùy chọn hỗ trợ OCSP

### Tạo cache cho website
Để tăng hiệu năng cho trang web ta có thể sử dụng cache cho nó bằng

Để sử dụng cache

`site wp.namdac.com -cache=on`

Nếu không muốn sử dụng cache nữa bạn có thể dùng lệnh

`site wp.namdac.com -cache=off`

### Sử dụng Reverse Proxy
Tạo reverse proxy cho một site với nginx trên server của bạn

`site proxy.namdac.com -proxy=[http://ip:80]`

### Bật tắt tạm thời website
Tắt tạm thời website

`site wp.namdac.com -off`

Bật trở lại 

`site wp.namdac.com -on`

Liệt kê các website trên server

`site -list`

Nó sẽ thống kê toàn bộ các website đang được tạo trên server

```
root@namdac:~# site -list

 + html.namdac.com
 + po.pi.vn
 + wp2.namdac.com
 + wp.namdac.com

```

### Xóa một website
Bạn có thể sử dụng tùy chọn này để xóa một site khỏi server. Nhưng bạn cần cân nhắc kỹ khi sử dụng option này, bạn không thể khôi phục lại các file khi đã bị xóa

`site wp.namdac.com -delete`

### Clone site
Bạn có thể clone một site từ một site có sẵn. Điều này rất có ích trong quá trình site của bạn đang phát triển

ví dụ mình clone site `wp2.namdac.com` từ một site có sẵn là `wp.namdac.com`

`site wp2.namdac.com -clone-from=wp.namdac.com`

Kết quả như sau là đã clone thành công

```
root@namdac:~# site wp2.namdac.com -clone-from=wp.namdac.com
Site 'wp2.namdac.com' is now a successfull clone of 'wp.namdac.com'
```

### Thay đổi version PHP
Ta có thể kiểm tra version hiện tại của PHP

```
root@namdac:~# php -v
PHP 7.4.7 (cli) (built: Jun 12 2020 07:44:05) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.7, Copyright (c), by Zend Technologies
```

Ta thấy server đang sử dụng version `7.4.6` nhưng site của bạn không phù hợp với version này. Bạn muốn sử dụng version `7.2` bạn có thể sử dụng lệnh sau

`stack -php-vẻ=7.2`

Sau khi caì đặt trong ta kiểm tra lại version 

```
```

### Xóa PHPMyAdmin
Mặc định khi cài đặt Webinoly thì phpmyadmin sẽ được cài đặt. Nếu ta không muốn sử dụng nó thì có thể xóa bằng cách

`stack -pma -purge`

Hệ thống sẽ hỏi ta có muốn xóa không. Nhập `y` để đồng ý xóa

### Xem username và password của database
Bạn chỉ có thể xem được username và mật khẩu đăng nhập database khi database này được tạo bằng `webinoly`

`webinoly -dbpass`

Hoặc ta có thê thay đôi mật khẩu của `root` và `admin`

`webinoly -mysql-password`

### Import MySQL database
Ta có thể import DB từ một file bằng cách sử dụng lệnh sau đây

`webinoly -db-import -file=/file_name`

Sau khi import thành công ta sẽ nhận được thông báo như sau

`Database successfully imported!`

### Chặn IP truy cập site
Nếu bạn muốn chặn địa chỉ IP không cho nó truy cập các site trên server của bạn. Ví dị ta muốn chặn địa chỉ `192.168.213.100`

`webinoly -blockip=192.168.213.100`

Để hiển thị các IP bị chặn

`webinoly -blockip -list -raw`

Để bỏ chặn một IP

`webinoly -blockip=192.168.213.100 -purge`

### Xem log
Xem log access của website

`log wp.namdac.com`

Mặc định log access sẽ bị tắt. Để cho phép ghi log này bạn cần dùng lệnh sau

`log wp.namdac.com -only-error=off`

Bạn cũng có thể xem các log của các dịch vụ khác ví dụ như `php` `ssh` hay `mail`

`log -ssh`

`log -php`

`log-mail`

### Update version mới nhất của webinoly

`webinoly -update`


