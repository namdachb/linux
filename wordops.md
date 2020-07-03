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

**Cổng**

|Service|Port|Inbound(trong nước)|Outbound(ra nước ngoài)|Notes|
|-|-|-|-|-|
|SSH|22|x|x|SSH mặc định hoặc cổng tùy chỉnh|
|HTTP|80|x|x|Nginx lắng nghe trên cổng 80|
|HTTPS|443|x|x|Nginx lắng trên cổng 443|
|WordOps Backend|22222|x|x|WordOps backend có sẵn trên cổng 22222 và được bảo vệ bằng mật khẩu|
|GnuPG|1137| |x|Cần thiết để nhập các khóa GPG kho lưu trữ|


## Cài đặt wordops trên ubuntu 18.04

### Cài đặt trang web
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

![Imgur](https://i.imgur.com/9LyuOU9.png)
 
 * Tài khoản authen để truy cập web
 * Password authen để truy cập web

#### Đặt lại tài khoản và mật khẩu authen khi truy cập web
Chạy lệnh sau để đặt lại tài khoản và mật khẩu để truy cập web

`wo secure --auth`

```
root@namdac:~# wo secure --auth
Provide HTTP authentication user name [Nguyen The Nam] :user
Provide HTTP authentication password [NXUzmMBR5Tvp0bxO7ePQmnhX] :
```

Sau khi chạy lệnh, bạn sẽ được nhắc nhập vào user và password và sử dụng để truy cập vào web monitoring WordOps:
 * Nhập vào user
 * Nhập password

Sau khi nhập vào user và password, nếu muốn truy cập web monitoring, chuyển đến trình duyệt và điều hướng đến link sau: `https://your.server.ip:22222`

### Tạo trang web
#### Tạo 1 trang wordpress
Để tạo 1 trang wordpress, bạn chỉ cần chạy câu lệnh sau:

`wo site create w.namdac.xyz --wp`

Sau khi chạy lệnh trên, WordOps sẽ tạo 1 trang wordpress với domain là `w.namdac.xyz`. Sau khi lệnh cài đặt xong sẽ có tài khoản và mật khẩu sử dụng để truy cập trang quản trị của wordpress

![Imgur](https://i.imgur.com/ebz2Kyd.png)

 * User sử dụng để đăng nhập vào trang quản trị của WordPress
 * Password để đăng nhập trang quản trị WordPress

Sau đó truy cập vào url với tên miền **w.namdac.xyz** để kiểm tra

![Imgur](https://i.imgur.com/mY48szv.png)

ta thấy rằng đã có một trang wordpress được tạo với tên miền trên
 