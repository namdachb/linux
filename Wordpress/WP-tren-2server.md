# Cài đặt WordPress trên 2 server bằng CentOS 7
WordPress là công cụ giúp ta tạo ra các trang website như: blog cá nhân, trang tin tức, trang thương mại hoặc chia sẻ mã nguồn mở . Nó dựa trên PHP và MySQL. Các tính năng của nó có thể được mở rộng với hàng ngàn plugin và chủ đề miễn phí

### Đặc điểm
 * Dễ sử dụng, cộng đồng hỗ trợ lớn
 * Bảo mật nội dung bằng mật khẩu
 * Nhiều websites cùng lúc : Phát triển , bảo trì nhiều trang web trong khi chỉ sử dụng 1 cài đặt WordPress
 * WordPress tự động lưu các nội dung chúng ta làm việc vì vậy không sợ mất mát dữ liệu khi gặp sự cố máy tính (trừ khi trên máy local)
 * Xuất bản nội dung trên các phương tiện khác nhau : WordPress ngoài phiên bản dành cho máy tính còn có các phiên bản dành cho các thiết bị di động
 * Tính năng xem trước bài viết giúp ta chỉnh sửa dễ dàng
 * Ngôn ngữ đa dạng

## Cài đặt WordPress

![Imgur](https://i.imgur.com/f8oOMzu.png)

Trong đó
 * IP 192.168.213.175 được sử dụng để ra ngoài internet
 * Dải IP 192.168.11.0/24 là dải IP local không thể ra được internet 

Tiến hành tắt tường lửa và selinux trên cả 2 máy Web và Sql:
```
[root@web ~]# systemctl stop firewalld
[root@web ~]# setenforce 0
```

## Cấu hình
### Trên máy SQL
### 1. Cài đặt MYSQL Server
Cài đặt dịch vụ MySQL

 `yum install wget`

```
wget http://repo.mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-server
```

Khởi động dịch vụ MySQL
 
 `systemctl start mysqld`

Chạy lệnh sau để thiết lập tài khoản `root` cho MySQL

 `mysql_secure_installation`

### 2. Tạo database cho WordPress
MySQL sử dụng cơ sở dữ liệu sql để lưu trữ dữ liệu của mình, nhưng để trang web hoạt động được ta cần tạo người dùng và cơ sở dữ liệu cho trang web của mình 

Để bắt đầu, ta đăng nhập vào MySQL bằng tài khoản `root` bằng lệnh sau:

```
[root@mysql ~]# mysql -u root -p
Enter password:
```

Nhập mật khẩu `root` tạo ở trên và nó sẽ chuyển đến dấu nhắc lệnh của MySQL

```
[root@mysql ~]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 5.6.48 MySQL Community Server (GPL)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

#### Tạo user và database sử dụng cho WordPress
Tạo tên cơ sở dữ liệu:
```
mysql> create database nam;
Query OK, 1 row affected (0.00 sec)
```

   * **Lưu ý** : Cuối mỗi câu lệnh của MySQL luôn kết thúc bằng dấu `;`

Tạo user và mật khẩu :
```
mysql> create user 'user'@'192.168.11.42' identified by 'pass';
Query OK, 0 rows affected (0.00 sec)
```

  * 192.168.11.42 : là đị chỉ của máy Web truy cập MySQL
  * user : là user để wordpress sử dụng để đăng nhập vào mysql
  * pass : là mật khẩu của user

Tiếp theo, ta set quyền cho user để có quyền truy cập vào cơ sở dữ liệu
```
mysql> GRANT ALL PRIVILEGES ON nam.* TO 'user'@'192.168.11.42' IDENTIFIED By 'pass';
Query OK, 0 rows affected (0.01 sec)
```

Bây giờ user đã có quyền truy cập vào cơ sở dữ liệu, thực hiện lệnh `flush privileges;`để MySQL cập nhật thay đổi:
```
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

```
mysql> exit
Bye
```

## Trên máy Web
### 1. Cài Apache
Apache là 1 phần mềm server http phổ biến và tồn tại lâu nhất (từ năm 1995), ta dễ dàng cài đặt Apache bằng trình quản lý gói (a repository maintained) của CentOs – Yum

Để cài đặt **Apache** ta sử dụng lệnh sau :

 `yum install httpd -y`

Sau khi cài đặt, ta khởi động dịch vụ **Apache** và cho phép nó khởi động cùng hệ thống :
```
systemctl start httpd
systemctl enable httpd
```

Để kiểm tra ta mở trình duyệt và truy cập vào địa chỉ ip của web server 

 `http://địa-chỉ-ip`

Kết quả trả về như sau:

![Imgur](https://i.imgur.com/7lSVtSD.png)

### 2. Cài PHP
Dịch vụ php để chạy các tập lệnh, kết nối với cơ sở dữ liệu MySQL để lấy thông tin và đưa nội dung được xử lý đến Web server để hiển thị

Ở đây mình muốn cài đặt php phiên bản 7.3 nên trước tiên ta phải bật kho lưu trữ **EPEl** và **REMI** cho hệ thống centos 7 bằng câu lệnh:

```
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

Tiếp theo cài đặt `yum-util` để tăng cường các chức năng như cung cấp cho nó các tùy chọn quản lý gói nâng cao và cũng giúp sử dụng dễ dàng hơn

 `yum install yum-utils`

Sau khi cài đặt xong, ta tiến hành cài đặt **php 7.3** với những lệnh sau:

```
yum -config-manager --enable remi-php73
yum install php php-mysql php-gd php-pear-y
```

Kiểm tra xem php đã được cài chưa bằng lệnh

 `php-v`

Nhập lệnh sau để tạo 1 tệp php :

 `echo "<?php phpinfo(); ?>" > /var/www/html/info.php`

Bây giờ ta cần khởi động lại dịch vụ Apache để nó cập nhật module mới :

 `systemctl restart httpd`

Vào trình duyệt Web và truy cập địa chỉ `http://địa_chỉ_ip/info.php`, 

### 3. Cài WordPress
Trước tiên, ta truy cập vào thư mục `/var/www/html/` sau đó tiến hành download Wordpress từ internet vào thư mục này để tránh việc phải sao chép lại thư mực wordpress vào đây

```
cd /var/www/html
wget https://wordpress.org/latest.tar.gz
```

Sau khi tải về nó là 1 tập tin nén, ta tiến hành giải nén tập tin

`tar xzvf latest.tar.gz`

Tiếp theo, trước khi định cấu hình cho WordPress ta cần đổi tên nó thành `wp-config.php`. Thực hiện điều đó như sau:

```
mv wordpress/* /var/www/html/
mv wp-config-sample.php wp-config.php
```

Bây giờ ta sẽ cấu hình bằng cách thay đổi 1 số thông tin cập nhật theo cơ sở dữ liệu trong file `wp-config.php`

`vi /var/www/html/wp-config.php`

Dưới đây là các giá trị cần cập nhật cho cơ sở dữ liệu :

![Imgur](https://i.imgur.com/hmUC3CY.png)

Theo như dữ liệu đã thiết lập ở MySQL ta thay đổi các thông số như sau:

```
define( 'DB_NAME', 'nam' );
define( 'DB_USER', 'user' );
define( 'DB_PASSWORD', 'pass' );

define( 'DB_HOST', '192.168.11.40' );

```

lưu và thoát ra (ESC-> :wq)

  * Lưu ý : 192.168.11.40 là địa chỉ ip localhost của sql

Tiếp theo ta tiến hành tắt cổng IFace **ens33** trên máy sql để máy không thể đi ra ngoài mạng và chỉ sử dụng để kết nối trong local