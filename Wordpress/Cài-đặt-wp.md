# Cài đặt WordPress
WordPress là một hệ thống quản lý nội dung miễn phí và mã nguồn mở phổ biến

## 1. Tổng quan
WordPress là một hệ thống quản lí nội dung miễn phí và mã nguồn mở xây dựng dựa trên PHP và Mysql. Được phát hành vào năm 2003, đến nay WordPress đã trở thành một trong những hệ thống quản lí website phổ biến nhất thế giới

Trong bài này, mình sẽ cài đặt WordPress trên hệ điều hành CentOS 7

## 2. Các bước cài đặt
### Bước 1 : chuẩn bị
Trước khi tiến hành cài đặt WordPress, chúng ta cần phải cài bộ LAMP trên máy của chúng ta . Đây là cách cài đặt [LAMP](https://github.com/namdachb/linux/blob/master/Wordpress/LAMP.md)

### Bước 2 : tạo cơ sở dữ liệu và tài khoản cho WordPress
Ở bước chuẩn bị, mình đã cài mariadb cho cơ sở dữ liệu. Chúng ta cũng có thể thao tác tương tự với MySQL. Đầu tiên, ta cần đăng nhập vào tài khoản root của cơ sở dữ liệu bằng câu lệnh :

 `mysql -u root -p`

Chúng ta cần nhập password mà chúng ta đã thiết lập lúc cài đặt mariadb. Khi nhập xong, terminal sẽ chuyển sang mariadb

Tiếp theo chúng ta cần tạo cơ sở dữ liệu cho wordpress. Chúng ta có thể sử dụng một cái tên bất kỳ . Trong bài này, mình sẽ đặt là **namdac**

 `CREATE DATABASE namdac;`

Chúng ta cần tạo một tài khoản riêng để quản lí cơ sở dữ liệu cho WordPress. Trong bài mình sẽ đặt tên  cho tài khoản là **admin** và mật khẩu là **pass**, như sau:
 
 `CREATE USER admin@localhost IDENTIFIED BY 'pass';`

Tiến hành cấp quyền quản lý cơ sở dữ liệu wordpress cho user mới tạo 

 `GRANT ALL PRIVILEGES ON namdac.* TO admin@localhost IDENTIFIED BY 'pass';`

Sau đó xác thực lại những thay đổi về quyền:

 `FLUSH PRIVILEGES;`

Sau khi hoàn tất, thoát khỏi mariadb:
 
 `exit`

### Bước 3: Tải và cài đặt WordPress
Trước khi bắt đầu tiến hành cài gói hỗ trợ php-gd:
 
 `yum -y install php-gd`

Tiến hành tải xuống WordPress với phiên bản mới nhất

 `wget http://wordpress.org/latest.tar.gz`

 > Lưu ý: chúng ta cần để ý tới thư mục đang lưu trữ file wordpress đang được tải xuống. Ở đây mình lưu tại thư mục /root

Tiến hành giải nén file `latest.tar.gz`:

 `tar xvfz latest.tar.gz`

 > Lưu ý: giải nén sẽ ra thư mục wordpress có đường dẫn /root/wordpress 

Copy các file trong thư mục WordPress tới đường dẫn `/var/www/html` như sau:

 `cp -Rvf /root/wordpress/* /var/www/html

### Bước 4: Cấu hình WordPress
Ta di chuyển đường dẫn tới thư mục chứa các file cài đặt WordPress như sau :

 `cd /var/www/html`

File cấu hình wordpress là `wp-config.php`. Tuy nhiên tại đây chỉ có file `wp-config-sample.php`. Tiến hành copy tại file cấu hình như sau:

 `cp wp-config-sample.php wp-config.php`

Mở file config với vi để sửa   

 `vi wp-config.php`

Trong file này, ta tìm tới dòng như hình dưới đây

![Imgur](https://i.imgur.com/hmUC3CY.png)

Tiến hành thay đổi thông tin cơ sở dữ liệu, tài khoản, mật khẩu như đã thiết lập ở bước 2. Ví dụ như sau :

![Imgur](https://i.imgur.com/dQI8s24.png)

Gõ ESC -> :wq để lưu thoát khỏi chế độ chỉnh sửa

### Bước 5: Hoàn tất phần cài đặt giao diện
Trên trình duyệt, gõ địa chỉ ip server trên thanh url, trình duyệt sẽ xuất hiện như sau :

![Imgur](https://i.imgur.com/if8s2MA.png)

Tiến hành nhập một số thông tin cần thiết rồi chọn `Install WordPress`

![Imgur](https://i.imgur.com/Pwebyrv.png)

Như vậy là chúng ta đã thiết lập thành công. Tiến hành đăng nhập vào WordPress:

![Imgur](https://i.imgur.com/y7Y4DTh.png)

Như vậy là chúng ta đã có thể sử dụng WordPress rồi

### Bước 6: Phân quyền cho thư mục wordpress
Khi chúng ta thực hiện thao tác upload ảnh hay đăng các bài viết, sẽ xuất hiện lỗi

Ta cần tiến hành phân quyền thư mục wordpress cho user apache để cho user này được phép tạo các thư mục và lưu các tệp tải lên. Trên cửa sổ terminal, ta gõ lệnh như sau:
```
chown -R apacha:apache /var/www/html/*
chmod -R 755 /var/www/html/*
```

Như vậy là bạn đã có thể tiến hành upload ảnh và đăng bài viết lên trang wordpress của bạn