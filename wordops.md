## Tổng quan WordOps

### WordOps là gì?
WordOps là một trình cài đặt tự động dựa trên **EasyEngine**. Nó sẽ tự động cài đặt trang web cho bạn. Nó đi kèm với nhiều tính năng khác và được tích hợp nhiều công cụ khác nhau có thể cải thiện hiệu suất và bảo mật của trang web

WordOps là một công cụ dòng lệnh giúp dễ dàng quản trị máy chủ bằng cách cung cấp khả năng thiết lập năng xếp LEMP được tối ưu hóa (Nginx,PHP,MySQL) và triển khai các trang web WordPress bằng các lệnh đơn giản. Nó cũng đi kèm với bảng điều khiển web nơi các số liệu máy chủ chính được hiển thị với các liên kết đến tất cả các công cụ khác được cung cấp với WordOps

### Các tính năng chính của WordOps

 * Hỗ trợ cài đặt stack lemp bao gồm Mariadb, Nginx, Php
 * Dễ dàng quản lý các Vhost nginx cũng như database, cài đặt website wordpress cùng với SSL chỉ trong một dòng lệnh
 * Hỗ trợ multicache wordpress sử dụng plugin hoặc không cần sử dụng plugin
 * WordOps tích hợp fastcgi_cache và Redis cache, WP-Super-Cache, Cache-Enabler và WP-Rocket
 * WordOps dựa trên Nginx được tối ưu hóa và bảo mật thư mục website

### Những hệ điều hành được WordOps hỗ trợ gồm có
 * Ubuntu TLS (Long Term Service) releases (16.04 & 18.04)
 * Debian 9 (stretch)
 * Debian 10 (buster)
 * Raspbian 9 (stretch)
 * Raspbian 10 (buster)

## Cài đặt wordops trên ubuntu 18.04
WordOps có thể cài đặt trên các thiết bị cấp thấp như Raspberry PI với yêu cầu tối thiểu là:
 * 100 MB dung lượng trống
 * RAM 512MB

### Cài đặt
#### Cài đặt trang web
**Tải về các phụ thuộc**

Để làm việc với WordOps rất đơn giản vì đã được cung cấp 1 tập lệnh cài đặt để cài đặt phụ thuộc cần thiết, chạy câu lệnh sau để cài đặt

`wget -pO wo wops.cc && sudo bash wo`

![Imgur](https://i.imgur.com/pBXsuXv.png)

 * Nhập tên của bạn
 * Nhập địa chỉ email của bạn

**Kích hoạt bash_completion**

Để bật tự động hoàn thành các lệnh WordOps, hãy chạy lệnh sau, sau khi cài đặt WordOps

`source /etc/bash_completion.c/wo_auto.rc`

**Cài đặt WordOps stacks**
Bạn có thể lựa chọn để cài đặt danh sách các thành phần WordOps (Nginx, php,mysql,..) hoặc không cài và đến khi thiết lập trang web cần các phụ thuộc nào thì WordOps sẽ tự động cài thành phần đó

`wo stack install`

Chờ đến khi lệnh chạy xong, WordOps sẽ cài đặt các thành phần như sau

```root@namdac:~# wo stack install
WP-CLI is already installed
Start : wo-kernel [OK]
Adding repository for MySQL, please wait...
Adding repository for NGINX, please wait...
Adding repository for PHP, please wait...
Updating apt-cache              [OK]
Installing APT packages         [OK]
Applying Nginx configuration templates
Testing Nginx configuration     [OK]
Restarting Nginx                [OK]
Testing Nginx configuration     [OK]
Restarting Nginx                [OK]
Configuring php7.3-fpm
Restarting php7.3-fpm           [OK]
Tuning MariaDB configuration
Stop  : mysql     [OK]
Start : mysql     [OK]
Restarting fail2ban             [OK]
Configuring Fail2Ban
Reloading fail2ban              [OK]
Downloading PHPMyAdmin           [Done]
Downloading phpRedisAdmin        [Done]
Downloading Composer             [Done]
Downloading Adminer              [Done]
Downloading Adminer theme        [Done]
Downloading MySQLTuner           [Done]
Downloading Netdata              [Done]
Downloading WordOps Dashboard    [Done]
Downloading eXtplorer            [Done]
Downloading cheat.sh             [Done]
Downloading bash_completion      [Done]
Downloading clean.php            [Done]
Downloading opcache.php          [Done]
Downloading Opgui                [Done]
Downloading OCP.php              [Done]
Downloading Webgrind             [Done]
Downloading pt-query-advisor     [Done]
Downloading Anemometer           [Done]
Installing composer             [OK]
Installing Netdata              [OK]
Restarting netdata              [OK]
Configuring packages            [OK]
HTTP Auth User Name: WordOps
HTTP Auth Password : yGCyV7IrSxzpWQuU9ziX2MZC
WordOps backend is available on https://117.4.255.125:22222 or https://namdac:22222
Successfully installed packages
```