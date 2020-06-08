# Cài LAMP trên CentOS 7
LAMP là một hệ thống các phần mềm để tạo dựng môi trường máy chủ web được viết bằng PHP

## 1. Tổng quan
LAMP là một hệ thống các phần mềm để tạo dựng môi trường máy chủ web có khả năng chứa và phân phối các trang web động được viết bằng PHP

LAMP bao gồm :
 * **Linux** : là hệ điều hành, cũng là phần mềm dùng để điều phối và quản lí các tài nguyên của hệ thống
 * **Apache** : tên chính thức là Apache HTTP Server - đây là một phần mềm web server miễn phí có mã nguồn mở, có thể thực hiện các request (yêu cầu) được gọi tới máy chủ thông qua giao thức HTTP
 * **Mysql/Mariadb** : là hệ quản trị cơ sở dữ liệu giúp lưu trữ và truy xuất dữ liệu. Cả 2 hệ quản trị cơ sở dữ liệu này khá tương đồng với nhau (có thể đọc thêm tại [đây](https://www.eversql.com/mariadb-vs-mysql/))
 * **PHP** : PHP là ngôn ngữ kịch bản phía máy chủ được thiết kế để phát triển web nhưng cũng được sử dụng làm ngôn ngữ lập trình cho mục đích chung

## 2. Tiến hành cài đặt
### 2.1 Cài đặt Linux
Đối với việc vài đặt hệ điều hành, chúng ta có thể hiện trên nhiều bản phân phối khác nhau của linux như Debian, Redhat, Ubuntu... Trong bài này, mình sử dụng hệ điều hành CentOS 7

### 2.2 Cài đặt Apache
Để cài đặt, trên cửa sổ terminal gõ lệnh :
 
 `sudo yum -y install httpd`

Cài xong, tiến hành khởi động lại service :
  
 ```
 systemctl start httpd
 systemctl enable httpd
 ```

Kiểm tra phiên bản Apache vừa cài đặt 
```
[root@localhost ~]# httpd -v
Server version: Apache/2.4.6 (CentOS)
Server built:   Apr  2 2020 13:13:23
```

Bạn có thể check lại trạng thái hoạt động của service bằng cách gõ :
 
 `systemctl status httpd`

 ![Imgur](https://i.imgur.com/Lj3fDiN.png)
 
Chúng ta có thể kiểm tra trạng thái trên trình duyệt bằng cách gõ trên thanh url địa chỉ sau:
 
 `<địa chỉ ip server>`

 ![Imgur](https://i.imgur.com/bRQopS1.png)
Nội dung file web mặc định khi bạn truy cập bằng địa chỉ IP **web server apache** nằm ở thư mục: **/var/www/html/**

**File cấu hình Apache**

Ở bài viết này chúng ta không đi chi tiết việc tìm hiểu cấu hình nâng cao dịch vụ Apache. Mà chỉ tìm hiểu cài đặt cơ bản vì vậy bạn cần biết một số thông tin sau :
 * **File cấu hình Apache**: /etc/httpd/conf/httpd.conf
 * **Thư mục chứa cấu hình phụ Apache**: /etc/httpd/conf.d/
 * **Thư mục log Apache**: /var/log/httpd/
 * **Thư mục web mặc định**: /var/www/html/

Nếu sử dụng hệ điều hành trên máy ảo, chúng ta có thể tắt **firewall** để có thể truy cập trên browser của máy thực :

 `systemctl stop firewalld`

Sau đó, gõ địa chỉ ip máy ảo trên thanh url cũng sẽ cho ra kết quả tương tự

### 2.3 Cài đặt hệ quản lí cơ sở dữ liệu
Trên thực tế với LAMP, chúng ta có thể sử dụng mysql hoặc mariadb đều được, bài này chúng ta hướng dẫn với **mariadb**

Trên cửa sổ terminal, tiến hành cài đặt mariadb:

 `sudo yum -y install mariadb mariadb-server`

Tiến hành khởi động mariadb service:
 
 `systemctl start mariadb`

Cài đặt mật khẩu cho quyền root của cơ sở dữ liệu:

 `sudo mysql_secure_installation`
 ![Imgur](https://i.imgur.com/3foYflJ.png)

Ở bước này ta sẽ thiết lập một số cấu hình như sau:

 `Enter current password for root (enter for none):`

Bước này yêu cầu chúng ta nhập mật khẩu gần đây cho root. Nếu mới cài lần đầu thì nhần Enter để bỏ qua

 `Set root password? (Y/n)`

Nếu bạn cài lần đầu, hệ thống sẽ hỏi bạn muốn cài password cho quyền root không. Chúng ta gõ **Y->Enter**, sau đó nhập mật khẩu và xác thực mật khẩu

![Imgur](https://i.imgur.com/iR5OFIU.png)

Với những máy mới cài mariadb lần đầu, hệ thống yêu cầu thêm một số thiết lập như sau:
 * Xóa bỏ các user khác
 * Không cho phép root đăng nhập từ xa
 * Xóa bỏ databases test
 * Khởi chạy lại bảng Privillege (bảng phân quyền)

Bạn chỉ cần gõ **Y** cho những yêu cầu đó

Sau khi thiết lập xong, kích hoạt mariadb để khởi động cùng hệ thống:

 `systemctl enable mariadb`

### 2.4 Cài đặt php
Phiên bản có sẵn trong repo của CentOS đang là 5.4. Phiên bản này khá cũ và sẽ khiến bạn gặp 1 số vấn đề xảy ra khi tiến hành cài đặt wordpress. Vì vậy chúng ta cần phải cài đặt phiên bản 7x để khắc phục. Chúng ta cần tiến hành thêm kho vào Remi CentOS:

 `sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm`

Cài yum -utils vì chúng ta cần tiện ích yum-config-manager để cài đặt:

 `yum -y install yum-utils`

Tiến hành cài đặt php. Ở đây ta cần lưu ý về phiên bản cài đặt như sau:
  * Bản 7.0 :
```
yum-config-manager --enable remi-php70
yum -y install php php-opcache php-mysql
```

  * Bản 7.1 :
```
yum-config-manager --enable remi-php71
yum -y install php php-opcache php-mysql
```

  * Bản 7.2 :
```
yum-config-manager --enable remi-php72
yum -y install php php-opcache php-mysql
```

  * Bản 7.3 :
```
yum-config-manager --enable remi-php73
yum -y install php php-opcache php-mysql
```

Trong bài này, mình sử dụng phiên bản 7.3

Sau khi cài đặt xong, thực hiện restart lại apache :

 `systemctl restart httpd`

Tiến hành kiểm tra kết quả. Ta thêm file sau :

 `echo "<?php phpinfo();?>" > /var/www/html/info.php`

Sau đó restart lại apache :

 `systemctl restart httpd`

Vào trình duyệt, gõ trên thanh url địa chỉ sau:
 
 `<địa chị ip>/info.php`

![Imgur](https://i.imgur.com/X7b9ci1.png)

