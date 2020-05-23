## 1. NTP là gì?
NTP (Network Time Protocol) là một thuật toán phần mềm giữ cho các máy tính và các thiết bị công nghệ khác nhau có thể đồng bộ hóa thời gian với nhau

## 2. Các thành phần NTP
NTP server và NTP client
 * NTP server : máy chủ NTP hay máy chủ thời gian là các thuật ngữ cùng mô tả một khái niệm, một thiết bị được sử dụng để nhận biết yêu cầu đồng bộ thời gian và phân phối tín hiệu thông tin thời gian
 * NTP client : nhận được được gọi tin từ NTP server rồi tính toán toán độ trễ, dựa vào thời gian mà nó nhận được cùng với độ trễ của đường truyền, nếu lệnh thời gian NTP client sẽ tự động set lại thời gian của nó

## 3. Các mô hình triển khai 
Có 2 mô hình:
 * Mô hình 1 : Cấu hình 1 server đồng bộ hóa với giờ quốc tế (thích hợp để cấu hình máy đơn lẻ)

 * Mô hình 2 : Cấu hình 2 server: server và client (thích hợp để cấu hình từ 2 máy trở lên)

## 4. Cài đặt mô hình 
### Mô hình 1:
![Imgur](https://i.imgur.com/VA8IESW.png)

 * Chuẩn bị 1 server (centos 7)
 * Có kết nối internet
 * Đăng nhâp với user `root`
 * Cấu hình timezone
 * Kiểm tra timezone sau khi cài đặt
 * Cấu hình firewall
 * Cấu hình Disable Selinux
 * Cài đặt Chrony
 * Khởi động dịch vụ
 * Sửa file cấu hình `/etc/chrony.conf
 * Restart lại dịch vụ
### Mô hình 2: 
* Quy hoạch mô hình
![Imgur](https://i.imgur.com/e1quje2.png)

* Mô hình ip planning
![Imgur](https://i.imgur.com/gQ4w5qr.png)

Bước 1:
 * Sử dụng 2 server cho mô hình (server,client)
 * Cent0S 7
 * Có kết nối Internet
 * Đăng nhập với user `root`

Bước 2:
 * Cấu hình timezone
 * Kiểm tra timezone sau khi cài đặt
 * Cấu hình allow Firewall
 * Cấu hình disable SElinux

Bước 3:
 * Cài đặt `Chrony` trên cả 2 máy
 * Tiến hành start Chrony và cho phép khởi động cùng hệ thống
 * Kiểm tra dịch vụ đang hoạt động
 * Mặc định cấu hình `Chrony` nằm ở `/etc/chrony.conf

Bước 4:
 * Cấu hình Chrony làm NTP Server

Bước 5:
 * Cấu hình Chrony Client

Bước 6: 1 vài câu lệnh bổ sung
 * Kiểm tra verify kết nối `chronyc tracking`
 * Stop Chrony `systemctl stop chronyd`