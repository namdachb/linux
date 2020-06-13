# Cài đặt Wordpress với LEMP (CentOS 7)
Trước tiến chúng ta tìm hiểu LEMP và Wordpress là gì?

**LEMP** là viết tắt của các khái niệm :
 * **L** : Linux (ở đây chúng ta sử dụng CentOS 7x) là hệ điều hành mã nguồn mở được sử dụng chủ yếu trên các server để phục vụ nhiều mục đích khác nhau
 * **E** : Nginx là phần mềm máy chủ web cũng tương tự Apache nhưng có sức chịu tải lớn hơn rất nhiều so với Apache nên thường được sử dụng trên các ứng dụng web rất đông người truy cập và bitly.com là một trong số đó. Nginx được phát âm là “engine-x” nên được viết tắt thành chữ E trong LEMP.
 * **M** : MySQL là hệ quản trị cơ sở dữ lieuej mã nguồn mở rất phổ biến do tính năng bảo mật, dễ sử dụng và miễn phí. MySQL thường được dùng để lưu trữ dữ liệu cho các ứng dụng web và thường dùng chung với PHP
 * **P** : PHP là ngôn ngữ lập trình kịch bản mã nguồn, chủ yếu được dùng để phát triển các ứng dụng web trên phía máy chủ

**Wordpress**  là một ứng dụng viết mã nguồn mở và miễn phí và một CMS động (Hệ thống Quản lý Nội dung) được phát triển sử dụng MySQL và PHP

Trong bài này, mình sẽ giới thiệu cho các bạn cách cài đặt WordPress bằng cách sử dụng LEMP (Linux, Nginx, MariaDB, PHP) trên RHEL/ CentOS 7

## Cài đặt và cấu hình LEMP
Trước khi tiến hành cài đặt WordPress, chúng ta cần cài LEMP trên máy của chúng ta. Và cách cài LEMP tại [đây](https://github.com/namdachb/linux/blob/master/Wordpress-LEMP/LEMP-centos.md)

## Tạo cở sở dữ liệu và tài khoản cho WordPress
Ở bước cài LEMP, mình đã cài mariadb cho cơ sở dữ liệu. Chúng ta cũng có thể thao tác tương tự với MySQL. Đầu tiên, chúng ta cần đăng nhập vào tài khoản root của cơ sở dữ liệu bằng câu lệnh

`mysql -u root -p`

Chúng ta cần nhập password mà chúng ta đã thiết lập lúc cài mariadb

Tiếp theo ta cần tạo cơ sở dữ liệu cho wordpress. Chúng ta có thể sử dụng 1 cái tên bất kỳ. Trong bài này, mình sẽ đặt là : `namdac`

`CREATE DATABASE namdac;`

Chúng ta cần tạo 1 tài khoản riêng để quản lý cơ sở dữ liệu cho wordpress. Trong bài minh sẽ đặt user là `admin` và mật khẩu là `pass`

`CREATE USER admin@localhost IDENTIFIED BY 'pass';`

Tiến hành cấp quyền quản lý cơ sở dữ liệu wordpress cho user mới tạo

`GRANT ALL PRIVILEGES ON namdac.* TO admin@localhost IDENTIFIED BY 'pass';`

Sau đó xác thực lại những thay đổi về quyền:

`FLUSH PRIVILIGES;`

Sau khi hoàn tất, thoát khỏi mariadb:

 `exit`

## Tải và cài đặt WordPress
Trước khi bắt đầu tiến hành cài gói hỗ trợ php-gd:

`yum -y install php-gd`

Tiến hành tải xuống WordPress với phiên bản mới nhất

`wget http://wordpress.org/latest.tar.gz`

Tiến hành giải nén file `latest.tar.gz`:

`tar xvfz latest.tar.gz`

Copy các file trong thư mục WordPress tới đường dẫn `/user/share/nginx/html` :

`cp -Rvf /root/wordpress/* /usr/share/nginx/html`

## Cấu hình WordPress
Ta di chuyển đường dẫn tới thư mục chứa các file cài đặt WordPress như sau:

`cd /usr/share/nginx/html`

File cấu hình wordpress là `wp-config.php`. Tuy nhiên tại đây chỉ có file `wp-config-sample.php`. Tiến hành copy tại file cấu hình như sau:

`cp wp-config-sample.php wp-config.php`

Mở file config với `vi` để sửa

`vi wp-config.php`

Trong file này, ta tìm tới dòng như hình dưới

![Imgur](https://i.imgur.com/hmUC3CY.png)

Tiến hành thay đổi thông tin cơ sở dữ liệu, tài khoản, mật khẩu như đã thiết lập trước đó

![Imgur](https://i.imgur.com/dQI8s24.png)

Gõ ESC -> :wq để thoát là lưu

## Hoàn tất phần cài đặt giao diện
Trên trình duyệt, gõ địa chỉ IP server, trình duyệt sẽ xuất hiện như sau:

![Imgur](https://i.imgur.com/EgLfrjl.png)

Đăng nhập bằng **Tên người dùng** và **Mật khẩu** đã tạo ở bước trên

![Imgur](https://i.imgur.com/Kxaexki.png)

Như vậy là đã thành công cài đặt và cấu hình WordPress trên CentOS 7 bằng LEMP

![Imgur](https://i.imgur.com/zH9y6EP.png)