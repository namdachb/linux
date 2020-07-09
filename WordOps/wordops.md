## Tổng quan WordOps

### WordOps là gì?
WordOps là một trình cài đặt tự động dựa trên **EasyEngine**. Nó sẽ tự động cài đặt trang web cho bạn. Nó đi kèm với nhiều tính năng khác và được tích hợp nhiều công cụ khác nhau có thể cải thiện hiệu suất và bảo mật của trang web

WordOps là một công cụ dòng lệnh giúp dễ dàng quản trị máy chủ bằng cách cung cấp khả năng thiết lập năng xếp LEMP được tối ưu hóa (Nginx,PHP,MySQL) và triển khai các trang web WordPress bằng các lệnh đơn giản. Nó cũng đi kèm với bảng điều khiển web nơi các số liệu máy chủ chính được hiển thị với các liên kết đến tất cả các công cụ khác được cung cấp với WordOps

WordOps cung cấp khả năng triển khai WordPress nhanh và bảo mật với Nginx bằng cách sử dụng lệnh đơn giản và dễ nhớ. Được bổ sung từ EasyEnginee v3, nó đã hơn nhiều so với phiên bản EEv3 cập nhật với một số tính năng mới bao gồm chứng chỉ SSL mã hóa ký tự đại diện với hỗ trợ xác thực API DNS, tối ưu hóa nhân Linux hoặc gói Nginx tùy chỉnh mới với TLS vl.3 và hỗ trợ HPACK của Cloudflare HTTP /2

WordOps là dự án mã nguồn mở được phát triển với mục đích tự động hóa cấu hình máy chủ web. WordOps là tập hợp các kịch bản python cung cấp tự động hóa cho việc cài đặt máy chủ web, tạo trang web, gỡ lỗi và giám sát dịch vụ
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

### Điều kiện tiên quyết 
**Yêu cầu phần cứng**

**Tài nguyên**

**Tối thiểu**

WordOps rất nhẹ, nó không đòi hỏi nhiều tài nguyên và có thể được cài đặt trên các thiết bị cấp thấp như Raspberry PI. Yêu cầu tối thiểu là
 * ~100 MB dung lượng lưu trữ
 * RAM 512MB

**Đề xuất**

Tuy nhiên, nếu bạn định sử dụng WordOps trong sản xuất, một số dịch vụ như MySQL hoặc Redis có thể cần nhiều tài nguyên hơn và chạy các ngăn xếp WordOps mà không có đủ tài nguyên có thể ảnh hưởng đến hiệu suất trang web của bạn. Việc sử dụng tài nguyên cũng phụ thuộc nhiều vào lưu lượng truy cập trang web của bạn:

Bạn có thể đề nghị cấu hình phần cứng cho quá trình

**Ảo hóa**

Các nền tảng ảo hóa sau được hỗ trợ
 * VMware
 * XEN
 * OpenVZ
 * KVM
 * Hyper-V
 * LXC/LXD
WordOps cũng tương tích với Ubuntu chạy trên Windows Linux subsystem (WSL)


### Cách sử dụng 
`wo site create  [<site_name>] [options]`

**Trang web cơ bản**

**Trang web HTML**

Để tạo trang web html đơn giản, sử dụng lệnh 

`wo site create site.tld --html`

**Trang web PHP**

Để tạo trang web php đơn giản không có cơ sở dữ liệu, hãy sử dụng lệnh

`wo site create site.tld --php`

**Trang web PHP + MySQL**

Để tạo trang web php đơn giản với cơ sở dữ liệu, hãy sử dụng lệnh này

`wo site create site.tld --mysql`

> Lưu ý : chúng ta có thể tìm thấy chi tiết cơ sở dữ liệu MySQL trong `/var/www/site.tld/wo-config.php`

**Trang web proxy**

Để tạo trang web với cấu hình Proxy, bạn có thể sử dụng --proxy trong khi tạo trang web

`wo site create site.tld --proxy=127.0.0.1:3000`

Điều này sẽ tạo ra trang web proxy.tld với đích proxy là 127.0.0.1:3000. Cổng là tùy chọn. Cổng mặc định: 80

