## LEMP là gì?
LEMP là một nhóm các phầm mềm có thể dùng để phục vụ các wed động được viết bằng PHP. LEMP bao gồm một hệ điều hành Linux, web server Nginx, hệ quản trị cơ sở dữ liệu MySql hoặc MariaDB và ngôn ngữ lập trình PHP. Nó tương tự như LAMP server ngoại trừ việc web server nền tảng được giám sát bằng Nginx thay vì Apache

### Nginx
**Nginx** đọc là (engine-x) là một phần mềm web server mã nguồn mở nổi tiếng. Ban đầu nó dùng để phục vụ wen HTTP. Tuy nhiên, ngày nay nó cũng được dùng làm reverse proxy, HTTP load balancer và email proxy như IMAP, POP3 và SMTP

### NGINX hoạt động như thế nào?
**nginx** được phát triển cho các mục đích tối ưu sử dụng (ram) bộ nhớ thấp nhưng phục vụ được nhiều kết nối đồng thời cao hơn. **nginx** sử dụng kiến trúc hướng sự kiện (event-driven) không đồng bộ (asynchronous) và có khả năng mở rộng. Ngay cả khi bạn không cần phải xử lý hàng ngàn truy vấn đồng thời, thì bạn vẫn nên sử dụng nginx do hiệu xuất cao và yêu cầu bộ nhớ thấp của nginx so với apache

## Cách cài đặt LEMP trên CentOS 7
Chuẩn bị một máy cài hệ điều hành CentOS 7. Sử dụng user root để cài đặt

### Bước 1: Cài đặt Nginx
Để hiển thị các trang web cho khách truy cập trang web của chúng ta, chúng ta sẽ sử dụng Nginx, một máy chủ web hiệu suất cao. Để có phiên bản Nginx mới nhất, trước tiên chúng ta cần cài đặt kho EPEL, chứa phần mềm bổ sung cho hệ điều hành CentOS 7

Để thêm kho lưu trữ CentOS 7 EPEL, sử dụng lệnh sau :

 `yum install epel-release`

Kho lưu trữ EPEL đã được cài đặt trên máy chủ của chúng ta, hãy cài đặt nginx bằng lệnh sau :

 `yum install nginx`

Sau khi cài đặt xong, hãy bắt đầu dịch vụ Nginx với:

 `systemctl start nginx`

Chúng ta có thể check ngay lập tức để xác minh liệu rằng hệ thống có hoạt động đúng theo mong đợi không bằng cách truy cập địa chỉ IP công khai trên trình duyệt

 `httpd://server_domain_name_or_IP/`

Chúng ta sẽ thấy trang web CentOs 7 Nginx mặc định, có sẵn cho mục đích thông tin và thử nghiệm. Nó sẽ trông như thế này:

![Imgur](https://i.imgur.com/xQhzpLL.png)

Nếu thấy trang này, thì máy chủ của chúng ta đã được cài đặt chính xác

Để cho phép Nginx khởi động khi khởi động, hãy chạy lệnh sau:

 `systemctl enable nginx`

### Tìm địa chỉ IP công khai 
Nếu bạn chưa biêt địa chỉ IP công khải của bạn là gì thì có khá nhiều cách để tìm ra, và đây là địa chỉ mà bạn dùng để kết nối với server thông qua SSH

 `ip addr show ens33 | grep inet | awk '{ print 2; }' | sed 's/\/.*$//'`

Kết quả trả về có thể là một hoặc hai địa chỉ IP , thế nhưng chúng ta chỉ cần chọn một thôi là được

Một cách khác là sử dụng bên thứ ba để truy cập một server cụ thể để lấy địa chỉ IP của nó

 `curl http://icanhazip.com`

Cả hai cách đều thao tác trên thanh địa chỉ trên trình duyệt của chúng ta

### Bước 2: Cài MySQL (MariaDB)
Tiếp theo là cài MariaDB, một công cụ thay thế cho MySQL. MariaDB là một nhánh của hệ thống quản lý cơ sở dữ liệu liên quan đến MySQL. Căn bản là nó sẽ điều khiển và cung cấp chức năng truy cập tới hệ cơ sở dữ liệu của site mà có thể lưu trữ thông tin trong đó

 `yum install mariadb-server mariadb`

Sau đó khởi động lại MariaDB:
 
 `systemctl start mariadb`

Giờ thì MariaDB đang hoạt động, ta sẽ chạy thêm tập lệnh bảo mật để loại bỏ các mã độc gây ảnh hưởng tới ứng dụng và ngăn chặn khả năng truy cập tới hệ cơ sở dữ liệu

 `mysql_secure_installation`

Bạn sẽ được hỏi password của root. Nếu bạn mới cài mariadb thì bạn cần `enter` để bỏ qua. Để đảm bảo tính bảo mật bạn nên đặt password cho tài khoản `root`

```
[root@localhost ~]# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!
```

Bây giờ đăng nhập thử với user `root` và paasssword đặt ở bên trên

```
[root@localhost ~]# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 10
Server version: 5.5.65-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

Cuối cùng là hữu hiệu hóa MariaDB để bắt đầu nạp vào bộ nhớ :
 
 `systemctl enable mariadb`

### Bước 3: Cài đặt PHP
PHP là thành phần thiết lập để xử lý mã để hiển thị nội dung động. Nó có thể chạy các tập lệnh, kết nối với cơ sở dữ liệu MySQL để lấy thông tin và trao nội dung được xử lý cho máy chủ web để hiển thị

Phiên bản PHP có sẵn theo mặc định trong các máy chủ CentOS 7 đã lỗi thời và vì lý do đó, chúng ta sẽ cần cài đặt kho lưu trữ gói của bên thứ ba để có được PHP 7+ và cài đặt nó trên máy chủ CentOS 7 của bạn. Remi là kho lưu trữ gói phổ biến cung cấp các bản phát hành PHP cập nhật nhất cho các máy chủ CentOS

Để cài đặt khi Remi cho CentOS 7, dùng lệnh:

```
yum install yum-utils
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

Sau khi cài đặt xong, chúng ta sẽ cần chạy một lệnh để kích hoạt kho lưu trữ chứa phiên bản PHP ưa thích của chúng ta. Để kiểm tra bản phát hành PHP 7+ nào có sẵn trong kho Remi, hãy chạy:

`yum --disablerepo="*" --enablerepo="dremi-safe" list php[7-9][0-9].x86_64`

Chúng ta sẽ thấy đầu ta như sau :

```
[root@localhost ~]# yum --disablerepo="*" --enablerepo="remi-safe" list php[7-9][0-9                                                                                  ].x86_64
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * remi-safe: mirror.telkomuniversity.ac.id
remi-safe                                                    | 3.0 kB  00:00:00
remi-safe/primary_db                                         | 1.7 MB  00:00:00
Available Packages
php70.x86_64                        2.0-1.el7.remi                         remi-safe
php71.x86_64                        2.0-1.el7.remi                         remi-safe
php72.x86_64                        2.0-1.el7.remi                         remi-safe
php73.x86_64                        2.0-1.el7.remi                         remi-safe
php74.x86_64                        1.0-3.el7.remi                         remi-safe
php80.x86_64                        1.0-3.el7.remi                         remi-safe
```

Mình sẽ cài dặt PHP7.4, để kích hoạt gói Remi hĩnh xác để cài đặt PHP7.4:

`yum-config-manager --enable remi-php74`

Cài đặt bao gồm cả gói php-mysql và php-fpm:

`yum install php php-mysql php-fpm`

Để xem phiên bản PHP :

`php -v`

Tiếp theo
