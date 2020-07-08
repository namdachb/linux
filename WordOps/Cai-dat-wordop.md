# Cài đặt WordOps trên Ubuntu 18.04

## Tài nguyên cần thiết
**Tối thiểu**

WordOps có thể cài đặt trên các thiết bị cấp thấp như Raspberry PI với yêu cầu tối thiểu là:
 * 100 MB dung lượng trống
 * RAM 512MB

Network : Có ít nhất 1 interface với IP public
 * ens3 : 103.101.161.199
 * ens4 : 10.10.35.114

## Cài đặt
### 1. Cấu hình trỏ domain cho IP public
Đầu tiên, khi có IP public chúng ta cần phải trỏ IP cho domain của mình

Mình sẽ trỏ IP 103.101.161.199 cho domain wo.namdac.xyz .Sau đó trỏ IP cho doamin, bằng câu lênh `nslookup <domain>` để kiểm tra

```
thuctap@lab-u18-srv1:~$ nslookup wo.namdac.xyz
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   wo.namdac.xyz
Address: 103.101.161.199
```

### 2. Cài đặt trang web

### 2.1 Tải về các phụ thuộc
Để làm việc với WordOps rất đơn giản vì đã được cung cấp 1 tập lệnh cài đặt để cài đặt các phụ thuộc cần thiết, chạy câu lệnh sau để cài đặt

`wget -qO wo wops.cc && sudo bash wo`

![Imgur](https://i.imgur.com/FexqIO1.png)

  1. Nhập vào tên của bạn
  2. Nhập địa chỉ email của bạn

### 2.2 Kích hoạt bash_completion
Để bật tự động hoàn thành các lệnh WordOps, hãy chạy lệnh sau sau khi cài đặt WordOps

`source /etc/bash_completion.d/wo_auto.rc`

### 2.3 Cài đặt WordOps stacks 
Bạn có thể lựa chọn để cài đặt sẵn danh sách các thành phần WordOps (Nginx,php,mysql,...) hoặc không cài và đến khi thiết lập trang web cần các phụ thuộc nào thì WordOps sẽ tự động cài thành phần đó

`wo stack install`

Chờ cho đến khi lệnh chạy xong, WordOps sẽ cài đặt các thành phần như sau

![Imgur](https://i.imgur.com/EvHD8fn.png)

  1.Tài khoản authen để truy cập web
  2.Password authen để truy cập web

### 2.4 Đặt lại tài khoản và mật khẩu authen khi truy cập web
Chạy lệnh sau để đặt lại tài khoản và mật khẩu để truy cập web

`wo secure --auth`

![Imgur](https://i.imgur.com/CMrPD47.png)

Sau khi chạy lệnh , chúng ta sẽ được nhắc nhập vào user và password sẽ sử dụng để truy cập web monitoring WordOps:
  
  1. Nhập vào user
  2. Nhập password

Sau khi nhập vào user vào password, nếu muốn truy cập web monitoring, chuyển đến trình duyệt và điều hướng đến link sau : `https://YOUR.SERVER.IP:22222`

## Tạo trang web
### 1. Tạo trang web

### 2. Tạo một trang wordpress
Để tạo 1 trang wordpress, chúng ta cần chạy câu lệnh sau

`wo site create wo.namdac.xyz --wp`

Sau khi chạy lệnh trên, WordOps sẽ tạo 1 trang wordpress với domain là `wo.namdac.xyz`. Sau khi lệnh cài đătk xong sẽ có tài khoản và mật khẩu sử dụng để truy cập trang quản trị của wordpress

![Imgur](https://i.imgur.com/gK06jwL.png)

  1. User sử dụng để đăng nhập vào trang quản trị của WordPress
  2. Password để đăng nhập trang quản trị WordPress

Sau đó truy cập vào url với tên miền wo.namdac.xyz để kiểm tra 

![Imgur](https://i.imgur.com/iT6iiDo.png)

ta thấy rằng đã có 1 trang wordpress được tạo với tên miền trên

Ngoài ra nếu muốn chúng ta cài wordpress với Let's Encrypt, hãy sử dụng lệnh sau

`wo site create wo.namdac.xyz --wp -le`

### 3. Tạo các trang web khác
Nếu không muốn cài wordpress, chúng ta cũng có thể sử dụng các tùy chọn của WordOps để cài các trang web cơ bản như
 
 * Tạo trang web html

`wo site create html.namdac.xyz.com --htm`

Thêm nội dung vào web

`vi /var/www/html.namdac.xyz/htdocs/index.html`

Kiểm tra 

![Imgur](https://i.imgur.com/YbOqkyN.png)

 * Tạo trang web PHP

`wo site create php.namdac.xyz --php`

 * Tạo trang web PHP + MySQL

`wo site create web.namdac.xyz --mysql`

### 4. Cập nhật và xem thông tin trang web
**Xem thông tin trang web**

Để biết các tùy chọn khi sử dụng lệnh `wo` , ta sử dụng -help hoặc -h :

`wo site -h`

Để liệt các web site được tạo và quản lý bởi WordOps, ta sử dụng

`wo site list`

sau khi chạy lệnh sẽ hiển thị ra danh sách các web site đã được tạo

![Imgur](https://i.imgur.com/KQj5bZ8.png)

Muốn xem thông tin chi tiết của 1 web site, ta sử dụng site info

`wo site info wo.namdac.xyz`

![Imgur](https://i.imgur.com/5CojGFZ.png)

**Cập nhật trang web**

Nếu trước đó bạn đã tạo 1 trang wordpress với WordOps mà chưa có let's encript, bạn có thể sử dụng lệnh sau để cập nhật chứng chỉ ssl cho site như sau

`wo site update wo.namdac.xyz -le`

![Imgur](https://i.imgur.com/Me3oQiU.png)

sau khi tạo ssl sẽ có thời hạn 80 ngày, nhưng tất cả các chứng chỉ được tự động gia hạn 60 ngày bởi `acme.sh` bằng cách sử dụng cronjob

Khi ta tạm thời không có nhu cầu sử dụng trang web nữa, ta có thể vô hiệu hóa trang web như sau

`wo site disable html.namdac.xyz`

khi đó trang web sẽ bị vô hiệu hóa và chuyển về trang mặc định của nginx

Hoặc có thể enable lại trang web nếu tiếp tục có nhu cầu sử dụng

`wo site enable wo.namdac.xyz`

Muốn xóa 1 trang web ta sử dụng tùy chọn delete

`wo site delete html.namdac.xyz`

### 5. Truy cập trang web monitoring
Sau khi cài đặt trang web, để theo dõi tải cũng như hiệu năng của trang web ta có thể truy cập theo địa chỉ `https://your.server.ip:22222` để theo dõi tải cũng như các thông số giám sát của web

Mình sẽ thử truy cập trang web monitoring `https://103.101.161.199:22222` của mình để kiểm tra:

Nhập vào tài khoản và mật khẩu authen đã đặt lại ở trên

Sau đó bạn sẽ được điều hướng đến trang web để view các thông số giám sát trang web của bạn