**WordPress**

Sau đây là các loại trang web WordPress bạn có thể tạo trang web dựa trên cơ chế bộ đệm

Trang web WordPress chuẩn

`wo site create site.tld --wp`

Trang web WordPress + Nginx fastcgi_cache

`wo site create site.tld --wpfc`

Trang web WordPress + Redis cache

`wo site create site.tld --wpsc`

Trang web WordPress + WP-super-cache

`wo site create site.tld --wpsc`

Trang web WordPress + bộ đệm WP-Rocket

`wo site create site.tld --wprocket`

Trang web WordPress + Trình tạo bộ đệm

`wo site create site.tld --wpce`

Kích hoạt trình chặn xấu Ultimate Nginx trên trang web mới

`wo site create site.tld --ngxblocker`

### Cấu trúc wordops
**Thư mục wordops**
|Path|Description|
|-|-|
|/etc/wo|Cấu hình chung|
|/var/lib/wo/dbase.db|Cơ sở dữ liệu trang web WordOps|
|/var/lib/wo/tmp|Thư mục tmp|
|usr/lib/wo/templates|Mẫu WordOps|


## Command
 * wo [options] {arguments}

**Options**

 * --version: hiển thị phiên bản đang sử dụng
 * info : hiển thị các thông tin như Nginx, PHP, MySQL
 * **log** : wo log (gzip| reset|show) [tên domain] {arguments}
    * gzip : nén file nhật ký
    * reset : xóa nhật ký tệp
    * show : hiển thị các file nhật ký
    
    * **Arguments**
       * -all : reser all logs file
       * --nginx : reset nginx error logs file
       * --php : reset PHP error logs file
       * --fpm : reset PHP-FPM slow logs file
       * --mysql : reset MySQL logs file
       * --wp : reset site specific WordPress logs file 
       * --access : reset Nginx access log file

    
 * **stack** :
    * **install** : cài đặt tất cả các gói phụ trợ WordOps hoặc cài từng phần riêng biệt bằng cách thêm các gói phụ trợ
       * --mysql
       * --nginx
       * --php
       * --phpmyadmin
    * **conmand** : wo stack install --nginx --php
       * remove : xóa tất cả gói phụ trợ hoặc xóa các gói k cần thiết bằng cách thêm gói phụ trợ
       * purge : gỡ cài đặt và lọc bỏ tất cả các gói hoặc các gói chỉ định
       * upgrade : cập nhật tất cả các gói hoặc các gói chỉ định
       * status : trạng thái tất cả các gọi hoặc các gói chỉ định
       * stop : dừng dịch vụ tất cả các gọi hoặc các gói chỉ định
       * reload : reload tất cả các gói hoặc các chỉ định
       * restart : restart tất cả các gói hoặc cái gói chỉ định


 * **site** :
    * cd [domain] : cd đến thư mục web
    * list --enabled/--disabled : liệt kê danh sách các site đang được kích hoạt/ chưa được kích hoạt  
    * info [domain] : thông tin domain, user/pass
    * show : thông tin các file cấu hình và các cấu hình gọi vào site
    * enable / disable : kích hoạt hoặc tạm ngưng dịch vụ của trang web
    * edit : sửa các file cấu hình của site muốn chọn

 * **site create/update example.com** : tạo hoặc update một site
    
    * --html : tạo hoặc update site --html
    * --php73
    * --mysql : tạo một website PHP + MySQL
    * --wpfc : tạo trang web WordPress với Nginx fastcgi_cache
 
 * **site delete example.com**
    
    * --db : xóa database của trang web
    * --files : xóa trang web chính
    * --no-prompt : không cần xác nhận khi xóa

 * **secure [--auth | --port| --ip | --ssh| --sshport]**
    
    * --auth : cập nhật thông tin xác thực của HTTP
    * --port : thay đổi port WordOps Admin hiện tại là 22222
    * --ip
    * --ssh : bảo mật ssh cứng. Tạo khóa ssh RSA
    * --sshport : thay đổi port ssh mặc định là 22
   